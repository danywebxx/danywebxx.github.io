---
title: "Profwiz + Microsoft Entra Join: Fix del Bug UCPD nella Migrazione dei Profili"
date: 2026-03-10
categories: [blogging, tutorial]
tags: [windows, entra, profwiz, active-directory, microsoft, appunti]
description: "Bug UCPD blocca la migrazione profili con Profwiz su Entra ID? Ecco la procedura completa con script PowerShell per risolverlo in pochi minuti."
image:
     path: /assets/2026-03-10/20260310-copertina.jpg
---

Se stai eseguendo la **migrazione di profili Windows verso Microsoft Entra ID con Profwiz** e dopo il join noti comportamenti anomali — associazioni file errate, browser predefinito che non risponde, file `.pdf` che si aprono con l'app sbagliata — il problema ha un nome preciso: **UCPD**.

Un aggiornamento Microsoft ha introdotto un bug nel servizio **UCPD (User Choice Protection Driver)** che interferisce con le associazioni di file e URL durante la migrazione del profilo utente con Profwiz. In questo articolo trovi la causa, la spiegazione tecnica e la procedura completa per risolverlo — incluso uno script PowerShell pronto all'uso.

---

## Cos'è UCPD e Perché Blocca Profwiz

**UCPD** (User Choice Protection Driver) è un driver Windows introdotto da Microsoft per proteggere le scelte dell'utente sulle app predefinite, impedendo che software di terze parti le modifichino silenziosamente. Un meccanismo utile in condizioni normali, ma problematico durante la migrazione dei profili.

Durante la migrazione con Profwiz, il profilo utente viene associato a un nuovo **SID (Security Identifier)** generato da Entra ID. UCPD monitora attivamente le chiavi di registro relative alle associazioni file e URL: quando Profwiz tenta di riscrivere queste chiavi per il nuovo SID, UCPD le blocca o le corrompe, lasciando il profilo in uno stato inconsistente.

> **Sintomi tipici del bug UCPD + Profwiz**
> - Impossibilità di cambiare il browser predefinito
> - File `.pdf` che si aprono con app errate
> - Associazioni `http`/`https` che non vengono rispettate
> - Errori silenziosi durante la fase di migrazione Profwiz

---

## Come Risolvere: Procedura Passo per Passo

> ⚠️ **Prerequisito:** esegui tutte le operazioni con un account Amministratore locale. Assicurati di avere accesso a PowerShell con privilegi elevati e all'Editor del Registro di sistema.

### Fase 1 — Disabilitare UCPD

Apri **PowerShell come Amministratore** ed esegui i seguenti comandi per fermare il servizio UCPD e disabilitare il task schedulato correlato:

```powershell
# Disabilita il servizio UCPD (Start = 4 = Disabled)
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\UCPD" -Name "Start" -Value 4 -Force

# Disabilita il task schedulato UCPD velocity
Disable-ScheduledTask -TaskName '\Microsoft\Windows\AppxDeploymentClient\UCPD velocity'
```

**Riavvia il computer** per applicare le modifiche prima di procedere al passo successivo.

---

### Fase 2 — Eliminare le Chiavi di Registro Corrotte

Dopo il riavvio, apri l'**Editor del Registro di sistema** (`regedit.exe`) come Amministratore.

Individua il SID dell'utente coinvolto espandendo il nodo `HKEY_USERS`, quindi **elimina le seguenti chiavi** (se una chiave non è presente, passa alla successiva):

```
HKEY_USERS\<SID_utente>\Software\Microsoft\Windows\Shell\Associations\UrlAssociations\http
HKEY_USERS\<SID_utente>\Software\Microsoft\Windows\Shell\Associations\UrlAssociations\https
HKEY_USERS\<SID_utente>\Software\Microsoft\Windows\Shell\Associations\UrlAssociations\pdf
HKEY_USERS\<SID_utente>\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.pdf
```

> 💡 **Come trovare il SID dell'utente**
>
> Apri PowerShell e digita:
> ```powershell
> Get-LocalUser | Select-Object Name, SID
> ```
> In alternativa, naviga in `HKEY_USERS` con regedit e identifica il SID del profilo migrato (di solito inizia con `S-1-5-21-...`).

---

### Fase 3 — Riattivare UCPD

