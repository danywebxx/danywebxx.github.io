---
title: Retention policy e Archivio In-Place non funzionano in Exchange Online
date: 2024-09-23
categories: [blogging, tutorial]
tags: [microsoft, m365, exchange]
description:  Se la retention policy che dovrebbe spostare i messaggi di posta elettronica nell'archivio In-Place di Exchange Online non funziona,  in questo articolo ti racconto come risolvere il problema.
image:
     path: /assets/2024-10-05/image2.png
---

Gli archivi di posta degli utenti in Exchange Online crescono ed è un dato di fatto; 50 o 100 GB di spazio di archiviazione possono sembrare tanti ma c’è sempre chi ha la capacità di raggiungere e superare i propri limiti.  
Concedendo altro spazio può sembrare semplice: si abilita l’**Archivio Online (in-place archive)** di Exchange e si applica una **policy di retention** per spostare automaticamente i messaggi più vecchi di XXX giorni nell’archivio. Il lavoro per il sysadmin è finito\!   
Qui il diavolo ci mette lo zampino\! Dopo poco tempo l’utente torna con la posta bloccata perché ha raggiunto i limiti di utilizzo e il sysadmin passa dalla paura alla disperazione perchè la retention policy non sta funzionando.

In rete sul tema si trova poco e le informazioni sono frammentate ma cun una domanda si risolve facilmente il problema: sono stati importati PST sulla casella del malcapitato con il servizio di importazione in Exchange Online?  
Se la risposta è affermativa non c’è bisogno di disperarsi, importando i PST si è abilitata la **Retention Hold** sulla casella che impedisce alle policy di retention di spostare i messaggi nell’archivio online.

## Come verificare se la Retention Hold è attiva?

Collegati a Exchange Online utilizzando Powershell:
 
```powershell
Connect-ExchangeOnline
```

Ora controlla i parametri della casella e-mail dell’utente:  

```powershell
Get-Mailbox -Identity username|fl \*hold\*  
```

![RetentionHoldEnabled](/assets/2024-10-05/image1.png)

Se la **RetentionHoldEnabled** è True allora la policy di retention non potrà spostare i messaggi in archivio e la dovrai disattivare:

```powershell
Set-Mailbox "user@domain.com" -RetentionHoldEnabled $false
```

Ora che hai disattivato la Retention Hold puoi velocizzare lo spostamento della posta in archivio forzando l’agente di archiviazione:

```powershell
Start-ManagedFolderAssistant -identity "user@domain.com"
```

## Bibliografia

- [Lucas Hadberg](https://hadberg.eu/exchange-online-retention-and-archiving-not-working/)  
- [Microsoft Techcommunity](https://techcommunity.microsoft.com/t5/microsoft-365/o365-online-archiving-not-working/m-p/96876)