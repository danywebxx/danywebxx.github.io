---
title: Bloccare la migrazione al nuovo Outlook tramite GPO - easy-way
date: 2025-01-03
categories: [blogging, tutorial]
tags: [microsoft, outlook]
description: In questo articolo ti guiderò su come bloccare la migrazione al nuovo Outlook utilizzando le Group Policy e delle chiavi di registro. 
image:
     path: /assets/2025-01-03/image03.png
---

Se sei un amministratore IT e vuoi impedire la migrazione al **nuovo Outlook** nella tua organizzazione, la via più semplice è utilizzando una Group Policy (**GPO**) per distribuire una chiave di registro che blocca la migrazione. L'utilizzo della "vecchia" versione di Outlook garantisce la compatibilità con le configurazioni aziendali esistenti.

Partendo da Gennaio 2025 gli utenti con le seguenti versioni di Microsoft 365 saranno automaticamente migrati alla nuova versione di Outlook:
- Microsoft 365 Business Standard - 6 Gennaio 2025
- Microsoft 365 Business Premium - 6 Gennaio 2025
- Microsoft 365 Enterprise - 1 Aprile 2026

Ecco una guida passo-passo per configurare una GPO che disabilita il paggaggio alla nuova versione di Outlook:

## Passaggi per configurare la GPO
Come accennato nell'introduzione dovrai creare un GPO che andrà a creare/modificare delle chiavi di registro sui computer dell'organizzazione. 

![Riepilogo della GPO](/assets/2025-01-03/image01.png) 

Le chiavi di registro che dovrai creare - sono facolative e personalizzabili - le riporto sotto. In bibliografia andrò ad inserire tutti gli articoli di riferimento utilizzati perchè la gestione di questa tipologia di aggiormanento è abbastanza complessa: 

### Disabilitare la migrazione al nuovo Outlook
Per disabilitare la migrazione al nuovo Outlook dovrai impostare questa chiave a "0":

```
[HKEY_CURRENT_USER\Software\Policies\Microsoft\office\16.0\outlook\preferences]
"NewOutlookMigrationUserSetting"=dword:00000000
```
![Disabilare migrazione di Outlook](/assets/2025-01-03/image02.png) 

### Nascondere il pulsante di switch
Se preferisci nascondere il pulsante che permette agli utenti di effettuare nuovamente lo switch alla nuova versione di Outlook:

```
[HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Outlook\Options\General]
"HideNewOutlookToggle"=dword:00000001
```
- Valore "1" nasconde il pulsante di switch 
- Valore "0" visualizza il pulsante 

### Ripristinare il vecchio Outlook
Se volete ripristinare agli utenti che hanno effettuato lo switch la precedente versione di Outlook:
```
[HKEY_CURRENT_USER\Software\Policies\Microsoft\office\16.0\outlook\preferences]
"UseNewOutlook"=dword:00000000
```
- Valore "0" vecchia versione
- Valore "1" nuova versione

## Altre possibilità di gestione
Puoi gestire il passaggio al nuovo Outlook anche in altri modi:
- Group Policy (scaricando la versione aggiornata dei template ADMX)
- Cloud Policy
- Intune (Admin-Controlled Migration to New Outlook)

## Bibliografia
- [Documentazione ufficiale di Microsoft](https://learn.microsoft.com/it-it/microsoft-365-apps/outlook/get-started/control-install)
- [Il maestro Ali Tajran su Linkedin](https://www.linkedin.com/posts/alitajran_microsoft365-outlook-activity-7274416800962908162-UsLa/?utm_medium=ios_app&utm_source=screenshot_social_share&utm_campaign=copy_link)
- [4sysops.com](https://4sysops.com/archives/block-migration-to-new-outlook-with-group-policy/)