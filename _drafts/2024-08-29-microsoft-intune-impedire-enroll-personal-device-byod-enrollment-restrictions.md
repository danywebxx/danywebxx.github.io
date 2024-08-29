---
title: Microsoft Intune - impedire enroll di personal device e BYOD con le enrollment restrictions
date: 2024-08-29
categories: [blogging, tutorial]
tags: [cloud, microsoft, m365, entraid, intune]
description: In questo articolo verrà illustrato come impedire che dispositivi personali e BYOD possano effettuare l'enroll in Microsoft Intune configurando correttamente le policy di enrollment restrictions. 
---

## Cos'è Intune e perché le organizzazioni lo usano

**Microsoft Intune** è un servizio di gestione dei dispositivi mobili (MDM) offerto da Microsoft che permette alle organizzazioni di controllare e proteggere i dispositivi usati dai loro dipendenti. Con Intune, le organizzazioni possono impostare delle policy per garantire che i dispositivi siano conformi agli standard di sicurezza e di privacy richieste dai garanti dei paesi in cui operano. Ad esempio, con Intune è possibile verificare che i dispositivi contenenti dati aziendali siano protetti da una password complessa, che siano aggiornati e che non abbiano applicazioni non autorizzate.

## Cosa sono le enrollment restrictions

Le **enrollment restricionts** sono policy che si possono configurare in Intune per regolare quali dispositivi si possono registrare sulla base di specifiche caratteristiche: 

- Sistema Operativo
- Versione
- Produttore 
- Ownership (Aziendale | Personale )

Le policy sono poi applicate a gruppi di utenti o dispositivi con la logica della priorità più alta.

## Come bloccare i dispositivi personali

All'interno del portale di Intune puoi bloccare la possibilità che dispositivi personali possano  effettuare l'enroll.  
Puoi configurare le policy di gestione nella sezione: **Device** > **Enrollment** > **Device platform restriction**

![Device Platform Restriction](/assets/2024-07-01/image_01.png)

Tramite Create restriction puoi creare delle policy ad-hoc per sistema operativo e assegnarla a gruppi  specifici.

![Enrollment Restrictions](/assets/2024-07-01/image_02.png)

![Creazione Policy](/assets/2024-07-01/image_03.png)

![blocco dei dispositivi personali](/assets/2024-07-01/image_04.png)

![Assegnazione](/assets/2024-07-01/image_05.png)

Oppure agire di default cliccando **All Users** e negare la possibilità di effettuare enroll di dispositivi personali a tutti gli utenti. Eventuali eccezioni possono essere create con la logica del paragrafo precendere.

![Configurazione il default](/assets/2024-07-01/image_06.png)

## Cosa succede quando un utente prova ad effetture enroll di un dispositivo personal in Intune?

Quando un utente prova a registrare un dispositivo personale in Intune visualizzerà un messaggio di errore: *"Your device can’t be enrolled because it’s not owned by your organization."*. Non sarà quindi in grado di completare il processo di registrazione del dispositivo, di accedere all'app Intune Company Portal, di effettuare la verifica di compliance e se eventuali policy di c**onditional access** fossero applicate, di accedere ai dati aziendali dal dispositovo personale.

## Come verificare che le enrollment restricions funzionano?

Per verificare che la restrizione di registrazione funzioni, puoi controllare lo stato di registrazione dei dispositivi nel portale Intune:

Device > Monitor > Enrollment failures 

![Controllo dei log](/assets/2024-07-01/image_07.png)