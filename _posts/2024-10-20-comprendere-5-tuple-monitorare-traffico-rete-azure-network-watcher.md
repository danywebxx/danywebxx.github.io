---
title: Comprendere i 5-Tuple e monitorare il traffico di rete in Azure con Network Watcher e Log Analytics
date: 2024-10-20
categories: [blogging, tutorial]
tags: [networking, security, audit]
description: Non esiste sicurezza se non si ha visibilità! Quando parliamo di traffico di rete e sicurezza, i 5-tuple sono fondamentali per comprendere come il flusso di dati scorre tra i dispositivi. In questo articolo, esploreremo cosa sono i 5-tuple, come abilitare Network Watcher in Azure per monitorare il traffico e come analizzare i dati attraverso Log Analytics.
image:
     path: /assets/2024-10-20/image9.png
---
## Cosa sono i 5-Tuple?

I **5-tuple** sono cinque parametri fondamentali utilizzati per identificare univocamente ogni flusso TCP/IP in una rete. Questi parametri definiscono la connessione tra due endpoint e includono:

* **Source IP**: l'indirizzo IP di origine del traffico, ossia il dispositivo che inizia la comunicazione.  
* **Source Port**: il numero di porta utilizzato dall'origine per inviare il traffico.  
* **Destination IP**: l'indirizzo IP di destinazione verso cui è diretto il traffico.  
* **Destination Port**: il numero di porta contattata sulla destinazione che riceve il traffico.  
* **Protocol**: il protocollo di comunicazione utilizzato (ad esempio, TCP, UDP o ICMP).

Queste informazioni vengono utilizzate per tracciare e analizzare le connessioni di rete, monitorare il traffico e applicare politiche di sicurezza come quelle configurate tramite un **Network Security Group (NSG)** in Azure o un comune firewall.

## Cos'è Azure Network Watcher?

**Azure Network Watcher (ANW)** è uno strumento di monitoraggio che consente agli amministratori di rete di raccogliere, diagnosticare e visualizzare il flusso di dati all'interno di una rete virtuale (VNet). Per fare ciò ANW utilizza i 5-tuple di cui abbiamo parlato poco fa.

## Come abilitare Azure Network Watcher

Per raccogliere i dati sui 5-tuple del traffico di rete in Azure, dobbiamo prima abilitare **Virtual network flow logs** sulla VNet (o anche su una singola subnet della VNet). Ecco come farlo:

### Passaggi per abilitare Virtual network flow logs sulla VNet

Per abilitare i **Virtual network flow logs** sulla **VNet** entra nella **VNet** desiderata e clicca **Virtual network flow log**.

![](/assets/2024-10-20/image3.png)

Crea un nuovo Flow Log.

![](/assets/2024-10-20/image7.png)

Come tipologia seleziona **Virtual Network** (i flow log per gli Network Security Group NSG saranno presto deprecati). 

![](/assets/2024-10-20/image4.png)

La procedura a questo punto ti chiederà di creare uno storage account per il salvataggio dei log:  

![](/assets/2024-10-20/image2.png)

Imposta una Retention per i dati sufficiente a future analisi. Nel mio caso 7 giorni sono sufficienti.  

![](/assets/2024-10-20/image6.png)

Nella sezione **Analytics** ho deciso di analizzare i dati sfruttando **Log Analytics**. Questa parte è facoltativa ma avere un account Log Analytics permette di una visualizzazione avanzata dei dati raccolti.  

![](/assets/2024-10-20/image5.png)

Rivedete i dati e create il flow log. A questo punto, i log di flusso VNet inizieranno a raccogliere i dati di traffico contenenti i 5-tuple di ogni connessione e verranno archiviati nell'account di archiviazione scelto.

Se sfogliate lo storage-account creato nella prima parte delle guifa vedrete dei dati archiviati per **mac-address** e per giorno in formato **json**.

![](/assets/2024-10-20/image8.png)

## Visualizzazione dei dati con Network Watcher

Sfruttando l’abbinamento Flow Logs e Log Analytics è possibile visualizzare ed analizzare i dati raccolti. Apri Network Watcher e seleziona Traffic Analytics; qui hai l’imbarazzo della scelta tra le opzioni disponibili.  

![](/assets/2024-10-20/image1.png)

## Analisi dei Dati con Log Analytics

Dopo che i dati sono stati raccolti, puoi utilizzare il **Kusto Query Language (KQL)** per analizzare i log. Un esempio di query per visualizzare il traffico consentito potrebbe essere:  
kql  
Copia codice  

```Kusto
NTANetAnalytics
| where SrcIp == "10.199.0.4" 
| where DestPort == "53"
```
Questo ti fornirà un riepilogo del traffico partito dalla VM con IP 10.199.0.4 che aveva come porta di destinazione la 53 (DNS).

## Bibliografia

- [Microsoft - Flow logs overview](https://learn.microsoft.com/en-us/azure/network-watcher/vnet-flow-logs-overview)  
- [Microsoft - Traffic Analytics FAQ](https://learn.microsoft.com/en-us/azure/network-watcher/traffic-analytics-faq)
