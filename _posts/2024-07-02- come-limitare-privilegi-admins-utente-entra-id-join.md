---
title: Come limitare i privilegi all'utente che effettua Entra ID join
date: 2024-07-02
categories: [blogging, tutorial]
tags: [cloud, microsoft, m365, entraid, modern workplace]
description: Limitare i privilegi di administrators e rendere standard user chi collega il dispositivo ad Entra ID
---
Quando un utente effettua il **join** di un dispositivo ad **Entra ID**, l'utente usato per il join viene aggiunto automaticamente al gruppo dei local **administrators**. Questa configurazione, contraria a tutte le best-practices, comporta dei rischi per la sicurezza, in quanto l'utente con privilegi elevati potrebbe modificare impostazioni, accedere a dati di altri utenti o installare applicazioni non autorizzate.

## Rispettare il principio del least privilege
Per rispettare il principio del **least privilege (PoLP)** è sufficiente che limiti i permessi assegnati all'utente che effettua il join del dispositivo ad Entra ID.
Per dare il minimo dei privilegi, bisogna usare un'opzione che trovi nel portale **Entra ID** > **Device** > **Device settings** nella sezione **Local administrator settings** che fa diventare chi collega il dispositivo uno **standard user**. Ovviamente questa opzione è in preview!

![standard user](/assets/2024-07-01/immagine1.png)

Potrai aumentare i privilegi dell'utente successivamente utilizzando Intune o eseguendo alcune operazione manuali. Te lo spiegherò in un prossimo articolo. Stay tuned!