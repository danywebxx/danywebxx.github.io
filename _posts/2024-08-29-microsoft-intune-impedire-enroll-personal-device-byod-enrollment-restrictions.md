---
title: Microsoft Intune - impedire enroll di personal device e BYOD con le enrollment restrictions
date: 2024-08-29
categories: [blogging, tutorial]
tags: [cloud, microsoft, m365, entraid, intune]
description: Come impedire che dispositivi personali e BYOD possano effettuare l'enroll in Microsoft Intune configurando correttamente le policy di enrollment restrictions.
image:
     path: /assets/2024-08-29/microsoft-intune.png
---

## Cos'è Intune e perché le organizzazioni lo usano

**Microsoft Intune** è un servizio di gestione dei dispositivi mobili (MDM) offerto da Microsoft che permette alle organizzazioni di controllare e proteggere i dispositivi usati dai loro dipendenti. Con Intune, le organizzazioni possono impostare delle **policy** per garantire che i dispositivi siano conformi agli standard di sicurezza e di privacy richieste dal garante per la protezione dei dati personali dei paesi in cui operano. Ad esempio, con Intune è possibile verificare che i dispositivi contenenti dati aziendali siano protetti da una password complessa, che siano aggiornati e che non abbiano applicazioni non autorizzate.

## Cosa sono le enrollment restrictions

Le **enrollment restricionts** sono policy che si possono configurare in Intune per regolare quali dispositivi si possono registrare sulla base di specifiche caratteristiche quali: 

- Sistema Operativo
- Versione
- Produttore 
- Ownership

Le policy sono poi applicate a gruppi di utenti o dispositivi con la logica della priorità più alta.

## Come bloccare i dispositivi personali

All'interno del portale di Intune potete bloccare la possibilità che personal device e BYOD possano effettuare l'enroll. Gestire i personal device è importante perchè un dispositvo mal configurato e poco potretto potrebbe compromettere la sicurezza dell'organizzazione.  
Potete configurare le policy di gestione all'interno del [Portale di Gestione Endpoint](https://endpoint.microsoft.com/) nella sezione: **Device** > **Enrollment** > **Device platform restriction**

![Device Platform Restriction](/assets/2024-08-29/image_01.png)

Tramite l'opzione **Create restriction** nel tab del sistema operativo corretto, potete creare delle policy ad-hoc per ed assegnarla a gruppi specifici di utenti o device.

![Enrollment Restrictions](/assets/2024-08-29/image_02.png)

![Creazione Policy](/assets/2024-08-29/image_03.png)

Selezionando **Personally owned device** con l'opzione **Block** toglierete la possibilità che alcuni utenti possano effetture l'enroll di dispositivi Windows personali.

![blocco dei dispositivi personali](/assets/2024-08-29/image_04.png)

![Assegnazione](/assets/2024-08-29/image_05.png)

Potete anche agire creando una policy di default, inizialmente totalmente persmissiva, cliccando su **All Users** e negare la possibilità di effettuare enroll di dispositivi personali a tutti gli utenti. Eventuali eccezioni possono essere create con la logica del paragrafo precenderntee.

![Configurazione il default](/assets/2024-08-29/image_06.png)

## Cosa succede quando un utente prova ad effetture enroll di un personal device in Intune?

Quando un utente prova a registrare un dispositivo personale in Intune visualizzerà un messaggio di errore: *"Your device can’t be enrolled because it’s not owned by your organization."*. Non sarà quindi in grado di completare il processo di registrazione del dispositivo, di accedere all'app Intune Company Portal, di effettuare la verifica di compliance e se eventuali policy di **conditional access** fossero applicate, di accedere ai dati aziendali dal dispositovo personale.

## Come verificare che le enrollment restricions funzionano?

Per verificare che le enrollment restricion sono applicato correttamente vi basetà controllare i log di registrazione dei dispositivi nel portale di Intune:

**Device** > **Monitor** > **Enrollment failures**

![Controllo dei log](/assets/2024-08-29/image_07.png)

## Bibliografia
- [Block Users Personal Devices to Join Entra ID using Intune](https://www.anoopcnair.com/block-users-personal-devices-join-entra-intune/)
- [Microsoft - What are enrollment restrictions?](https://learn.microsoft.com/en-us/mem/intune/enrollment/enrollment-restrictions-set#blocking-personal-windows-devices)