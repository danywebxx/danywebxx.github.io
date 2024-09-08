---
title: Abilitare il debug logging in Windows DNS Server
date: 2024-09-08
categories: [blogging, tutorial]
tags: [microsoft, audit, dns, networking]
description: Come abilitare il debug logging in Windows DNS Server per tracciare le richieste dei client dell'organizzazione
image:
     path: /assets/2024-09-08/dns.png
---

Il servizio **DNS** permette da decenni di convertire facilmente nomi di dominio in indirizzi IP e viceversa; lo troviamo alla base di molte infrastrutture tecnologiche ed è uno dei mattoni funzionali per le organizzazioni che utilizzano Microsoft Active Directory.

## L’importanza dei log

La raccolta degli audit log è fondamentale per monitorare lo stato di salute dei sistemi informativi dell’organizzazione; raccogliere log delle richieste DNS risulta importante in molti programmi di cybersecurity perché queste possono contenere dati utili alle indagini forensi.  
In questo articolo voglio illustrare  come si può abilitare il registro delle richieste DNS in un sistema operativo Microsoft Windows Server che ricopre questa funzionalità. 

## Abilitare il debug logging

Ci sono diversi modi per tenere traccia delle query DNS, quello più semplice consiste nell’abilitare il **debug logging** del servizio DNS.  
Per abilitare il debug logging andate in:

**Start** \> **Programs** \> **Administrative Tools** \> e qui **DNS Manager**.

Tasto destro del mouse sul server dove volete abilitare il logging DNS e quindi **Properties**. 

![][/assets/2024-09-08/image1.png]  
 Spostatevi nel tab **Debug Logging** e selezionate le opzioni di cui volete tenere traccia.  
![][/assets/2024-09-08/image2.png]  
Ci sono alcune considerazioni da fare sulle opzioni abilitabili nel debug logging:

- **Transport protocol:** il servizio DNS potrebbe funzionare sia con protocollo UDP (il default) che TCP. Meglio che li selezionate entrambi.  
- **Packet Type:** in questo esempio ho voluto registrare solo le richieste perché mi interessa sapere il “*chi”* ha fatto la richiesta e non “*cosa”* gli è stato risposto. Nulla vieta di registrare anche le risposte.  
- **Other Options:** Per un log dettagliato potreste voler registrare anche i dettagli delle transazioni. Sappiate che in questo caso il log generato non sarà lineare e sarà più difficile metterlo in una tabella.  
- **Maximum size:** il file di log potrebbe crescere a dismisura se non controllato. Dategli un limite\!

Il risultato sarà un file di testo leggibile in cui saranno contenute le query DNS e il client che le ha richieste. 

## Anatomia del Log file

Le prime 29 righe contengono la descrizione delle colonne che si trovano nelle righe sottostanti; le righe contengono tutte le transazioni tra il client che effettua la richiesta DNS e il server che risponde.  
![][/assets/2024-09-08/image3.png]  
Tutte le colonne sono separate da spazio e facilmente “tabellabili” per una più facile analisi. Potete usare tool quali Excel o Log Parser scaricabile da [questo link](https://www.microsoft.com/en-us/download/confirmation.aspx?id=24659).

## Bibliografia

* [https://prophecyinternational.atlassian.net/wiki/spaces/Snare/pages/897417517/How+to+Collect+DNS+Logs](https://prophecyinternational.atlassian.net/wiki/spaces/Snare/pages/897417517/How+to+Collect+DNS+Logs)  
* [https://trustedsec.com/blog/tracing-dns-queries-on-your-windows-dns-server](https://trustedsec.com/blog/tracing-dns-queries-on-your-windows-dns-server)  
* [https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/introduction-to-network-trace-analysis-4-dns-it-s-always-dns/ba-p/4005803](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/introduction-to-network-trace-analysis-4-dns-it-s-always-dns/ba-p/4005803)  
* [https://learn.microsoft.com/en-us/archive/blogs/teamdhcp/network-forensics-with-windows-dns-analytical-logging](https://learn.microsoft.com/en-us/archive/blogs/teamdhcp/network-forensics-with-windows-dns-analytical-logging)  
* https://learn.microsoft.com/it-it/archive/blogs/secadv/parsing-dns-server-log-to-track-active-clients