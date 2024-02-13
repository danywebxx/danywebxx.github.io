---
layout: post
title:  "Perchè non dovresti mai chiudere con la X una sessione remota RDP"
date:   2024-02-13 16:00:00 +0100
categories: [blogging, security]
tags: [appunti, rdp, microsoft, hardening] 
---
Se utilizzi abitualmente i `Remote Desktop Services` (RDP) - anche conosciuti come `terminal services` se sei nel mondo dell'IT già da qualche anno - non dovresti mai chiudere una sessione remota con la X per fare più in fretta. Devi sapere che disconnettere la sessione remota in questo modo potrebbe permettere ad un malintenzionato di dirottare la sessione remota disconnessa ed effettuare privileged escalation di utenti privilegiati.

## Sono le cattive abitudini a causare i peggiori disastri
Come può un malintenzionato `dirottare una sessione RDP` e prenderne il controllo? Cercando su Google le parole `RDP hijacking` si capisce facilmente che è piu difficile da spiegare che farsi.
La tecnica più comune per dirottare una sessione RDP utilizza l'utilità nativa di Microsoft `tscon.exe`. Utilizzare tscon con i giusti privilegi permette di prendere possesso di un'altra sessione (attiva o disconnessa) senza inserire password e se si è fortunati, effettuare privileged escalation lasciando pochissime tracce.

```cmd
sc create hijackedsession binpath= “cmd.exe /k tscon 1 /dest:rdp-tcp#2”
```
L'anello debole della catena siamo spesso noi amministratori che lasciamo le sessioni privilegiate aperte, disconnesse e non terminate (comprese le console).

![Dimostrazione di Alexander Korznikov](/assets/2024-02-13/korznikov.png)
_Dimostrazione di Alexander Korznikov_

## Mitigare il problema
Mitigrare il problema non è particolarmente difficile sapplicando le best-practices standard:
- Applicare criteri di gruppo per disconnettere le sessioni inattive o disconnesse. Così anche l'amministratore più distratto può dormire sonni tranquilli.
  ![Group Policy Computer - Session Time Limits](/assets/2024-02-13/msrdc_remote-desktop.png)
  _Group Policy Computer - Session Time Limits_
- Limitare l'esposizione dei servizi RDP su internet e nella rete. Se un servizio non è necessario, anche sui client, meglio disattivarlo.

## Bibliografia
https://www.csoonline.com/article/569621/rdp-hijacking-attacks-explained-and-how-to-mitigate-them.html
https://doublepulsar.com/rdp-hijacking-how-to-hijack-rds-and-remoteapp-sessions-transparently-to-move-through-an-da2a1e73a5f6
https://www.youtube.com/watch?v=bbTfN5geSKw
