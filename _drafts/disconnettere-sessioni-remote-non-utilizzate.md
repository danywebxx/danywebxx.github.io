---
layout: post
title:  "Perchè non dovresti mai chiudere con la X una sessione remota RDP"
date:   2024-02-13 16:00:00 +0100
categories: [blogging, security]
tags: [appunti, rdp, microsoft, hardening] 
---
Se utilizzi abitualmente i Remote Desktop Services (RDP) - anche terminal services se sei nel mondo dell'IT già da qualche anno -  non dovresti mai chiudere una sessione remota con la X del banner blu sulla parte alta dello schermo. Devi sapere che disconnettere la sessione remota in questo modo potrebbe permettere ad un malintenzionato di dirottare la sessione remota disconnessa ed effettuare privileged escalation di utenti privilegiati.

## Sono le cattive abitudini a causare i peggiori disastri.
Come può un malintenzionato dirottare una sessione remota e prenderne il controllo?


## Bibliografia
https://www.csoonline.com/article/569621/rdp-hijacking-attacks-explained-and-how-to-mitigate-them.html
https://doublepulsar.com/rdp-hijacking-how-to-hijack-rds-and-remoteapp-sessions-transparently-to-move-through-an-da2a1e73a5f6
https://www.youtube.com/watch?v=bbTfN5geSKw
