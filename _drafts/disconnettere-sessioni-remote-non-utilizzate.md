---
layout: post
title:  "Perchè non dovresti mai chiudere con la X una sessione remota RDP"
date:   2024-02-13 16:00:00 +0100
categories: [blogging, security]
tags: [appunti, rdp, microsoft, hardening] 
---
Se utilizzi abitualmente i Remote Desktop Services (RDP) - anche conosciuti come terminal services se sei nel mondo dell'IT già da qualche anno - non dovresti mai chiudere una sessione remota con la X per fare più in fretta. Devi sapere che disconnettere la sessione remota in questo modo potrebbe permettere ad un malintenzionato di dirottare la sessione remota disconnessa ed effettuare privileged escalation di utenti privilegiati.

## Sono le cattive abitudini a causare i peggiori disastri.
Come può un malintenzionato dirottare una sessione remota e prenderne il controllo? Se cercate su Google le parole RDP hijacking capirete che probabilmente è piu difficile da spiegare che farsi.
La tecnica più comune per dirottare una sessione RDP utilizza l'utilità nativa di Microsoft tscon.exe. Utilizzando tscon con i giusti privilegi permette di prendere possesso di un'altra sessione (attiva o disconnessa) senza inserire password e, se si è fortunati, di effettuare privileged escalation lasciando pochissime tracce.
L'anello debole della catena siamo sempre noi che lasciamo le sessioni amministrative aperte o disconnesse ma non terminate.

![immagine](assets/2024-02-13/korznikov.png)



## Bibliografia
https://www.csoonline.com/article/569621/rdp-hijacking-attacks-explained-and-how-to-mitigate-them.html
https://doublepulsar.com/rdp-hijacking-how-to-hijack-rds-and-remoteapp-sessions-transparently-to-move-through-an-da2a1e73a5f6
https://www.youtube.com/watch?v=bbTfN5geSKw
