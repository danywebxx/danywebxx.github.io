---
title: Come limitare i privilegi all'utente che effettua Entra ID join
date: 2024-07-02
categories: [blogging, tutorial]
tags: [cloud, microsoft, m365, entraid, modern workplace]
description: Limitare i privilegi di administrators e rendere standard user chi collega il dispositivo ad Entra ID
---
Quando si effettua il **join** di un dispositivo a **Microsoft Entra ID**, l'utente usato per il join viene aggiunto automaticamente al gruppo dei local **administrators**. Questa configurazione risulta contraria a tutte le best-practices di security comportando notevoli rischi perché l'uso di utenze con privilegi elevati permette di modificare impostazioni, accedere a dati di altri utenti o installare applicazioni non autorizzate.

## Rispettare il principio del least privilege
Per rispettare il principio del **least privilege (PoLP)** è sufficiente limitare i permessi assegnati all'utente che effettua il join del dispositivo ad Entra ID.
Per dare i minimi privilegi bisogna usare un'opzione che trovi nel portale **Entra ID** > **Device** > **Device settings** nella sezione **Local administrator settings** che fa diventare chi collega il dispositivo uno **standard user**. Ovviamente questa opzione è in preview!

![standard user](/assets/2024-07-01/immagine1.png)

Potrai aumentare i privilegi dell'utente successivamente utilizzando Intune o eseguendo alcune operazione manuali. Te lo spiegherò in un prossimo articolo. Stay tuned!