Una volta eliminate le chiavi corrotte, riattiva UCPD:

```powershell
# Riattiva il servizio UCPD (Start = 1 = Automatic)
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\UCPD" -Name "Start" -Value 1 -Force

# Riabilita il task schedulato UCPD velocity
Enable-ScheduledTask -TaskName '\Microsoft\Windows\AppxDeploymentClient\UCPD velocity'
```

---

## Perché Questo Fix Funziona

Disabilitando temporaneamente UCPD si rimuove il blocco che impedisce la corretta riscrittura delle chiavi di registro durante la migrazione. Le chiavi `UrlAssociations` e `FileExts` sono quelle che Windows usa per determinare quale app aprire per un determinato protocollo o tipo di file.

Eliminando le chiavi corrotte, Windows le ricreerà correttamente al successivo login utente, rispettando le impostazioni di default definite nel profilo migrato. Riattivare UCPD al termine garantisce che la protezione delle scelte utente torni operativa.

---

## Script PowerShell Completo per Automatizzare il Fix

Per velocizzare il processo su più macchine, puoi usare questo script riutilizzabile. Passagli il SID come parametro e gestirà tutto in automatico.

```powershell
# ============================================================
# Fix-UCPDProfileMigration.ps1
# Uso: .\Fix-UCPDProfileMigration.ps1 -UserSID "S-1-5-21-..."
# Eseguire come Amministratore dopo migrazione Profwiz
# ============================================================

param(
    [Parameter(Mandatory=$true)]
    [string]$UserSID
)

Write-Host "[1/4] Disabilitazione UCPD..." -ForegroundColor Yellow
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\UCPD" -Name "Start" -Value 4 -Force
Disable-ScheduledTask -TaskName '\Microsoft\Windows\AppxDeploymentClient\UCPD velocity'

Write-Host "[2/4] Pulizia chiavi registro per SID: $UserSID" -ForegroundColor Yellow
$keys = @(
    "HKEY_USERS\$UserSID\Software\Microsoft\Windows\Shell\Associations\UrlAssociations\http",
    "HKEY_USERS\$UserSID\Software\Microsoft\Windows\Shell\Associations\UrlAssociations\https",
    "HKEY_USERS\$UserSID\Software\Microsoft\Windows\Shell\Associations\UrlAssociations\pdf",
    "HKEY_USERS\$UserSID\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.pdf"
)

foreach ($key in $keys) {
    $regPath = "Registry::$key"
    if (Test-Path $regPath) {
        Remove-Item -Path $regPath -Recurse -Force
        Write-Host "  [OK] Eliminata: $key" -ForegroundColor Green
    } else {
        Write-Host "  [--] Non trovata (ok): $key" -ForegroundColor Gray
    }
}

Write-Host "[3/4] Riattivazione UCPD..." -ForegroundColor Yellow
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\UCPD" -Name "Start" -Value 1 -Force
Enable-ScheduledTask -TaskName '\Microsoft\Windows\AppxDeploymentClient\UCPD velocity'

Write-Host "[4/4] Completato. Riavviare il PC per applicare le modifiche." -ForegroundColor Cyan
```

---

## Conclusioni

Ammetto che ci ho messo un po' a capire cosa stesse succedendo — i sintomi non sono immediati da collegare a UCPD, soprattutto se si lavora su più macchine in sequenza e i problemi emergono solo dopo il primo login dell'utente.

Una volta identificata la causa, il fix è fortunatamente veloce. L'unica cosa a cui fare attenzione è non dimenticarsi di riattivare UCPD al termine: lasciarlo disabilitato espone il sistema a modifiche non autorizzate delle app predefinite.

Se stai lavorando su ambienti ibridi, potresti trovare utile anche [come disattivare il Seamless SSO in Microsoft Entra Connect]({% link _posts/2025-08-09-disabilitare-seamless-sso-entra-connect.md %}) e [come bloccare la migrazione al nuovo Outlook tramite GPO]({% link _posts/2025-01-03-bloccare-migrazione-nuovo-outlook-gpo.md %}).

---

> **Riferimento:** discussione tecnica originale dalla community sysadmin su [Reddit r/sysadmin](https://www.reddit.com/r/sysadmin/s/DXBoKCjpxW).
