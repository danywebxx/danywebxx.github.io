---
title: Gestire i certificati on-premise con Intune - La via semplice
date: 2024-10-26
categories: [blogging, tutorial]
tags: [m365, intune]
description: 
image:
     path: 
---

Con **Microsoft Intune** è possibile installare sui dispositivi gestiti dall’organizzazione i certificati digitali utilizzati nei processi di autenticazione. Intune permette due scenari di implementazione differenti:

- **PKCS**: richiede l'**Intune Certificate Connector** (proxy) installato on-premises che gestisce la richiesta dei certificati alla **Certification Authority (CA)**. Non è necessario che i dispositivi abbiano visibilità diretta sulla rete interna. La chiave privata è generata e protetta sul dispositivo, senza essere esportabile.  
- **SCEP/NDES**: necessità del **Network Device Enrollment Service (NDES)** e di '**Intune Certificate Connector** per distribuire i certificati con visibilità diretta tra i dispositivi e il server NDES. Ideale per scenari di distribuzione di massa, ma meno sicuro rispetto a PKCS, poiché richiede una connessione diretta alla CA.

Questa differenziazione rende PKCS più semplice da implementare ed evita l'esposizione pubblica del server NDES. In questo articolo prenderemo in considerazione la distribuzione dei certificati PKCS.

## Scenario di implementazione

![](/assets/2024-10-25/image16.png)

1. Il dispositivo contatta intune   
2. Intune inoltra la richiesta ad **Intune Certificate Connector (ICC)**  
3. Il connettore crea la coppia chiave pubblica/privata da mandare alla CA on-premise.  
4. Il connettore manda i certificati firmati dalla CA ad Intune  
5. Intune li consegna al dispositivo richiedente

## Installazione di Intune Certificate Connector

### Creazione Account di servizio

ICC necessita di un **account di servizio** che verrà usato per la generazione dei certificati e l’invio alla CA per la firma.   
Purtroppo ICC non supporta i managed service account ed è quindi necessaria la creazione di un’utenza dedicata *non privilegiata* con password complessa. Questa utenza dovrà avere la possibilità di autenticarsi come servizio (**logon as a service)** sul server su cui è installato ICC.   
![][image2]  
![][image3]

### Installazione agent ICC

Da **portale di Intune** spostati in **Tenant Admin** \> **Connectors and tokens** \> **Certificate connectors**  
Premi **add** per scaricare il connettore che dovrai installare sul server che funge da proxy.  
   
![Immagine che contiene testo, schermata, software, Icona del computerDescrizione generata automaticamente][image4]  
![Immagine che contiene testo, schermata, CarattereDescrizione generata automaticamente][image5]  
Con privilegi amministrativi installa ora ICC

![Immagine che contiene testo, elettronica, schermata, CarattereDescrizione generata automaticamente][image6]

Al termine dell’installazione, premendo **Configura ora** comincerà la configurazione del proxy.  
![][image7]

La feature importante che dovrai essere certo di aver selezionato tra quelle proposte è **PKCS** 

   
   
![][image8]

Inserisci ora i dati dell’utenza di servizio creata in precedenza.  
   
![][image9]

Assicurati che tutti i requisiti siano soddisfatti.  
   
   
![Immagine che contiene testo, schermata, software, computerDescrizione generata automaticamente][image10]

Inserisci ora l’utenza necessaria alla registrazione del connettore al portale di Intune  
   
![Immagine che contiene testo, schermata, software, schermoDescrizione generata automaticamente][image11]

   
   
![][image12]

Se tutto sarà andato nel modo corretto troverai il connettore registrato nel portale.

   
![][image13]

## Creazione dei template per i certificati

La parte complicata e più delicata di tutto il percorso è la creazione dei template dei certificati. Questi dovranno riportare le informazioni che sono necessarie alla tua organizzazione.  
 

### Modello Utente

All’interno della CA, parti duplicando il modello di template **User**

   
![Immagine che contiene testo, schermata, numero, softwareDescrizione generata automaticamente][image14]  
   
Importante che il nome che darai al template non riporti spazi. Ho letto in diverse guide che l’utilizzo di spazi nei nomi dei template causa problemi.  
![][image15]

Nella sezione **Subject Name** seleziona **Supply in the request**. Sarà Intune stessa a fornire i dati necessari alla compilazione del certificato.  
![Immagine che contiene testo, elettronica, schermata, schermoDescrizione generata automaticamente][image16]

