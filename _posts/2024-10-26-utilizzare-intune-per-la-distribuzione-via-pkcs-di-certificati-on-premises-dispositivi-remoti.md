---
title: Utilizzare Intune per la distribuzione via PKCS di certificati on-premises su dispositivi remoti
date: 2024-10-26
categories: [blogging, tutorial]
tags: [m365, intune]
description: L'articolo esplora come distribuire certificati digitali gestiti da una Certification Authority (CA) on-premises per l'autenticazione VPN o Wi-Fi, anche su dispositivi remoti, grazie a Microsoft Intune.
image:
     path: /assets/2024-10-26/image00.png
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

ICC ha bisogno per funzionare di un **account di servizio** e questo account verrà usato per la generazione della richiesta di certificati e l’invio alla CA. Purtroppo ICC non supporta i *managed service account* ed è quindi necessario che crei un’utenza dedicata *non privilegiata* ma con *password complessa*. Questa utenza dovrà avere la possibilità di autenticarsi come servizio (**logon as a service**) sul server su cui installerai ICC.

![](/assets/2024-10-26/image21.png)

Assegnagli semplici permessi da **Domain Users**

![](/assets/2024-10-26/image25.png)

### Installazione agente ICC

Dal **Portale di Intune** spostati in **Tenant Admin** > **Connectors and tokens** > **Certificate connectors**  
e premi **add** per scaricare il setup di ICC.  

![](/assets/2024-10-26/image11.png)

![](/assets/2024-10-26/image27.png)

Esegui l'installazione di ICC con privilegi amministrativi.

![](/assets/2024-10-26/image29.png)

Al termine dell’installazione, premi **Configura ora** per iniziare la configurazione id ICC.  

![](/assets/2024-10-26/image17.png)

Seleziona tra le opzioni disponibili **PKCS** 

![](/assets/2024-10-26/image24.png)

Inserisci i dati dell’utenza di servizio creata in precedenza. 

![](/assets/2024-10-26/image15.png)

Assicurati che tutti i requisiti siano soddisfatti. Un messaggio ti avviserà in caso contrario con le correzioni da fare.

![](/assets/2024-10-26/image9.png)

Autenticati sul portale M365 utilizzando un utente che abbia i privilegi necessari alla registrazione del connettore nel portale di Intune. Usa un *Global Admin* o un *Intune administrator*.

![](/assets/2024-10-26/image5.png)

![](/assets/2024-10-26/image19.png)

Se visualizzi il connettore ICC nel portale di Intune tutto sarà andato come da programma.

![](/assets/2024-10-26/image18.png)

## Creazione dei template per i certificati

La parte complicata e più delicata di tutta la procedura è la creazione dei template per i certificati. Questi dovranno necessariamente riportare le informazioni per validare le utenze alla tua organizzazione.  

### Modello Utente

In questa prima parte creerai un template per i certificati che saranno distribuiti agli utenti. All’interno della CA, inizia duplicando il template **User**
  
![](/assets/2024-10-26/image14.png)

Assicurati di non inserire spazi nel nome che darai al nuovo template. Ho letto in diverse guide che l’utilizzo di spazi nei nomi causa problemi.

![](/assets/2024-10-26/image6.png)

Nella sezione **Subject Name** seleziona **Supply in the request**. Sarà Intune a fornire i dati necessari alla compilazione del certificato prendendoli da una serie di variabili che configureremo nel prossimo paragrafo.

![](/assets/2024-10-26/image20.png)

**IMPORTANTE:** rendi la chiave privata esportabile. Se hai dubbi sulla sicurezza sappi che lo sarà soloper ICC e non per il dispositivo o l’utente che riceverà il certificato finale.{: .prompt-danger }

![](/assets/2024-10-26/image31.png)

Nella sezione **Security** del nuovo template assegna all’account di servizio i permessi di **Read** ed **Enroll**.

![](/assets/2024-10-26/image7.png)

### Certificato Computer

Per creare un template da utilizzare per i computer vale la stessa procedura del paragrafo precedente ma dovrai duplicare il template **computer**  

![](/assets/2024-10-26/image1.png)

![](/assets/2024-10-26/image2.png)

![](/assets/2024-10-26/image26.png)

![](/assets/2024-10-26/image22.png)
 
![](/assets/2024-10-26/image20.png)

