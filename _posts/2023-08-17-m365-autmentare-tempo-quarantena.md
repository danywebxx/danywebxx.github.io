---
title: M365 - Aumentare il periodo di conservazione della quarantena
date: 2023-08-17
categories: [blogging, tutorial]
tags: [cloud, microsoft, m365 ]
description: In M365 le policy di antispam preimpostate conservano i messaggi messi in quarantena per 15 giorni prima di elimiarli automaticamente. In questo articolo vediamo come modificare il periodo di conservazione.
---

Nel cloud Microsoft 365 una serie di `policy di sicurezza` predefinite proteggono gli utenti dai messaggi di posta indesiderati e pericolosi spostanto quello che la `threat intelligence` classifica spam o phishing in `quarantena`. I messaggi che finiscono in quarantena sono poi eliminati automaticamente dopo `15 giorni` fatta salva la possibilità di rilascio del destinatario che li identifica come falsi-positivi.

## Raccomandazione
I maggiori esperti raccomandano di elevare il tempo di conservazione ad almeno 30 giorni cosi da non perdere eventualo messaggio finiti in quarantena per errore.

### Perché 30 giorni?

Durante il periodo di quarantena è possibile vedere, rilasciare, scaricare, cancellare e segnalare un messaggio come falso positivo. Conservare un messaggio in quarantena per 30 giorni consente di avere più tempo per investigare sul problema o permettere ai più sbadati di guardare la quarantena per più tempo.

## Configurare il periodo di conservazione in quarantena
1- Andare nel portale [Security di M365](https://security.microsoft.com/) nella sezione Policies e Rules 
![Policy e Rules](/assets/2023-08-17/image1.png)
_Policy e Rules_

2- Aprire le policy di anti-spam
![Anti-spam policy](/assets/2023-08-17/image2.png)
_Anti-spam policy_

3- Selezionare l'anti-spam inbound policy
![Anti-spam inbound policy](/assets/2023-08-17/image3.png)
_Anti-spam inbound policy_

4- Modificare il periodo di conservazione portandolo ad almeno 30
![Giorni di conservazione](/assets/2023-08-17/image4.png)
_Giorni di conservazione_

Ricordati di lasciarmi un commento se l'articolo è stato utile.
