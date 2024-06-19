---
title: Active Directory Assessment con Purple Knight
date: 2024-04-01
categories: [blogging, security]
tags: [cloud, microsoft, hardening, audit]
description: Utilizzando Purple Knight è possibile effettuare un assessment di sicurezza di Active Directory con lo scopo di ridurre gli indicatori di espostizione (IeE) e gli indicatori di compromissione (IoC) 
image:
     path: /assets/2024-04-01/purple-feature.png
---

Active Directory è lo strumento di autenticazione ed autorizzazione utilizzato dal 90% delle aziende in tutto il mondo. Essendo la sua popolarità così elevata aumenta esponenzialmente la probabilità che venga attaccata da agenti esterni per minarne la sicurezza. Ne consegue che ridurre al minimo la superficie di attacco limita la probabilità che venga colpita con successo da un attacco informatico. Per raggiungere questo obiettivo è necessario effettuare periodicamente un assessment del suo stato e rimediare ad eventuali indicatori di esposizione rilevati.

## Cos’è Purple Knight
Le vulnerabilità di Active Directory, Microsoft Entra ID ed Okta potrebbero dare ad un attaccante un accesso illimitato alle risorse dell’organizzazione. 
Semperis ha creato `Purple Knight` (PK), un tool gratuito di assessment per la sicurezza che aiuta a rilevare gli indicatori di esposizioni (`IoE`) e gli indicatori di compromissione (`IoC`) per gli ambienti elencati sopra.
Per i più scettici all’utilizzo di tool gratuiti per gli assessment posso anticipare che gli script che vengono eseguiti sul sistema oggetto di assessment possono essere tranquillamente letti ed ispezionati.

## Assessment
Per eseguire l'assessment di Active Directory seguire questi step.

### Step 1 - Scaricare Purple Knight
Scaricare Purple Knight dal sito di Semperis. Il software scaricato andrà copiato sul domain controller oggetto di assessment [Pagina per il download di Purple Knigh Community](https://www.semperis.com/purple-knight/resources/).

![Purple Knight](/assets/2024-04-01/Immagine1.png)
_Semperis Purple Knight_

Come anticipavo qualche riga sopra, il tool PK andrà copiato ed eseguito sul domain controller oggetto di assessment.

![Purple Knight Community](/assets/2024-04-01/Immagine2.png)
_File compresso contenente Purple Knight Community appena scaricato_

### Step 2 - Sbloccare i file con Powershell
Avendo scaricato i file di PK da internet è importante [eseguire lo sblocco dei file scaricati utilizzando Powershell](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/unblock-file?view=powershell-7.4):

```powershell
Get-ChildItem -Path "C:\Install\PurpleKnight-Community\PK Community 4.2" -Recurse | Unblock-File
```

![Sblocco dei file](/assets/2024-04-01/Immagine3.png)
_Esecuzione del comando Powershell per lo sblocco dei file_

### Step 3 - Eseguire Purple Knight

Non ci resta che eseguire l'eseguibile PurpleKnight.exe contenuto nella cartella di cui sopra.

![Eseguire PK](/assets/2024-04-01/Immagine4.png)
_Eseguire PurpleKnight.exe_

Schermata di avvio:

![Schermata di avvio](/assets/2024-04-01/Immagine5.png)
_Schermata di avvio_

Prima di proseguire è necessario accettare il License Agreement

![License Agreement](/assets/2024-04-01/Immagine6.png)
_Schermata con il License Agreement_

Verificare la presenza di aggiornamenti di programma e indicatori:

![Check for Updates](/assets/2024-04-01/Immagine7.png)
_Verificare la presenza di aggiornamenti_

Aggioramento degli indicatori di sicurezza in corso:
![Check for Updates](/assets/2024-04-01/Immagine8.png)
_Aggiornamento degli indicatori di sicurezza_

![Check for Updates](/assets/2024-04-01/Immagine9.png)
_Aggiornamento completato_

Nella cartella script sono contenuti tutti quelli usate da PK nell’attività di assessment. Chi volesse può ispezionarne il contenuto per verificarne la sicurezza.

![Scripts folder](/assets/2024-04-01/Immagine10.png)
_Cartella che contiene gli scripts utilizzati da PK_

Scopo di questo assessment è Active Dictory di Microsoft. Selezionare la foresta e i domini interessati all’assessment:

![Selezione di foresta e dominio AD](/assets/2024-04-01/Immagine11.png)
_Selezione di foresta e dominio AD_

Tutti gli indicatori sono selezionati come impostazione predefinita ad eccezione della `Zerologon vulnerability`. 
> L'assessment di `Zerologon vulnerability` in un ambiente di produzione potrebbe richiedere diverse ore di esecuzione. Il mio consiglio è di eseguire un primo assessment senza la vulnerabilità Zerologon e solo successivamente eseguirne uno dedicato a tale scopo.
{: .prompt-info }

![Rivedere gli indicatori inclusi nella scansione](/assets/2024-04-01/Immagine12.png)
_Rivedere gli indicatori inclusi nella scansione_

Il tempo di esecuzione per la scansione di default è di circa due munuti.
![Stato della scansione](/assets/2024-04-01/Immagine13.png)
_Stato della scansione_

Risultato dell'assessment con "valutazione" scolastica.
![Risultato dell'assessment](/assets/2024-04-01/Immagine14.png)
_Risultato dell'assessment_

## Esaminare il report
Il report HTML che appare visualizza Active Directory Security Assessment. Quest oreport va studiato nel dettaglio affinchè vengano poi applicate le opportune misure correttive.

![Risultato dell'assessment](/assets/2024-04-01/Immagine15.png)
_Esaminare il risultato dell'assessment_

Un esempio di uno dei tanti IoE trovati in esaminati in Active Directory. <strong>Privileged Account with a password that never expires</strong>.

![Dettaglio di un IoC](/assets/2024-04-01/Immagine16.png)
_Dettaglio di un IoC_

## Conclusioni
Purple Knight effettua l'assessment molto velocemente e restituisce risultati (vulnerabilità) che non ci si aspettava di avere. Questo genere di tool dovrebbe essere eseguito a cadenza regolare durante l'anno per convalidare la sicurezza dell'organizzazione. Ripetere il test ogni qualvolta avvengano dei cambiamente all'ambiente che potrebbero aumentare la superficie di attacco di Active Directory.

## Bibliografia
- Ali Tajran [How to Create an Active Directory Security Assessment report](https://www.alitajran.com/active-directory-security-assessment/)