Ora non ti resta che rendere disponibili i template appena creati agli utenti finali (che in questo caso sarà il service account usato da ICC).

![](/assets/2024-10-26/image12.png)

## Creazione delle policy in Intune

L'ultimo passaggio di questa lunga guida consiste nella creazione delle policy per la distribuzione dei certificati all'interno del portale amministrativo di Intune.

### Trusted Root

La prima policy che dovrai creare distribuirà la **Trusted Root** a dispositivi o utenti e farà in modo che la considerino affidabile.  
Per prima cosa esporta la chiave pubblica del certificato root dalla CA: 

```prompt
certutil -ca.cert ca_name.cer
```

Salva il certificato esportato e usalo per la creazione della prima policy. Spostati nella sezione **Device** > **Configuration**  

![](/assets/2024-10-26/image30.png)

Importa la Trusted Root e seleziona come archivio di destinazione **Computer certificate store - Root**  

![](/assets/2024-10-26/image3.png)  

Assegna poi la policy ad un gruppo di dispositivi o utenti.

### PKCS certificate

Ora crea una configurazione per distribuire i certificati ad utenti e dispositivi.

Sempre nella CA esegui il comando:   

```prompt
certutil -config --ping
```

Esporta i dati che ti serviranno per la creazione della policy:
- FQDN della CA
- Nome della CA

![](/assets/2024-10-26/image28.png)

Come ti accennavo in precedenza, durante la creazione dei certificti potrai usare alcune variabili per la creazione dei soggetti e dei soggetti alternativi; l'elenco completo lo potrai trovare a questo [link](https://learn.microsoft.com/en-us/mem/intune/protect/certificates-profile-scep) 

### Richiesta per certificati di tipo computer

Crea una profilo per dispositivi **Windows 10 and later** > **Templates** > **PKCS certificate**

![](/assets/2024-10-26/image8.png)

![](/assets/2024-10-26/image23.png)

Configura i valori che ritieni corretti per la tua organizzazione per la durata e tempo il periodo di rinnovo; nello spazio **Certification authority** inserisci il nome FQDN della CA e in **Certification authority name** il nome dato alla CA stessa. Entrambi i parametri li hai visualizzati e copiati precedentemente con il comando cli.
E' importante che per **Certificate template name** inserisci lo stesso nome che hai dato ai template creati precedentemente. Seleziona come tipo di certificato **Device**.

![](/assets/2024-10-26/image4.png)

![](/assets/2024-10-26/image13.png)

### Richiesta per certificati di tipi utente

Vale la stessa procedura per la configurazione dei certificati Device ma questa volta come **Type** seleziona **User**.
Usa le variabili che ritieni più adatte alla tua organizzazione.

![](/assets/2024-10-26/image10.png)

## Bibliografia

Ecco alcuni degli articoli che ho letto per la creazione di questa guida e la configurazione del mio ambiente PoC. Ce ne sono molti altri ma questi potrebbero esservi moto di aiuto:

- [Oceanleaf è stata la svolta per comprendere le differenze tra le varie distribuzioni](https://www.oceanleaf.ch/intune-certificate-management/)
- [Differenza tra certificati PKCS e SCEP](https://learn.microsoft.com/it-it/mem/intune/protect/certificates-configure)
- [Installazione NDES](https://learn.microsoft.com/it-it/mem/intune/protect/certificates-scep-configure?WT.mc_id=Portal-Microsoft_Intune_DeviceSettings#set-up-ndes)  
- [Creazione account di servizio NDES](https://learn.microsoft.com/it-it/windows-server/identity/ad-cs/create-domain-user-account-ndes-service-account)  
- [Installare connettore intune](https://learn.microsoft.com/it-it/mem/intune/protect/certificate-connector-install)  
- [Hardening per account dedicato ad Intune](https://directaccess.richardhicks.com/2023/01/30/intune-certificate-connector-service-account-and-pkcs/)  
- [Creazione del trusted root certificate profile](https://learn.microsoft.com/en-us/mem/intune/protect/certificates-trusted-root)  
- [Esportare il certificato root](https://learn.microsoft.com/it-it/troubleshoot/windows-server/certificates-and-public-key-infrastructure-pki/export-root-certification-authority-certificate)  
- [Lista delle variabili](https://learn.microsoft.com/en-us/mem/intune/protect/certificates-profile-scep)