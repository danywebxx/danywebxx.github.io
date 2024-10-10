---
title: Aggiungere un Alias su Exchange Online da Active Directory in Ambiente Ibrido
date: 2024-10-10
categories: [blogging, tutorial]
tags: [microsoft, m365, exchange]
description:  In un ambiente ibrido con Microsoft 365 e Active Directory, gestire alias di posta può risultare complesso. Questo articolo spiega come aggiungere un alias a un account Exchange Online sincronizzato da AD, evitando errori comuni.
image:
     path: /assets/2024-10-10/image4.png
---
Se si usa **Microsoft 365** in modalità ibrida sincronizzando gli utenti da **Active Directory (AD)** utilizzando **Microsoft Entra Connect**, una delle prime cose da imparare è che “quello che nasce on-premise va gestito on-premise”.

Qualche giorno fa stavo lavorando in un’organizzazione con AD locale e account di posta **Exchange Online cloud only** e aggiungendo un alias di posta elettronica sul portale Admin M365 ottengo un bel messaggio di errore.

![Errore in Exchange Online](/assets/2024-10-10/image2.png)

Mi ricordo dell’insegnamento; dovevo creare l’alias on-premise.

## Come creare alias Exchange Online da Active Directory
Apri **Active Directory Users and Computers** (ADUC) abilitando la visualizzazione avanzata dal menu **view**.  
All’interno dell’utente spostati sul tab **Attribute Editor** e trova **proxyAddresses** tra gli i valori presenti.  

![proxyAddresses in ADUC](/assets/2024-10-10/image3.png)

Ora aggiungi l’alias facendo attenzione a come lo scrivi:

- utilizza il formato **SMTP:[indirizzo-principale@daominio.xx](mailto:indirozzo-principale@daominio.xx)** per l’indirizzo principale (SMTP scritto in maiuscolo)  
- utilizza il formato **smtp:[alias@dominio.xx](mailto:alias@dominio.xx)** per l’alias (smtp scritto in minuscolo)

![Aggiunta dell'alias](/assets/2024-10-10/image1.png)

Attendi che l’utenza venga sincronizzata da Entra Connect con M365 e il tuo alias sarà attivo.

## Bibliografia
[Netwrix](https://blog.netwrix.com/2017/02/02/using-ad-to-add-an-alias-to-an-office-three-six-five-email-account/)