---
title: Come impedire che un utente che effettua Microsoft Entra ID joid diventi administrator locale
date: '2024-07-02'
description: Evitare che l'utente che effettua il join del disposivo a Microsoft Entra ID diventi amministraotre locale me rimanga utente con privilegi standard
summary: Evitare che l'utente che effettua il join del disposivo a Microsoft Entra ID diventi amministraotre locale me rimanga utente con privilegi standard
url: /posts/come-limitare-privilegi-admins-utente-entra-id-join/
categories:
- Microsoft 365 e Azure
tags:
- m365
- entra
- modern workplace
---

Quando si effettua il **join** di un dispositivo a **Microsoft Entra ID**, l'utente usato per il join viene aggiunto automaticamente al gruppo dei local **administrators**. Questa configurazione risulta contraria a tutte le best-practices di security comportando notevoli rischi perché l'uso di utenze con privilegi elevati permette di modificare impostazioni, accedere a dati di altri utenti o installare applicazioni non autorizzate.

## Rispettare il principio del least privilege
Per rispettare il principio del **least privilege (PoLP)**, [dopo aver effettuato il join di un dispositivo con Microsoft Entra ID](/posts/dispositivi-microsoft-entra-joined-guida-steb-by-step/), è sufficiente limitare i permessi assegnati all'utente che effettua il join del dispositivo ad Entra ID.

Per assegnare i privilegi minimi bisogna usare un'opzione che si trova nel portale **Entra ID** > **Device** > **Device settings** nella sezione **Local administrator settings** che assegna i priviledi di **standard user** a chi effettua il join. Quesra opzione è in preview!

![standard user](/assets/2024-07-01/immagine1.png)

Sarà possibile aumentare i privilegi dell'utente, se strettamente necessario, in una fase successiva  utilizzando Intune o eseguendo alcune operazione manuali. Lo spiegherò in un prossimo articolo. Stay tuned!
