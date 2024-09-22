---
title: Replica tra Azure Blob Storage
date: 2024-09-11
categories: [blogging, appunti]
tags: [microsoft, azure, storage]
description: Azure Blob Storage supporta la replica asincrona tra oggetti di tipo block blobs permettendo la realizzazione discenari interessanti per la ridondanza e il backup dei dati.
image:
     path: /assets/2024-09-11/image7.png
---

**Azure Blob Storage** supporta la replica asincrona tra oggetti di tipo **block blobs** e permette la realizzazione di alcuni scenari interessanti: 

- *Geo-redundancy:* replicare blob su su storage account in regioni differenti permette di avere copia del dato in diverse parti del mondo. Questo aumenta la disponibilità del dato se la regione Azure primaria dovesse venire a mancare.  
- *Minor tempo di accesso al dato:* replicare il dato in regioni Azure più vicine all’utente permette di diminuire tempi di accesso e latenza aumentando le performance di applicazioni web “attaccate” allo storage.  
- *Backup e Archivio:* gli oggetti replicati possono essere usati come backup, archivio ed eventualmente siti di disaster recovery.

## Creazione della replica blob

In questo laboratorio ho previsto la creazione di due storage account; uno primario che contiene il contenitore blob sorgente ed uno secondario per il contenitore blob di backup.  
Creazione dello **storage account sorgente**:

![](/assets/2024-09-11/image5.png)

Creazione del **contenitore blob sorgente**  
![](/assets/2024-09-11/image3.png)

Creazione dello **storage account di replica** (per semplicità ho usato la stessa region ma nulla vieta di usarne diverse):  
![](/assets/2024-09-11/image4.png)

Creazione del **contenitore blob destinazione**!
![](/assets/2024-09-11/image6.png)

### Definire le regole di replica
Una volta creati gli storage account con relativi contenitore è il momento di attivare le regole di replica all’interno dello storage account sorgente.  
Crea le regole di replica nella sezione **Data management** \>  **Object replication** \> **Create replication rules**  
![](/assets/2024-09-11/image2.png)

I parametri che puoi selezionare non sono molti. **Destination subscription** e **Destination storage account**. Fatta questa prima selezione puoi indicare **Source Container** e **Destination Container**.

![](/assets/2024-09-11/image1.png)
Thats it\!

## Alcune considerazioni 

- *La replica è asincrona:* il processo di replica avviene in modalità asincrona e non è immediato. Non sono previsti tempi di replica garantiti per il completamento dell’attività. insomma, bisogna avere pazienza ma tramite gli strumenti di Azure Monitoring è possibile verificarne lo stato.  
- *Funziona solo con oggetti di tipo block blobs:* la replica non funziona con append blobs e page blobs.  
- *Versioning attivo:* sui blob deve essere attivo il versioning sia su quello sorgente che sulla destinazione.

## Bibliografia: 

- [Microsoft Learning - Object Replicazion Configuration](https://learn.microsoft.com/it-it/azure/storage/blobs/object-replication-configure?tabs=portal)  
- [Microsoft Learning - Laboratorio](https://github.com/MicrosoftLearning/Secure-storage-for-Azure-Files-and-Azure-Blob-Storage/blob/master/Instructions/Labs/LAB_02b_storage_private_docs.md)  