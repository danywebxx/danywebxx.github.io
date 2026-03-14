---
title: "Hyper-V e Ubuntu 24.04: abilitare la Enhanced Session Mode per il resize della finestra"
date: 2026-03-12
categories: [blogging, tutorial]
tags: [appunti, hyper-v, microsoft, linux, ubuntu, rdp]
image:
  path: /assets/2026-03-12/copertina.png
  alt: "Ubuntu 24.04 in Enhanced Session Mode su Hyper-V"
---

Se hai installato Ubuntu 24.04 su Hyper-V usando la funzione `Creazione Rapida`, ti sarai subito scontrato con un limite fastidioso: la finestra è bloccata a una risoluzione fissa — spesso 1024×768 — e non puoi ridimensionarla trascinando i bordi come con qualsiasi altra finestra di Windows.

Il sottoscritto ci è cascato esattamente come era già successo con [il disco da 13GB di default](https://danyweb.it/posts/ubuntu-hyperV-allargare-disco/) — Hyper-V semplifica la creazione della VM ma lascia all'utente qualche configurazione non ovvia da scoprire a posteriori.

La soluzione è abilitare la **Enhanced Session Mode**, che sostituisce la connessione grafica base con un protocollo RDP (tramite `xrdp`), portando con sé:

- ✅ Ridimensionamento dinamico della finestra
- ✅ Risoluzione personalizzabile a ogni avvio
- ✅ Copia/incolla bidirezionale tra host e VM
- ✅ Condivisione di cartelle e dispositivi USB

> 📸 *[Screenshot: finestra Ubuntu bloccata con risoluzione fissa in basic session]*

---

## Prerequisiti

- Windows 10/11 **Pro, Enterprise o Education** — Hyper-V non è disponibile su Windows Home
- Ubuntu 24.04 già installato e funzionante come VM su Hyper-V
- **Login automatico disabilitato** su Ubuntu: con xrdp è necessario inserire le credenziali manualmente. Se lo hai attivo, disattivalo prima da *Impostazioni → Utenti → Accesso automatico*

---

## Passo 1 — Abilitare Enhanced Session sul lato Windows

Prima di tutto bisogna istruire Hyper-V a permettere le sessioni enhanced.

1. Apri **Hyper-V Manager** (cercalo nel menu Start)
2. Click destro sul **nome del tuo PC** nel pannello sinistro → **Impostazioni Hyper-V**
3. Sotto **Server** → **Enhanced Session Mode Policy** → spunta **"Allow enhanced session mode"**
4. Sotto **User** → **Enhanced Session Mode** → spunta **"Use enhanced session mode"**
5. Clicca **OK**

> 📸 *[Screenshot: pannello Hyper-V Settings con Enhanced Session Mode attivata]*

---

## Passo 2 — Installare xrdp su Ubuntu

Ubuntu 24.04 non supporta la Enhanced Session di default: il motivo è che Hyper-V usa questa funzionalità tramite **RDP over vsock**, e serve il server `xrdp` configurato correttamente sul guest.

Il modo più rapido è usare lo script del progetto **linux-vm-tools** (fork community, dal momento che Microsoft ha archiviato il repository ufficiale dopo Ubuntu 18.04).

Avvia la VM in modalità normale (basic session) e apri un terminale:

```bash
# Aggiorna il sistema e installa git
sudo apt update && sudo apt upgrade -y
sudo apt install -y git

# Clona il repository
git clone https://github.com/Hinara/linux-vm-tools.git
cd linux-vm-tools/ubuntu/24.04

# Rendi lo script eseguibile e avvialo
chmod +x install.sh
sudo ./install.sh
```

> ⚠️ **Nota:** se lo script si ferma comunicando che è necessario un riavvio (ad esempio per un aggiornamento del kernel), riavvia la VM e poi riesegui lo script una seconda volta per completare l'installazione.

```bash
# Seconda esecuzione se richiesta dopo il reboot
cd linux-vm-tools/ubuntu/24.04
sudo ./install.sh
```

> 📸 *[Screenshot: terminale Ubuntu con script completato con successo]*

---

## Passo 3 — Configurare HvSocket da PowerShell

Dopo che lo script ha terminato, spegni la VM e torna su **Windows**. Apri **PowerShell come Amministratore** ed esegui:

```powershell
Set-VM -VMName "NomeDellatuaVM" -EnhancedSessionTransportType HvSocket
```

> 💡 Sostituisci `NomeDellatuaVM` con il nome esatto che hai assegnato alla VM in Hyper-V. Se il nome contiene spazi, mantienilo tra virgolette come nell'esempio.

Questo comando abilita il trasporto **vsock** di Hyper-V — necessario perché la sessione enhanced su Linux funziona tramite socket virtuale anziché via rete TCP come avviene con le VM Windows.

---

## Passo 4 — Connettersi in Enhanced Mode

1. **Spegni** completamente la VM (non sospendere, proprio spegni)
2. Riavviala da Hyper-V Manager → **Connetti**
3. Comparirà una nuova finestra di dialogo con uno slider per scegliere la **risoluzione** — seleziona quella del tuo monitor
4. Clicca **Connetti** e accedi con le tue credenziali Ubuntu

> 📸 *[Screenshot: dialog di connessione Hyper-V con slider per la risoluzione]*

Da questo momento la finestra sarà **liberamente ridimensionabile** trascinandone i bordi, esattamente come qualsiasi altra finestra in Windows.

---

## Risoluzione problemi

### La sessione enhanced non parte — torna sempre in basic session

Il problema più frequente su Ubuntu 24.04 è **Wayland attivo**. Xrdp non è compatibile con Wayland, quindi la sessione RDP non riesce a collegarsi e Hyper-V ricade sulla modalità base.

Soluzione:
1. Alla schermata di login Ubuntu, clicca sull'icona **⚙️ ingranaggio** in basso a destra
2. Seleziona **"Ubuntu on Xorg"**
3. Accedi normalmente

### Il comando PowerShell restituisce un errore sul parametro

Se `-EnhancedSessionTransportType` non viene riconosciuto, verifica di avere Windows 10 versione 1803 o successiva — e PowerShell aggiornato. In alternativa puoi impostare il parametro tramite Hyper-V Manager: click destro sulla VM → **Impostazioni** → **Servizi di integrazione** → verifica che tutti i servizi siano abilitati.

### La risoluzione non si aggiorna ridimensionando la finestra

La risoluzione viene negoziata **all'inizio della sessione RDP**, non in tempo reale. Chiudi la connessione e riconnetti: ti verrà proposto nuovamente il dialog con lo slider per scegliere la dimensione.

---

## Conclusione

Una volta configurata, la Enhanced Session Mode trasforma Ubuntu su Hyper-V in un ambiente decisamente più comodo: finestra ridimensionabile, clipboard condivisa e risoluzione personalizzata a ogni sessione. L'unico passaggio non scontato è l'installazione manuale di `xrdp` — operazione che la `Creazione Rapida` di Hyper-V non fa per te, a differenza di come gestisce il resto del setup.

Se stai allestendo la tua VM Ubuntu da zero e non l'hai ancora fatto, ti consiglio di partire anche da qui: [Allargare il disco di Ubuntu creato da Hyper-V](https://danyweb.it/posts/ubuntu-hyperV-allargare-disco/) — il disco da 13GB di default si esaurisce molto prima di quanto si pensi.

---

### Fonti

- [Hinara/linux-vm-tools — GitHub](https://github.com/Hinara/linux-vm-tools)
- [Hyper-V Enhanced Session Mode — Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/learn-more/use-local-resources-on-hyper-v-virtual-machine-with-vmconnect)
