---
layout: post
title:  "10 regole da seguire quando si configura un firewall"
date:   2024-02-13 16:00:00 +0100
categories: [blogging, security]
tags: [appunti, firewall, paloalto, opnsense, cisco] 
description: Una serie di semplici regole di best practices da seguire quando si configura un firewall per evitare di commettere errori.
image:
     path: /assets/2024-02-19/firewall.jpeg
---
Nelle scorse settimane lavorando ad un progetto con un `firewall OPNsense` installato su `cloud Azure` per regolare il traffico tra vNet, ho considerato quanto fosse importante per un tecnico avere delle regole di best practices da seguire per una corretta configurazione dei firewall.
Sono 10 semplici regole che si possono utilizzare sia su firewall più semplici che su quelli di nuova generazione (NGFW) in grado di riconoscere le applicazioni in layer 7.

## 10 regole per configurare al meglio un firewall
- Focalizzati sull'obiettivo, poi crea le policy
- Metti sempre alla fine una regola che blocca tutto (deny any) e ricordati che dovrà essere monitorata
- L'ordine delle policy è importante. Nei firewall le policy si leggono in top-down partendo dall'alto
- La prima policy che fa match viene applicata e le altre ignorate
- Filtra sempre dalla piu specifica (in alto) alla piu generica (in basso). Ad esempio prima si nega il traffico al singolo host e poi si permette tutto il resto
- Applica le regole il piu vicino possibile alla sorgente. Eviterai in questo modo che traffico indesiderato sia presente nel network
- Le descrizioni esistono. Vanno usate per ricordarsi cosa fa una determinata policy
- Usa gli oggetti e i gruppi per semplificarti la vita
- Applica sempre la logica del minor privilegio. Permetti quello che è necessario e blocca tutto il resto (ricorda che i log sono importanti per capire se qualcosa viene bloccato erroneamente)
- Cambia sempre le password di default delle utenze amministrative

## La regola d'oro - la numero zero
L'ultima regola ma la piu importante perchè oltre questa non ce ne sono altre:
- Non chiuderti fuori! Come amministratore assicurati di avere sempre una via sicura di accesso per manutenzione!