**IMPORTANTE:** rendi la chiave privata esportabile. Questa sarà esportabile solo per ICC e non per il dispositivo o l’utente che riceverà il certificato finale.  
   
![Immagine che contiene testo, schermata, Carattere, numeroDescrizione generata automaticamente][image17]

Assegna all’account di servizio i giusti permessi sul template. Bastano quelli di **Read** ed **Enroll**  
   
![Immagine che contiene testo, schermata, schermo, numeroDescrizione generata automaticamente][image18]  
 

### Certificato Computer

Vale la stessa procedura del paragrafo precedente. Unica differenza dovrai clonare il template **computer**  
   
![Immagine che contiene testo, schermata, numero, softwareDescrizione generata automaticamente][image19]  
   
![][image20]  
   
![Immagine che contiene testo, schermata, Carattere, numeroDescrizione generata automaticamente][image21]  
   
![Immagine che contiene testo, schermata, schermo, softwareDescrizione generata automaticamente][image22]  
   
![Immagine che contiene testo, elettronica, schermata, schermoDescrizione generata automaticamente][image16]

Rendi attivi ora i template creati in precedenza.  
![Immagine che contiene testo, schermata, Carattere, softwareDescrizione generata automaticamente][image23]

## Creazione delle policy in Intune

### Trusted Root

La prima policy da creare è la distribuzione della **Trusted Root** e fare in modo che i client la considerino affidagile.  
Per prima cosa esporta il certificato root dalla CA: 

``` prompt
certutil -ca.cert ca_name.cer
```

 Salva il certificato esportato e usalo per la creazione della prima policy.  
![Immagine che contiene testo, schermata, software, Pagina WebDescrizione generata automaticamente][image24]  
 Importa la Trusted Root e seleziona come archivio di destinazione **“Computer certificate store \- Root”**  
   
![Immagine che contiene testo, software, Carattere, Pagina WebDescrizione generata automaticamente][image25]  
Assegna poi la policy ad utenti o dispositivi.  
 

### PKCS certificate

Distribuzione ora i tuoi certificati ad utenti e dispositivi.

Sempre nella CA con il comando   

``` prompt
certutil -config -- ping
```

esporta i dati che ti serviranno nella creazione della policy 

- FQDN  
- Nome della CA

   
![Immagine che contiene testo, schermata, CarattereDescrizione generata automaticamente][image26]

Le variabili corrette che potrai usare nella creazione dei certificati le puoi trovare qui: [https://learn.microsoft.com/en-us/mem/intune/protect/certificates-profile-scep](https://learn.microsoft.com/en-us/mem/intune/protect/certificates-profile-scep) 

### Richiesta certificato computer

   
   
![Immagine che contiene testo, schermata, Carattere, numeroDescrizione generata automaticamente][image27]  
   
![Immagine che contiene testo, schermata, Carattere, numeroDescrizione generata automaticamente][image28]  
   
![Immagine che contiene testo, schermata, numero, CarattereDescrizione generata automaticamente][image29]  
   
![Immagine che contiene testo, Carattere, numero, schermataDescrizione generata automaticamente][image30]  
 

### Richiesta certificato utente
   
![Immagine che contiene testo, schermata, numero, CarattereDescrizione generata automaticamente][image31]

## Bibliografia
    • https://learn.microsoft.com/it-it/mem/intune/protect/certificates-scep-configure?WT.mc_id=Portal-Microsoft_Intune_DeviceSettings#set-up-ndes
    • Installazione NDES - https://learn.microsoft.com/it-it/mem/intune/protect/certificates-scep-configure?WT.mc_id=Portal-Microsoft_Intune_DeviceSettings#set-up-ndes
    • Creazione account di servizio NDES - https://learn.microsoft.com/it-it/windows-server/identity/ad-cs/create-domain-user-account-ndes-service-account
    • Installare connettore intune https://learn.microsoft.com/it-it/mem/intune/protect/certificate-connector-install
    • Hardening per account dedicato ad Intune: https://directaccess.richardhicks.com/2023/01/30/intune-certificate-connector-service-account-and-pkcs/
    • Creazione del trusted root certificate profile https://learn.microsoft.com/en-us/mem/intune/protect/certificates-trusted-root
    • Esportare il certificato root https://learn.microsoft.com/it-it/troubleshoot/windows-server/certificates-and-public-key-infrastructure-pki/export-root-certification-authority-certificate
    • Lista delle variabili https://learn.microsoft.com/en-us/mem/intune/protect/certificates-profile-scep