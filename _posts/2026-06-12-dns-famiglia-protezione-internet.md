---
title: "Proteggi la tua famiglia su Internet con due numeri"
date: 2026-06-12
categories: [blogging,tutorial]
tags: [sicurezza, dns, famiglia, internet, consigli]
description: >-
  Cambiare due numeri nel router di casa può bloccare malware e contenuti
  inappropriati per i tuoi figli. Ecco come farlo gratis in pochi minuti.
---

Mettiamo il lucchetto alla porta di casa, ma lasciamo Internet completamente aperta. Eppure basterebbe cambiare due numeri nel router per aggiungere un primo livello di protezione - gratis, senza installare nulla. Lo faccio anch'io a casa mia, con il DNS `1.1.1.2`. In questo articolo ti spiego come funziona e come configurarlo in pochi minuti.

## Cos'è un DNS e perchè ti interessa

Ogni volta che digiti un indirizzo nel browser o che un'app si collega a Internet, il tuo dispositivo chiede a un server DNS: *"Dove si trova questo sito?"*. Immaginalo come un elenco telefonico della rete.

Di solito questo "elenco" è quello del tuo operatore, che risponde a qualsiasi domanda senza fare distinzioni ma esistono DNS alternativi che, prima di risponderti, controllano anche cosa stai cercando. Se il sito è noto per distribuire malware o contenuti per adulti, semplicemente non ti danno la risposta. Il sito non si apre e basta.

Questi DNS filtrati sono offerti da Cloudflare, una delle infrastrutture Internet più grandi al mondo. Il servizio si chiama **1.1.1.1 for Families**, è gratuito e non richiede registrazione.

Nessun software da installare, nessun abbonamento da sottoscrivere e funziona su tutti i dispositivi collegati alla tua rete di casa.

## Due opzioni, a seconda di cosa vuoi bloccare

Puoi scegliere tra due livelli di protezione:

**Solo malware** - blocca i siti pericolosi (virus, phishing, truffe):
- DNS Primario: `1.1.1.2`
- DNS Secondario: `1.0.0.2`

**Malware + contenuti per adulti** - blocca tutto quanto sopra più i siti pornografici:
- DNS Primario: `1.1.1.3`
- DNS Secondario: `1.0.0.3`

Il secondo è quello che consiglio alle famiglie con bambini o ragazzi.

## Dove si cambiano questi numeri

Hai due strade:

**1. Sul router** - è la scelta migliore perché protegge tutti i dispositivi della casa in un colpo solo: smartphone, tablet, PC, Smart TV. Devi entrare nell'interfaccia di configurazione del tuo router (di solito all'indirizzo `192.168.1.1` o `192.168.0.1`), trovare la sezione DNS e sostituire i valori esistenti con quelli sopra.

**2. Sul singolo dispositivo** - se vuoi proteggere solo un PC o uno smartphone specifico, puoi cambiare il DNS nelle impostazioni di rete di quel dispositivo. Meno comodo, ma funziona lo stesso.

Le istruzioni precise variano a seconda del modello di router o del sistema operativo, ma cercando su Google il nome del tuo router + "come cambiare DNS" trovi quasi sempre una guida specifica.

## Cosa cambia davvero nell'uso quotidiano

Nulla di visibile per la navigazione normale perchè i siti che usi tutti i giorni continuano a funzionare esattamente come prima. La differenza la noti solo quando qualcuno cerca di raggiungere un sito bloccato: semplicemente non si apre, senza messaggi allarmanti o blocchi invasivi.

Non è una soluzione infallibile perchè nessun filtro lo è ma aiuta ad alzare l'asticella per i contenuti indesiderati, soprattutto per i più giovani che navigano in modo meno consapevole.

## Un ultimo punto

Questo tipo di protezione agisce sulla rete di casa ma tieni a mente che  quando i dispositivi escono e si connettono ad altre reti - la rete mobile, il Wi-Fi di scuola o di un amico - il filtro non è più attivo. Per una protezione più estesa sui dispositivi dei ragazzi esistono anche soluzioni dedicate: parental control integrati nei sistemi operativi o applicazioni specifiche. Ma questo è un argomento per un altro articolo.

Per adesso, cambiare i DNS del router è il punto di partenza più semplice e immediato che puoi fare oggi, senza spendere un euro.

## Riferimento

La documentazione ufficiale del servizio è disponibile sul blog di Cloudflare: [Introducing 1.1.1.1 for Families](https://blog.cloudflare.com/introducing-1-1-1-1-for-families/)