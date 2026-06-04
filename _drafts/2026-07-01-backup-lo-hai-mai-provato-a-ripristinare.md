---
title: "Backup sì, ma lo hai mai provato a ripristinare?"
date: 2026-06-04 09:00:00 +0200
categories: [blogging, guida]
tags: [backup, sicurezza, pmi, microsoft, m365]
description: >-
  Avere un backup non basta. La vera domanda è: se domani mattina i tuoi dati sparissero, saresti in grado di recuperarli davvero? Ecco cosa controllare.
---

Lasciami indovinare.

Da qualche parte nella tua azienda gira un sistema di backup. Magari è sul NAS in sala server, magari è su un cloud che ti ha configurato qualcuno anni fa. Ogni tanto vedi una notifica che dice "completato" e vai avanti con la giornata.

Bene. Il problema è che probabilmente non sai se funziona davvero.

E no, non lo dico per spaventarti. Lo dico perché è esattamente la situazione in cui si trovano la maggior parte delle aziende con cui lavoro.

## Il backup che non ha mai salvato nessuno

Ho visto sistemi di backup girare regolarmente per mesi. Notifiche verdi, log puliti, tutto apparentemente a posto. Poi un giorno qualcosa va storto — un ransomware, un disco che cede, un file cancellato per errore — e si scopre che i dati ripristinati sono corrotti, incompleti, o peggio ancora: mancanti.

Nessuno se n'era accorto perché nessuno aveva mai fatto una prova sul serio.

Ecco la verità scomoda: **un backup che non hai mai testato non è un backup. È una speranza.**

## Le tre domande a cui devi saper rispondere

Se anche una sola di queste ti mette in difficoltà, hai un problema da risolvere.

**Di cosa stai facendo il backup, esattamente?**

"Di tutto" non è una risposta. I dati della tua azienda sono sparsi ovunque: cartelle condivise, email, gestionale, file sui PC dei singoli, roba su SharePoint o OneDrive. Ogni pezzo va coperto in modo specifico. Se non sai cosa è incluso e cosa no, non sai cosa perderesti in caso di emergenza.

**Ogni quanto gira il backup e per quanto tempo vengono conservati i dati?**

Se il backup gira ogni 24 ore e ti accorgi del problema dopo tre giorni, hai già perso tre giorni di lavoro. Se i backup vengono conservati solo per sette giorni e un ransomware è rimasto silenzioso per dieci giorni prima di attivarsi — e succede, credetemi — hai perso tutto. La finestra temporale conta quanto la frequenza.

**Quanto tempo ci vuole per tornare operativo?**

Questa è la domanda che nessuno si pone mai prima dell'emergenza. Ripristinare 500 GB di dati richiede ore, a volte giorni. Se la tua azienda non può permettersi di stare ferma, devi saperlo adesso — non mentre i tuoi dipendenti ti guardano chiedendosi quando possono rimettersi a lavorare.

## Una cosa che devi fare nei prossimi trenta giorni

Testa il ripristino.

Non serve simulare un disastro completo. Basta prendere una cartella, un file importante, e verificare concretamente che si riesca a recuperarlo, in quanto tempo, e in che stato arriva.

Fallo ogni tre o sei mesi. Documentalo. Scrivi chi ha fatto il test, cosa ha ripristinato, quanto ci ha messo. È una di quelle cose che sembra burocratica finché non ti salva davvero.

## La cosa che quasi nessuno sa su Microsoft 365

Parentesi rapida, perché la vedo sbagliare spesso.

Se usi OneDrive, SharePoint o Teams, probabilmente pensi che i tuoi dati siano "già al sicuro perché sono nel cloud di Microsoft". È una comprensione ragionevole ma parzialmente sbagliata.

Microsoft garantisce che il servizio funzioni. Non garantisce il ripristino dei tuoi dati se un utente cancella qualcosa per errore, se un ransomware cifra i file sincronizzati, o se succede qualcosa che va oltre i limiti del cestino integrato. Per quello serve una soluzione di backup separata, dedicata a M365.

È un dettaglio che può fare una differenza enorme.

## La regola che uso come punto di riferimento

Si chiama regola del 3-2-1, ed è uno standard di settore semplice da ricordare:

- **3** copie dei dati
- **2** su supporti diversi (ad esempio server locale più cloud)
- **1** in una posizione separata, fuori dall'ufficio

Non è l'unica strategia possibile, ma è un buon metro per valutare se quello che hai regge o ha dei buchi evidenti.

## Per chiudere

Avere un backup attivo è già meglio di niente. Ma sapere che funziona, sapere cosa copre, e sapere quanto tempo ci vuole per tornare operativo — quello è tutta un'altra cosa.

Se non sei sicuro di almeno una di queste tre risposte, è il momento giusto per verificarlo. Prima che arrivi il motivo urgente per farlo.

---

*Hai dubbi su come è strutturato il tuo sistema di backup o vuoi capire se quello che hai è adeguato? [Scrivimi](mailto:daniele@danyweb.it), ne parliamo senza impegno.*
