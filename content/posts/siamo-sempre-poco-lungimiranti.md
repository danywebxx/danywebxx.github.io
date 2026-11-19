---
title: "Siamo sempre poco lungimiranti e con le fabbriche connesse si vede"
date: 2026-11-19
description: "Le fabbriche sono sempre più nel mirino degli attacchi informatici. Spesso non nascono da un grande errore, ma da tante piccole cose rimandate. Vediamo quali, e da dove si può partire."
summary: "Le fabbriche sono sempre più nel mirino degli attacchi informatici. Spesso non nascono da un grande errore, ma da tante piccole cose rimandate. Vediamo quali, e da dove si può partire."
categories: ["Sicurezza"]
tags: ["ot", "ransomware", "clusit", "networking", "business-continuity"]
featureimage: "/assets/IMG_0565.jpeg"
images: ["/assets/IMG_0565.jpeg"]
draft: false
---

*"È facile perdere di vista i grandi rischi perché sono solo una reazione a catena di piccoli eventi, ognuno dei quali è facile da ignorare. Quindi tendiamo sempre a sottovalutare le probabilità di grandi rischi."* (Morgan Housel)

Una password mai cambiata. Un collegamento remoto lasciato aperto perché "serve al tecnico". Un macchinario che funziona da vent'anni e che nessuno osa toccare. Un aggiornamento rimandato per troppo tempo.

Presi uno per uno, sono dettagli. Messi in fila, sono la strada più breve per fermare una linea di produzione.

## Il significato di OT

Nelle aziende convivono due mondi tecnologici. Il primo è l'IT (Information Technology), cioè PC, posta elettronica, gestionale e server. Il secondo è l'OT (Operational Technology), la tecnologia che comanda macchine, impianti e processi produttivi, come i PLC e i piccoli computer industriali che dicono a un macchinario cosa fare.

Per anni l'OT è rimasta isolata dal resto - non sconnesso ma quasi - e questo isolamento ci faceva sentire al sicuro. Poi, per controllare gli impianti da remoto, raccogliere dati e far lavorare meglio i reparti, abbiamo collegato tutto. La sicurezza, in molti casi, è rimasta indietro.

## I numeri non sono rassicuranti

