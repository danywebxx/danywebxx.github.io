---
layout: post
title:  "10 regole da seguire quando si configura un firewall"
date:   2024-02-13 16:00:00 +0100
categories: [blogging, security]
tags: [appunti, firewall, paloalto, opnsense, cisco, sicurezza] 
description: "Configurare un firewall senza seguire delle best practice porta quasi sempre a problemi: traffico bloccato per errore, regole in conflitto, o peggio ancora buchi di sicurezza che nessuno nota. Ecco le 10 regole che seguo ogni volta."
image:
     path: /assets/2024-02-19/firewall.jpeg
---
Nelle scorse settimane, lavorando a un progetto con un firewall OPNsense su Azure per regolare il traffico tra vNet, ho messo per iscritto le regole che seguo ogni volta che mi trovo a configurare un firewall — che sia un apparato semplice o un NGFW di nuova generazione come Palo Alto o Fortinet.

Non sono regole teoriche. Sono il risultato di anni di lavoro su reti reali, con errori reali alle spalle.

## 1. Parti dall'obiettivo, non dalle regole

Prima di aprire l'interfaccia del firewall, chiediti: **cosa deve proteggere questa configurazione? Cosa deve permettere, e cosa deve bloccare?**

È un passaggio che sembra ovvio ma viene saltato spesso. Il risultato è una lista di policy costruita a colpi di "aggiungiamo questa regola perché serve adesso" — senza una logica d'insieme. Dopo sei mesi nessuno capisce più cosa fa cosa, e modificare qualcosa diventa un rischio.

Definisci prima la politica di sicurezza in linguaggio semplice, poi traducila in regole.

## 2. Chiudi tutto, apri solo quello che serve

La logica di default di qualsiasi firewall ben configurato è: **blocca tutto, permetti l'eccezione**.

Non il contrario. Partire da una postura permissiva e aggiungere blocchi man mano è un approccio che lascia finestre aperte che non ricordi di aver aperto. La regola finale della tua policy dovrebbe sempre essere una *deny any any* esplicita — non affidarti al comportamento implicito dell'apparato.

E quella regola va monitorata. I log di quello che viene bloccato sono spesso più informativi di quelli di quello che viene permesso.

## 3. L'ordine delle regole non è un dettaglio

I firewall leggono le policy dall'alto verso il basso. **La prima regola che fa match viene applicata, le successive vengono ignorate.**

Questo significa che l'ordine è parte integrante della logica di sicurezza — non una questione estetica. Una regola permissiva messa troppo in alto può vanificare un blocco che hai definito più in basso. Succede più spesso di quanto si pensi, soprattutto quando le policy crescono nel tempo e vengono modificate da più persone.

## 4. Dal più specifico al più generico, sempre

Conseguenza diretta del punto precedente: **le regole più specifiche vanno in cima, quelle più generiche in fondo**.

Esempio pratico: se vuoi bloccare il traffico verso un singolo host ma permettere tutto il resto verso quella subnet, la regola di blocco deve stare sopra quella permissiva. Se le metti nell'ordine sbagliato, il blocco non verrà mai applicato.

## 5. Applica le regole vicino alla sorgente

Più tardi intercetti il traffico indesiderato, più strada ha già percorso nella tua rete. **Filtra il prima possibile, il più vicino possibile alla sorgente.**

Questo vale soprattutto in architetture segmentate con più zone (LAN, DMZ, WAN, guest). Ogni zona dovrebbe avere le proprie policy in uscita — non affidarti solo al perimetro esterno per proteggere le zone interne.

## 6. Le descrizioni non sono opzionali

Ogni regola dovrebbe avere una descrizione che risponda a tre domande: **chi l'ha creata, perché esiste, e quando può essere rimossa.**

So che sembra burocratico. Ma torna su una configurazione dopo un anno — o falla toccare a un collega — e capirai immediatamente il valore di una riga di testo che dice *"Regola temporanea per progetto X, verificare a dicembre"* invece di niente.

Le regole senza descrizione tendono a diventare immortali: nessuno sa cosa fanno, nessuno osa rimuoverle.

## 7. Usa oggetti e gruppi, non indirizzi IP a caso

Scrivere un indirizzo IP direttamente in una regola è la strada più veloce verso una configurazione ingestibile. **Usa oggetti con nome** (es. `SRV-FileServer`, `NET-Sede-Milano`) e raggruppali dove ha senso.

Quando quell'indirizzo cambia — e prima o poi cambia — aggiorni l'oggetto in un posto solo invece di cercare tutte le regole che lo referenziano. E il nome descrittivo rende la policy leggibile a colpo d'occhio.

## 8. Principio del minimo privilegio

Permetti solo il traffico strettamente necessario per far funzionare quello che deve funzionare. **Non "apri la porta" per comodità** con regole troppo ampie che poi rimangono lì per sempre.

Se un'applicazione ha bisogno di parlare con un server su una porta specifica, apri quella porta verso quell'host — non tutta la subnet su tutte le porte. Il delta di lavoro è minimo, il delta di rischio è enorme.

I log sono i tuoi alleati in questo: se qualcosa smette di funzionare dopo che hai ristretto una regola, lo vedi subito nel traffico bloccato.

## 9. Cambia sempre le credenziali di default

Sembra ovvio. Non lo è — lo vedo ancora succedere.

Qualsiasi apparato firewall — fisico o virtuale — viene consegnato con credenziali di default documentate pubblicamente. **Cambiale prima di fare qualsiasi altra cosa**, prima ancora di collegarlo alla rete di produzione. Vale per l'interfaccia web, per SSH, per SNMP, per qualsiasi accesso amministrativo.

## 10. Documenta e rivedi periodicamente

Una configurazione firewall non è un'opera finita. La rete cambia, i servizi cambiano, le minacce cambiano. **Ogni sei-dodici mesi vale la pena fare una review delle policy** per rimuovere regole obsolete, consolidare quelle ridondanti, e verificare che la logica complessiva sia ancora coerente con l'architettura reale.

Una configurazione che non viene mai rivista tende ad accumulare regole inutili, eccezioni temporanee diventate permanenti, e — nel peggiore dei casi — buchi di sicurezza che nessuno nota più perché ci si è abituati.

---

## La regola zero — quella che viene prima di tutte

Prima di applicare qualsiasi modifica in produzione: **assicurati di avere sempre una via di accesso alternativa per la manutenzione.**

Non chiuderti fuori. È la cosa più imbarazzante che possa capitare a un sistemista, e capita. Una sessione console fisica, un accesso out-of-band, un IP di management su una interfaccia dedicata — qualcosa che ti permetta di recuperare la situazione se una modifica va storta.

---

Se stai valutando o rivedendo la configurazione firewall della tua azienda e vuoi un confronto con qualcuno che ci lavora ogni giorno, [scopri come posso aiutarti →](/servizi/)
