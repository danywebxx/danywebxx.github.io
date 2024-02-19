---
layout: post
title:  "10 regole da seguire quando configuri un firewall"
author: Daniele Longhi
date:   2024-02-13 16:00:00 +0100
categories: [blogging, security]
tags: [appunti, firewall, Paloalto, OPNsense, Cisco] 
description: Una serie di semplici regole da seguire quando si configura un firewall per evitare di commettere errori.
image:
  path: /assets/IMG_6772.jpeg
---
Nelle scorse settimane lavoravo ad un progetto con un firewall OPNsense su Azure per regolare il traffico tra vNet. Ho pensato a quanto fosse importante per un tecnico avere delle semplici regole di configurazione da seguire quando si configura un firewall.
Sono 10 semplici regole che si possono seguire partendo dai dispositivi più semplici fino ai firewall di nuova generazione (NGFW) in grado di riconoscere le applicazioni in layer 7.

## 9 regole per gestire al meglio un firewall
- Pensa in generale cosa devi fare, poi crea le policy
- Metti sempre alla fine una regola che blocca tutto (deny any) e che dovrà essere monitorata. Si costruiranno sopra tutte le altre policy
- Ricordati che l'ordine delle policy è importante. Nei firewall le policy si leggono in top-down partendo dall'alto
- La prima policy che fa match viene applicata e le altre ignorate
- Filtra sempre dalla piu specifica (in alto) alla piu generica (in basso). Ad esempio prima si nega il traffico al singolo host e poi si permette tutto il resto
- Applica le regole il piu vicino possibile alla sorgente. Eviterai in questo modo che traffico indesiderato giri nel network
- Le descrizioni esistono. Vanno usate per ricordarsi cosa fa una determinata policy
- Usa gli oggetti e i gruppi per semplificati la vita
- Applica sempre la logica del minor privilegio. Permetti quello che è necessario e blocca tutto il resto (ricorda che i log sono impirtanti per capire se qualcosa viene bloccato erroneamente)

## La regola d'oro
L'ultima regola ma la piu importante. Oltre questa non ce ne sono altre:
- Non chiuderti fuori! Come amministratore assicurati di avere sempre una via sicura di accesso per manutenzione!