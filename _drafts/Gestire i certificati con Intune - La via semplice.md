---
title: Utilizzare Intune per la distribuzione di certificati on-premise - La via PKCS
date: 2024-10-26
categories: [blogging, tutorial]
tags: [m365, intune]
description: 
image:
     path: 
---

La verifica delle identità aziendali per l'accesso VPN o Wi-Fi basata su certificati digitali è considerata sicura da utilizzare con una **Certificate Authority (CA)** che garantisce la catena di fiducia. Grazie a **Microsoft Intune** è possibile distribuire i certificati digitali gestiti dalla (CA) on-premises, integrata in Active Directory, anche su dispositivi remoti non connessi alla rete aziendale. 

Intune per la gestione di questi certificati utilizza un connettore **Intune Certificate Connector (ICC)** installato su Windows Server nella rete interna e permette diversi scenari di utilizzo:

- **PKCS**: l'ICC installato on-premises gestisce la richiesta dei certificati alla CA e non è necessario che i dispositivi abbiano visibilità diretta con quest'ultima. La chiave privata è generata e protetta sul dispositivo, senza essere esportabile.  
- **SCEP/NDES**: necessità di**Network Device Enrollment Service (NDES)** e di ICC per distribuire i certificati con visibilità diretta tra i dispositivi e il server NDES. Ideale per scenari di distribuzione di massa, ma meno sicuro rispetto a PKCS, poiché richiede una connessione diretta alla CA.

PKCS risulta più semplice da implementare:
- non espone servizi all'esterno e riduce la superficie di attacco
- non necessita di servizi aggiuntivi da installare sui server

Per questi motivi in questo laboratorio utilizzerò questo scenario di utilizzo.

## PKCS
La richiesta e la distribuzione dei certificati avviene in pochi e semplici passaggi:

![](/assets/2024-10-26/image16.png)

1. Il dispositivo contatta intune   
2. Intune inoltra la richiesta ad **Intune Certificate Connector (ICC)**  
3. Il connettore crea la coppia chiave pubblica/privata da mandare alla CA on-premise.  
4. Il connettore manda i certificati firmati dalla CA ad Intune  
5. Intune li consegna al dispositivo richiedente

## Installazione di Intune Certificate Connector

### Creazione Account di servizio

ICC ha bisogno per funzionare di un **account di servizio**. Questo account verrà usato da ICC per la generazione della richiesta di certificati e l’invio alla CA.   
Purtroppo ad oggi ICC non supporta i *managed service account* ed è quindi necessario che crei un’utenza dedicata *non privilegiata* ma con *password complessa*. Questa utenza dovrà avere la possibilità di autenticarsi come servizio (**logon as a service**) sul server su cui installerai ICC.

![](/assets/2024-10-26/image21.png)

Semplici permessi da **Domain Users**

![](/assets/2024-10-26/image25.png)

### Installazione agente ICC

Dal **Portale di Intune** spostati in **Tenant Admin** > **Connectors and tokens** > **Certificate connectors**  
Premi **add** per scaricare il setup di ICC che dovrai installare sul server.  

![](/assets/2024-10-26/image11.png)

![](/assets/2024-10-26/image27.png)

Installa ICC con privilegi amministrativi

![](/assets/2024-10-26/image29.png)

Al termine dell’installazione, premi **Configura ora** per iniziare la configurazione id ICC.  

![](/assets/2024-10-26/image17.png)

Seleziona tra le opzioni disponibili **PKCS** 

![](/assets/2024-10-26/image24.png)

Inserisci ora i dati dell’utenza di servizio che ti ho menzionato precedentemente e che ti invito a creare se non lo avessi ancora fatto.  

![](/assets/2024-10-26/image15.png)

Assicurati che tutti i requisiti siano soddisfatti.  

![](/assets/2024-10-26/image9.png)

Inserisci l’utenza M365 necessaria alla registrazione del connettore nel portale di Intune  

![](/assets/2024-10-26/image5.png)

![](/assets/2024-10-26/image19.png)

Se tutto sarà andato nel modo corretto troverai il connettore registrato nel portale.

![](/assets/2024-10-26/image18.png)

## Creazione dei template per i certificati

