---
title: Step by Step - Configurare NTP server su controller di dominio Active Directory
date: 2025-01-02
categories: [blogging, tutorial]
tags: [microsoft]
description: 
image:
     path: 
---
Quando l'orologio fa tictac, la rete aziendale (network) non può restare indietro sopratutto quando arrivano le 18:00 e vuoi uscire dall'ufficio per tornare a casa a rilassarti. La sincronizzazione accurata dell'ora all'interno della network è fondamentale per garantire la coerenza tra i sistemi, facilitare la risoluzione dei problemi e mantenere alto il livello di sicurezza. 
In un dominio basato su Microsoft Active Directory, il controller di dominio che detiene il ruolo di emulatore Primary Domain Controller (PDC emulator) serve come riferimento autorevole per l'ora per i client del dominio. È quindi essenziale configurare correttamente il servizio Network Time Protocol (NTP) su questo server per sincronizzarsi con un'origine esterna affidabile.

## Configurare NTP server su PDC emulator:

### Creare un Filtro WMI per Identificare l'Emulatore PDC:

- Apri "Group Policy Management" su un controller di dominio.
- Nel riquadro sinistro, premi con il tasto destro su "WMI Filters" e selezionare "New...".
- Assegna un nome al filtro, ad esempio "PDC Role".
- Fai clic su "Aggiungi" e assicurarti che "root\CIMv2" sia specificato come Namespace.
- Nel campo "Query", inserisci:

```sql
SELECT * FROM Win32_ComputerSystem WHERE DomainRole = 5
```
	 
- Premi su "OK" e poi su "Salva" per creare il filtro.

![Creazione filtro WMI](/assets/2025-01-02/image01.png) 

### Creare una GPO per la configurazione NTP:

- In "Group Policy Management", fai clic con il tasto destro su "Group Policy Object" e seleziona "New".
- Assegna un nome alla GPO, ad esempio "NTP Settings - Domain Controller PDC".
- Fai clic con il tasto destro sulla nuova GPO e seleziona "Edit...".
- Spostati alle policies:

```
Computer Configuration > Policies > Administrative Templates > System > Windows Time Service > Time Providers
```
- Modifica le voci: **Configure Windows NTP Client** ed **Enable Windows NTP Server** con i parametri che trovi nell'immagine sottostante o sostituendoli con il server NTP che preferisci:

![Policy NTP Server](/assets/2025-01-02/image02.png) 

- Assegna il filtro WMI creato al punto precedente alla GPO.

![Applicazione filtro WMI](/assets/2025-01-02/image03.png) 

- Seleziona l'unità organizzativa (OU) che contiene i domain controller e collega la policy appena creata.

### Forzare l'Aggiornamento dei Criteri di Gruppo:

- Aprire un prompt dei comandi con privilegi elevati sull'emulatore PDC e digitare:

```
gpupdate /force
```

- Riavviare il servizio Ora di Windows con i comandi:

```
net stop w32time
net start w32time
```

### Verificare la Configurazione:

- Per controllare lo stato del servizio Ora di Windows, eseguire:

```
w32tm /query /status
```

- Per verificare i peer configurati:

```
w32tm /query /peers
```

Questa configurazione garantisce che l'emulatore PDC sincronizzi l'ora con fonti esterne affidabili, mentre gli altri dispositivi nel dominio seguiranno la gerarchia interna per la sincronizzazione dell'ora. È importante monitorare periodicamente la sincronizzazione dell'ora per assicurarsi che tutti i sistemi mantengano l'accuratezza temporale necessaria.