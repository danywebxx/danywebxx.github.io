---
title: Windows Hello for Business - Tu sei la chiave
date: 2024-04-21
categories: [blogging, security]
tags: [cloud, microsoft, mfa] 
description: Scopri cos'è, come funziona e quali vantaggi offre questa tecnologia di autenticazione per Windows 10 e 11...
image:
     path: /assets/2024-06-21/media/image1.jpeg
---

`Windows Hello for Business` è una soluzione di autenticazione sicura per le aziende che utilizzano Windows 10 e Windows 11. Grazie a Windows Hello for Business gli utenti possono accedere ai propri dispositivi, alle applicazioni e ai servizi online utilizzando un PIN, un'impronta digitale o il riconoscimento facciale, eliminando la necessità di utilizzare password complesse (grattacapo per le aziende e gli uffici IT).

## Autenticazione a più fattori - MFA - a portata di dito

Windows Hello for Business è considerato sicuro come avere
`l'autenticazione a più fattori (MFA)` tradizionale perché utilizza un approccio simile per verificare l'identità dell'utente. Entrambi richiedono due o più delle seguenti credenziali:

-   qualcosa che conosci (come una password)
-   qualcosa che hai (come un dispositivo di sicurezza o il tuo
    telefono)
-   qualcosa che sei (come un'impronta digitale o il riconoscimento del
    viso).

Questo approccio rende molto più difficile per un attaccante accedere al tuo dispositivo o ai tuoi dati senza avere tutte le credenziali necessarie.

### A proposito di MFA
Quando si sente dire "Qualcosa che conosci" si riferisce a una forma di conoscenza che solo l'utente dovrebbe avere, come una password o un PIN che sono memorizzati nella memoria e non scritti su post-it appiccicati chissà dove. Anche se non digiti più una password tradizionale, il PIN che imposti funge da suo sostituto. È qualcosa che hai scelto e che conosci, e viene utilizzato assieme ai dati biometrici per verificare la tua identità. Il PIN è unico per il tuo dispositivo e non viene trasmesso o memorizzato al di fuori di esso, il che lo rende una forma sicura di "qualcosa che conosci".

Per chi non volesse utilizzare dati biometrici, il terzo fattore di autenticazione può essere qualcosa che hai, come un dispositivo di sicurezza o il dispositivo stesso che utilizzi per autenticarti perchè è "qualcosa che hai". L'autenticazione richiede che il dispositivo sia presente e utilizzato attivamente per il processo di accesso. Questo approccio a più fattori garantisce un livello di sicurezza molto più elevato rispetto ai metodi tradizionali di autenticazione a singolo fattore (username) e due fattori (username e password). Se ci si pensa, anche il prelievo in un bancomat è un'autenticazione a più fattori che sfrutta "qualcosa che hai", la tessera con un seriale unico, "qualcosa che conosci", il PIN e "qualcosa che sei", la persona che si reca al bancomat, per identificare il soggetto in modo univoco.

### Dove vengono salvati i dati biometrici?
Quando in Windows Hello for Business si utilizzano i dati biometrici come metodo di autenticazione, questi sono legati a un dispositivo specifico, aggiungendo un ulteriore livello di sicurezza. Ogni dispositivo di sicurezza crea il proprio database crittografato di informazioni. I dati biometrici e il PIN non vengono memorizzati su server (cloud o onprem), ma rimangono sul dispositivo in un database crittografato, riducendo il rischio di furto di credenziali da remoto.

## I Vantaggi

I vantaggi nell'implementazione ed uso di Windows Hello for Business
sono molteplici:

-   **Migliore sicurezza:** Windows Hello for Business utilizza la
    crittografia asimmetrica per proteggere le credenziali di accesso,
    rendendo più difficile per gli hacker rubare le informazioni di
    accesso come la password dell'utente (visto che quest'ultima non
    viene né usata né digitata).

-   **Facilità d'uso:** Gli utenti possono accedere rapidamente ai propri dispositivi e servizi utilizzando metodi di autenticazione bioometrici o PIN senza dover ricordare password complesse. Alla fine un PIN risulta più facile da ricordare e si inserisce più rapidamente una password.

-   **Riduzione dei costi:** La riduzione della dipendenza dalle password può ridurre i costi associati alla gestione delle password, come quelli legati al ripristino di password dimenticate.

## Prerequisiti

Windows Hello for Business è facile da implementare se si rispettano
alcuni prerequisiti:

-   Avere un dispositivo con Windows 10 o Windows 11 (versioni
    Professional o successive) aggiornato.

-   Un server di Active Directory o un servizio di directory basato su
    cloud, come Azure Active Directory (Entra ID).

-   Il PIN è sempre disponibile mentre per il biometrico è necessario un
    hardware compatibile, come un lettore di impronte digitali o una
    fotocamera a infrarossi per il riconoscimento facciale.

## Conclusione

In sintesi, Windows Hello for Business offre un modo sicuro e conveniente per gli utenti aziendali di accedere ai propri dispositivi e servizi, migliorando la sicurezza e riducendo i costi associati alla gestione delle password.