La parte complicata e più delicata di tutto il percorso è la creazione dei template dei certificati. Questi dovranno riportare le informazioni che sono necessarie alla tua organizzazione.  

### Modello Utente

All’interno della CA, parti duplicando il modello di template **User**
  
![](/assets/2024-10-26/image14.png)

Importante che il nome che darai al template non riporti spazi. Ho letto in diverse guide che l’utilizzo di spazi nei nomi dei template causa problemi.  
![](/assets/2024-10-26/image6.png)

Nella sezione **Subject Name** seleziona **Supply in the request**. Sarà Intune stessa a fornire i dati necessari alla compilazione del certificato.  
![](/assets/2024-10-26/image20.png)

**IMPORTANTE:** rendi la chiave privata esportabile. Questa sarà esportabile solo per ICC e non per il dispositivo o l’utente che riceverà il certificato finale.{: .prompt-danger }

![](/assets/2024-10-26/image31.png)

Assegna all’account di servizio i giusti permessi sul template. Bastano quelli di **Read** ed **Enroll**  

![](/assets/2024-10-26/image7.png)

### Certificato Computer

Vale la stessa procedura del paragrafo precedente. Unica differenza dovrai clonare il template **computer**  

![](/assets/2024-10-26/image1.png)

![](/assets/2024-10-26/image2.png)

![](/assets/2024-10-26/image26.png)

![](/assets/2024-10-26/image22.png)
 
![](/assets/2024-10-26/image20.png)

Rendi attivi ora i template creati in precedenza.  
![](/assets/2024-10-26/image12.png)

## Creazione delle policy in Intune

### Trusted Root

La prima policy da creare è la distribuzione della **Trusted Root** e fare in modo che i client la considerino affidagile.  
Per prima cosa esporta il certificato root dalla CA: 

```prompt
certutil -ca.cert ca_name.cer
```

Salva il certificato esportato e usalo per la creazione della prima policy.  

![](/assets/2024-10-26/image30.png)

Importa la Trusted Root e seleziona come archivio di destinazione **“Computer certificate store \- Root”**  

![](/assets/2024-10-26/image3.png)  

Assegna poi la policy ad utenti o dispositivi.

### PKCS certificate

Distribuzione ora i tuoi certificati ad utenti e dispositivi.

Sempre nella CA con il comando   

```prompt
certutil -config --ping
```

esporta i dati che ti serviranno nella creazione della policy 
- FQDN  
- Nome della CA

![](/assets/2024-10-26/image28.png)

Le variabili corrette che potrai usare nella creazione dei certificati le puoi trovare qui: [https://learn.microsoft.com/en-us/mem/intune/protect/certificates-profile-scep](https://learn.microsoft.com/en-us/mem/intune/protect/certificates-profile-scep) 

### Richiesta certificato computer

![](/assets/2024-10-26/image8.png)

![](/assets/2024-10-26/image23.png)

![](/assets/2024-10-26/image4.png)

![](/assets/2024-10-26/image13.png)

### Richiesta certificato utente
  
![](/assets/2024-10-26/image10.png)

## Bibliografia

- [Installazione NDES](https://learn.microsoft.com/it-it/mem/intune/protect/certificates-scep-configure?WT.mc_id=Portal-Microsoft_Intune_DeviceSettings#set-up-ndes)  
- [Creazione account di servizio NDES](https://learn.microsoft.com/it-it/windows-server/identity/ad-cs/create-domain-user-account-ndes-service-account)  
- [Installare connettore intune](https://learn.microsoft.com/it-it/mem/intune/protect/certificate-connector-install)  
- [Hardening per account dedicato ad Intune](https://directaccess.richardhicks.com/2023/01/30/intune-certificate-connector-service-account-and-pkcs/)  
- [Creazione del trusted root certificate profile](https://learn.microsoft.com/en-us/mem/intune/protect/certificates-trusted-root)  
- [Esportare il certificato root](https://learn.microsoft.com/it-it/troubleshoot/windows-server/certificates-and-public-key-infrastructure-pki/export-root-certification-authority-certificate)  
- [Lista delle variabili](https://learn.microsoft.com/en-us/mem/intune/protect/certificates-profile-scep)