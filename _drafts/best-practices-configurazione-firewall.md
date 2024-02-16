---
layout: post
title:  "7 best practices quando configuri un firewall"
date:   2024-02-13 16:00:00 +0100
categories: [blogging, security]
tags: [appunti, firewall, Paloalto, OPNsense, Cisco] 
---
Quando configuri ed applichi le regole sul tuo firewall usa queste best practices:

## 8 regole per gestire al meglio il firewall
- Pensa alle policy generali e poi applicale
- Metti sempre alla fine una regola che blocca tutto (deny any)
- Ricordati cbe l'ordine delle policy è importante. Si parte dall'alto e poi si scende (top-down)
- La prima policy che fa match viene applicara e le altre ignorate.
- Filtra sempre dalla piu specifica (in alto) alla piu generica (in basso). Ad esempio prima neghi il traffico al singolo host e poi permetto tutto il resto.
- Applica le regole il piu vicino possibile alla sorgente (cosi non affatichi altri apparati nel gestire traffico indesiderato)
- Le descrizioni esistono. Use per ricordati quella policu a cosa serve.
- Usa gli oggetti e semplificati la vita.
- Isa la logica del minor privilegio. Permetti quello che è necessario e blocca tutto il resto (ricordati che i log sono impirtanti per capire se qualcosa viene bloccato erroneamente)

## La regola d'oro
L'ultima ma la piu importante regola. Oltre questa non ce ne sono altre:
- Non chiuderti fuori! Assicurati di avere sempre una via di fuga!