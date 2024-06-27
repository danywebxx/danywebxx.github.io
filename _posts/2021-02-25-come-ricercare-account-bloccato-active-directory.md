---
title: Come ricercare l’origine di un account bloccato in Active Directory
date: 2021-02-25
categories: [blogging, tutorial]
tags: [microsoft, Active Directory, password] 
description: Scopri come ricercare facilmente l'origine di un account bloccato in Active Directory ...
---

Hai ricevuto una notifica di blocco di un account e vuoi individuare da quale computer sia partito? In questo articolo viene spiegato come trovare l'origine di un blocco account in **Active Directory** (**Account Lockout**).

Password scordate, errori di battitura, distrazione o reali tentativi di intrusione sono alcuni dei motivi che fanno inserire troppe volte una password sbagliata provocando il blocco preventivo di un account in AD.

L'attività di ricerca e sblocco è una delle più frequenti per il responsabile IT aziendale e per le realtà in cui la sicurezza è prioritaria, è essenziale ricercare e verificare da dove proviene un blocco account per non subire brutte sorprese. Non bisogna mai riattivare senza aver prima accertato che si tratti di un falso allarme o di una minaccia effettiva.

## Ricercare l'ordine di un account bloccato

### 1. Auditing

Per una rete con Active Directory, i log chiari e correlati agli eventi che bloccano gli utenti sono essenziali. Per abilitarli su tutti i DC basta modificare la **Default Domain Controller Policy** nella Group Policy Management Console.

![Group Policy](/assets/2024-06-27/image1.png)

Il parametro da abilitare è l'**Audit User Account Management** reperibile in questo percorso:

**Computer configuration** > **Security Settings** > **Advanced Audit Policy Configuration** > **Audit Policies** > **Account Management**

![Dettaglio GPO](/assets/2024-06-27/image2.png)

In una finestra powershell amministrativa eseguiamo su tutti i DC il comando **gpupdate ** per aver la certezza che la policy venga subito applicata:

```powershell
gpupdate /force
```

### 2. Verifica del PDC

Se si hanno più domain controller nella rete bisogna controllare quale svolte il ruolo di PDC. Infatti, il PDC registra tutti i log degli account bloccati, mentre i domain controller base registrano solo le loro autenticazioni grazie alla policy abilitata. In una finestra powershell aperta come amministratore:

```powershell
get-addomain | select PDCEmulator
```
Il risultato del comando sarà il nome del server che svolge la funzione di PDC nella rete e sulla quale andranno eseguiti i comandi sottostanti.

### 3. Ricercare l'evento ID 4740

Qui cominciamo a leggere i log per scoprire il computer da cui è partito l'evento che ha bloccato l'account in questione.

In powershell con privilegi elevati usiamo questo comando per cercare nei log di Windows gli eventi con l'**ID 4740**:
```powershell
Get-WinEvent -FilterHashtable \@{logname='security'; id=4740}
```

Per analizzare l'account bloccato e il computer che ha provocato l'evento, si possono filtrare i risultati del comando:
```powershell
Get-WinEvent -FilterHashtable \@{logname='security'; id=4740} \| fl
```

![Dettaglio del log in Powershell](/assets/2024-06-27/image3.png)

Nell'immagine sopra ho mostrato il particolare di un log ID 4740 che contiene il campo Account name, l'utente bloccato, e il Caller Computer Name, il computer da cui probabilmente è stato causato l'evento di blocco.

Lo stesso criterio di ricerca può essere usato anche graficamente nel registro eventi del domain controller cercando, con un filtro specifico, l'ID 4740.

![Dettaglio del log nel Event Viewer](/assets/2024-06-27/image4.png)

## Conclusioni
Come hai potuto vedere, con un semplice comando PowerShell puoi recuperare tutte le informazioni necessarie per individuare la causa di un blocco account. Questo ti permette di risolvere il problema in modo rapido ed efficiente, senza dover esaminare manualmente i log di sicurezza. Ti consiglio di usare questo metodo ogni volta che devi affrontare una situazione simile.
Grazie per aver letto il mio articolo che spero possa averti aiutato a migliorare le tue competenze da sysadmin di Active Directory.
