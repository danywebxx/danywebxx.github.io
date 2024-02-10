---
layout: post
title:  "Allargare il disco di Ubuntu creato da Hyper-V"
date:   2024-02-10 15:00:00 +0100
categories: [blogging, tutorial]
tags: [appunti, file-system, hyper-v, microsoft, linux] 
---
Creare una macchiba virtuale utilizzato la funzione `Creazione Rapida` di Microsoft `Hyper-V` è molto semplice. In pochi minuti si riesce ad avere un computer Linux `Ubuntu` funzionante a disposizione per lavorare o sviluppare.

![Creazione Rapida di Hyper-V](/assets/2024-02-10/VMCreate_3lDrrOdvQh.png)
_Creazione Rapida di Hyper-V_

## Hyper-V - Creazione rapida macchina virtuale
Con questo metodo di installazione della macchina virutale si possono selezionare pochi parametri di configurazione e tra questi non è presente la dimensione del disco virtuale.
Hyper-V crea per Ubuntu un disco di 13GB che si esaurisce molto rapidamente se si installano software e tools particolarmente avidi di spazio. Il sottoscritto ammette di esserci cascato in pieno!

Fortunatamente allargare il disco con le guide giuste non è particolarmente complicato e non comporta particolari rischi. Vediamo come farlo in pochi passaggi.

## Allargare il disco virtuale del s.o. Ubuntu

Non è possibile estendere il disco virtuale a caldo ma è necessario spegnere la macchina virtuale prima di proseguire con l'espansione del disco.

![Espandere il disco virtuale](/assets/2024-02-10/mmc_ds1wsWAche.png)
_Espandere il disco virtuale_

Ora è possibile riaccendere Ubuntu ed installare le utility necessarie aper proseguire.

```bash
sudo apt install cloud-guest-utils
```

Nell'ordine andrà poi estesa la partizione sda1 nello spazio libero (attenzione allo spazio tra sda >> 1).

```bash
sudo growpart /dev/sda 1
```

Si conclude estendendo il file-system (sda1 senza spazio): 

```bash
sudo resize2fs /dev/sda1
```

Ora Ubuntu ha spazio a disposizione per funzionare correttamente!

### Fonti: 
[Linguist.it](https://linguist.is/2020/08/12/expand-ubuntu-disk-after-hyper-v-quick-create/)
