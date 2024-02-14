---
layout: post
title:  "Perchè dovresti evitare di chiudere con la X una sessione RDP"
date:   2024-02-13 16:00:00 +0100
categories: [blogging, security]
tags: [appunti, rdp, microsoft, hardening] 
---
Chi utilizza abitualmente i `Remote Desktop Services` (RDP) - anche conosciuti come `terminal services` se sei nel mondo dell'IT già da qualche anno - non dovrebbe mai chiudere una sessione remota con la X. E' necessario comprendere che disconnettere la sessione remota in questo modo potrebbe permettere ad un malintenzionato connesso allo stesso server di dirottare la sessione disconnessa su se stesso ed effettuare attacchi di privileged escalation.

## Sono le cattive abitudini a causare i danni peggiori
Come può un malintenzionato `dirottare una sessione RDP` e prenderne il controllo? Cercando su Google le parole `RDP hijacking` si capisce facilmente che è piu facile farlo che spiegarlo.
La tecnica più comune per dirottare una sessione RDP utilizza l'utilità nativa di Microsoft `tscon.exe`. Utilizzare tscon con i giusti accorgimenti e privilegi permette di prendere possesso di un'altra sessione (attiva o disconnessa) senza inserire password. Se si è fortunati perche la sessione disconnessa è di un utente privilegiato, diventa "facile" scalare privilegi, conquisrare il castello e lasciare pochissime tracce.

```cmd
sc create hijackedsession binpath= “cmd.exe /k tscon 1 /dest:rdp-tcp#2”
```
L'anello debole della catena siamo spesso noi amministratori che lasciamo le sessioni privilegiate aperte, disconnesse e non terminate (comprese le console).

![Dimostrazione di Alexander Korznikov](/assets/2024-02-13/korznikov.png){: width="600"}
_Dimostrazione di Alexander Korznikov_

## Mitigare il problema
Mitigrare il problema non è particolarmente difficile applicando le best-practices standard:
- Applicare criteri di gruppo per disconnettere le sessioni inattive o disconnesse. Così anche l'amministratore più distratto può dormire sonni tranquilli.
  ![Group Policy Computer - Session Time Limits](/assets/2024-02-13/msrdc_remote-desktop.png){: width="550"}
  _Group Policy Computer - Session Time Limits_

- Limitare l'esposizione dei servizi RDP su internet e nella rete. Se un servizio non è necessario, anche sui client, meglio disattivarlo.

La soluzione a molti problemi è evitare che un malintenzionato riesca ad utilizzare un utente amministrativo su un qualsiasi computer presente nella rete (sia server che client). Avendo a disposizione utenti privilegiati, anche solo localmente, è possibile effettuare un gran numero di attacchi informatici. 

## Bibliografia
Per approfondire la tecnica di questo particolare attacco alle sessioni RDP consiglio i post di:
- [csoonline](https://www.csoonline.com/article/569621/rdp-hijacking-attacks-explained-and-how-to-mitigate-them.html)
- [Doublepulsar](https://doublepulsar.com/rdp-hijacking-how-to-hijack-rds-and-remoteapp-sessions-transparently-to-move-through-an-da2a1e73a5f6)
- [Alexander Korznikov](https://www.youtube.com/watch?v=bbTfN5geSKw)