Secondo i dati Clusit (l'Associazione Italiana per la Sicurezza Informatica) presentati a settembre, nel primo semestre 2026 il manifatturiero italiano ha subìto 45 incidenti informatici noti, il 18,4% di tutti quelli registrati in Italia. Nel 2025 la quota era del 12,4%. In pratica, quasi un incidente su cinque riguarda una fabbrica.

Nella grande maggioranza dei casi dietro c'è la criminalità informatica, oltre il 98% degli episodi nel manifatturiero, e il ransomware è passato dal 76% del 2025 all'89% del primo semestre 2026. Il ransomware è una specie di sequestro: i file vengono cifrati e, per riaverli, si chiede un riscatto. Clusit stessa invita alla cautela: l'aumento potrebbe dipendere anche dai nuovi obblighi di notifica della NIS2, che fanno emergere episodi prima non classificati.

Anche a livello mondiale i segnali vanno nella stessa direzione. Dragos, società specializzata in sicurezza industriale, ha segnalato per il 2025 un aumento del 49% dei gruppi ransomware che hanno colpito il settore industriale, per circa 3.300 aziende nel mondo.

## Non serve toccare la macchina per fermarla

C'è un equivoco molto diffuso: "tanto i nostri macchinari non navigano in Internet". Può anche essere vero ma un attacco non deve per forza compromettere un PLC per fermare una fabbrica. Basta che un ransomware blocchi la logistica, le postazioni di controllo o altre workstation, e la produzione si ferma lo stesso.

Il caso di Jaguar Land Rover lo dimostra: l'azienda ha fermato la produzione il 1° settembre 2025 dopo un attacco informatico ai suoi sistemi. Gli stabilimenti britannici, che di norma producono circa 1.000 veicoli al giorno, sono rimasti fermi per più di un mese. Il Cyber Monitoring Centre ha stimato un danno di 1,9 miliardi di sterline per l'economia britannica, con oltre 5.000 organizzazioni coinvolte, perché un fermo del genere si porta dietro tutti i fornitori.

Più vicino a noi c'è il porto di Ancona. Un'analisi tecnica della società americana Resecurity ricostruisce l'attacco ransomware di dicembre 2025, attribuito al gruppo Anubis. Il copione è quello classico: un allegato malevolo, poi l'aggressore sale di privilegi, si muove nella rete, copia i dati e infine cifra tutto. Il dettaglio più istruttivo è che non ha toccato i sistemi di controllo industriale. Sono bastati account cloud configurati in modo non sicuro, e le operazioni del porto si sono fermate lo stesso. Nessuna magia, solo una catena di passaggi, ognuno facile da ignorare.

Sono casi di grandi organizzazioni, ma il meccanismo non dipende dalle dimensioni. Per un'azienda da 20 o 60 postazioni, una settimana di fermo può pesare, in proporzione, anche di più.

## Perché succede? Ancora una volta per le piccole cose

Torniamo a Housel. Nelle PMI manifatturiere gli ingredienti ritornano spesso.

- **Una rete unica per tutto "piatta".** Uffici, Wi-Fi degli ospiti e macchinari condividono la stessa rete. Se un PC amministrativo viene infettato, il problema ha strada libera fino al reparto.
- **Accessi remoti dei fornitori.** Il tecnico del produttore si collega per l'assistenza, ma chi controlla come, quando e con quali credenziali?
- **Sistemi che nessuno aggiorna.** Un computer si riavvia (quasi) senza problemi, una macchina che lavora a ciclo continuo no. Così si rimanda, e poi si rimanda ancora.
- **Carenza nell'inventario di ciò che è connesso in rete.** Non si può proteggere ciò che non si sa di avere.
- **Backup mai provati.** La copia di sicurezza esiste, ma nessuno ha mai verificato che il ripristino funzioni. Peggio ancora se è raggiungibile dalla stessa rete che viene attaccata.

Il punto, osserva un'analisi pubblicata da Agenda Digitale, non è solo evitare l'attacco: è fare in modo che un'intrusione non si trasformi in un fermo prolungato.

## Da dove partire senza fare rivoluzioni

Si può partire da poche msemplici mosse:

1. **Sapere cosa c'è.** Un elenco di macchine e dispositivi collegati alla rete, con il nome di chi li gestisce.
2. **Separare le reti.** Un problema in ufficio non deve poter arrivare in produzione, e viceversa.
3. **Controllare gli accessi remoti.** Chi può collegarsi, da dove, con quali credenziali, e solo quando serve.
4. **Avere un piano di emergenza per ripartire.** Copie di sicurezza che la rete aziendale non possa raggiungere, provate davvero, e una procedura scritta per il giorno in cui tutto si ferma.

Sono mosse alla portata di qualunque azienda e il loro costo di realizzazione va confrontato con quello di un giorno di produzione ferma.

## La lungimiranza è un esercizio piccolo e quotidiano

I grandi rischi non si presentano mai con un annuncio ma sono la somma di cose piccole che abbiamo scelto di rimandare. La buona notizia è che la somma funziona anche al contrario, perché pochi gesti semplici e fatti con continuità, tolgono molto terreno a chi attacca.

**E voi? Nella vostra azienda, sapreste dire chi può collegarsi da remoto ai macchinari?**

{{< scrivimi >}}

## Fonti

- Dati Clusit sul primo semestre 2026 (45 incidenti, 18,4%, quota del 12,4% nel 2025, cybercrime oltre il 98%, ransomware dal 76% all'89%, cautela sulla NIS2): [Agenda Digitale, "Manifatturiero, quanti rischi cyber: cosa dicono i dati del Clusit"](https://www.agendadigitale.eu/sicurezza/manifatturiero-quanti-rischi-cyber-cosa-dicono-i-dati-del-clusit/)
- Dragos, aumento del 49% dei gruppi ransomware nel settore industriale (3.300 aziende), e fermo di produzione senza compromissione dei PLC: [Acronis, "Attacchi informatici e arresto della produzione"](https://www.acronis.com/it/blog/posts/when-a-cyberattack-stops-production-how-ot-teams-can-recover-faster-in-2026/)
- Ransomware che blocca logistica, HMI o workstation senza toccare i PLC: [Cybersecurity360, "I costi del downtime industriale da attacco cyber: come quantificare l'impatto economico dei blocchi di produzione"](https://www.cybersecurity360.it/soluzioni-aziendali/i-costi-del-downtime-industriale-da-attacco-cyber-come-quantificare-limpatto-economico-dei-blocchi-di-produzione/)
- Jaguar Land Rover, stima dei danni (1,9 miliardi di sterline, oltre 5.000 organizzazioni), durata del fermo e produzione giornaliera: [Business Today, "JLR cyberattack led to £1.9 billion loss, impacted over 5,000 companies"](https://www.businesstoday.in/amp/latest/corporate/story/jlr-cyberattack-led-to-1-9-billion-pound-loss-impacted-over-5000-companies-499273-2025-10-23)
- Jaguar Land Rover, data di inizio del fermo: [Wikipedia, "Jaguar Land Rover cyberattack"](https://en.wikipedia.org/wiki/Jaguar_Land_Rover_cyberattack)
- Porto di Ancona, attacco del gruppo Anubis e analisi di Resecurity: [Cybersecurity360, "Il confine IT/OT non dà garanzie. E il caso del porto di Ancona lo dimostra"](https://www.cybersecurity360.it/outlook/il-confine-it-ot-non-da-garanzie-e-il-caso-del-porto-di-ancona-lo-dimostra/)
- Evitare che una compromissione diventi fermo produttivo: [Agenda Digitale, "Cybersecurity OT, come evitare il fermo produttivo nella fabbrica digitale"](https://www.agendadigitale.eu/industria-5-0/cybersecurity-ot-come-evitare-il-fermo-produttivo-nella-fabbrica-digitale/)
- Citazione di Morgan Housel, tratta da *Same as Ever* e tradotta dall'inglese.
