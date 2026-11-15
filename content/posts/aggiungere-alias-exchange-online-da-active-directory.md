---
title: Aggiungere un Alias su Exchange Online da Active Directory in Ambiente Ibrido
date: '2024-10-10'
lastmod: '2024-10-10T22:44:18+02:00'
description: In un ambiente ibrido con Microsoft 365 e Active Directory, gestire alias di posta può risultare complesso. Questo articolo spiega come aggiungere un alias a un account Exchange Online sincronizzato da AD.
summary: In un ambiente ibrido con Microsoft 365 e Active Directory, gestire alias di posta può risultare complesso. Questo articolo spiega come aggiungere un alias a un account Exchange Online sincronizzato da AD.
url: /posts/aggiungere-alias-exchange-online-da-active-directory/
categories:
- Microsoft 365 e Azure
tags:
- m365
- exchange
featureimage: assets/2024-10-10/image4.png
images:
- /assets/2024-10-10/image4.png
---

Se si usa **Microsoft 365** in modalità ibrida sincronizzando gli utenti da **Active Directory (AD)** utilizzando **Microsoft Entra Connect**, una delle prime cose da imparare è che “quello che nasce on-premise va gestito on-premise”.
Qualche giorno fa stavo appunto lavorando in un’organizzazione con AD locale e account di posta **Exchange Online cloud only** ed aggiungendo un alias di posta elettronica sul portale Admin M365 ottengo un bel messaggio di errore.

<img src="/assets/2024-10-10/image2.png" alt="Errore in Exchange Online" width="500" loading="lazy">

Mi ricordo dell’insegnamento; dovevo creare l’alias on-premise.

## Come creare alias Exchange Online da Active Directory
Apri **Active Directory Users and Computers** (ADUC) abilitando la visualizzazione avanzata dal menu **view**.  
All’interno dell’utente spostati sul tab **Attribute Editor** e trova **proxyAddresses** tra gli i valori presenti.  

<img src="/assets/2024-10-10/image3.png" alt="proxyAddresses in ADUC" width="600" loading="lazy">

Ora aggiungi l’alias facendo attenzione a come lo scrivi:

- utilizza il formato **SMTP:[indirizzo-principale@dominio.xx](mailto:indirozzo-principale@daominio.xx)** per l’indirizzo principale (SMTP scritto in maiuscolo)  
- utilizza il formato **smtp:[alias@dominio.xx](mailto:alias@dominio.xx)** per l’alias (smtp scritto in minuscolo)

<img src="/assets/2024-10-10/image1.png" alt="Aggiunta dell'alias" width="300" loading="lazy">

Attendi che l’utenza aggiornata venga sincronizzata dall'on-prem in M365 e il tuo alias sarà attivo.

## Bibliografia
[Netwrix](https://blog.netwrix.com/2017/02/02/using-ad-to-add-an-alias-to-an-office-three-six-five-email-account/)