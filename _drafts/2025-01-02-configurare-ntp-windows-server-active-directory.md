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

**Configurare NTP server su PDC emulator:**

1. ***Creare un Filtro WMI per Identificare l'Emulatore PDC:***

- Aprire "Group Policy Management" su un controller di dominio.
- Nel riquadro sinistro, fare clic con il tasto destro su "WMI Filters" e selezionare "New...".
- Assegnare un nome al filtro, ad esempio "PDC Role".
- Fare clic su "Aggiungi" e assicurarsi che "root\CIMv2" sia specificato come Namespace.
- Nel campo "Query", inserire:

```sql
SELECT * FROM Win32_ComputerSystem WHERE DomainRole = 5
```
	 
- Fare clic su "OK" e poi su "Salva" per creare il filtro.

![Creazione filtro WMI](/assets/2025-01-02/image01.png) 

2. ***Creare una GPO per la configurazione NTP**

- In "Group Policy Management", fare clic con il tasto destro su "Group Policy Object" e selezionare "New".
- Assegnare un nome al GPO, ad esempio "NTP Settings - Domain Controller PDC".
- Fare clic con il tasto destro sul nuovo GPO e selezionare "Edit...".
- spostarsi alle policies:

```
Computer Configuration > Policies > Administrative Templates > System > Windows Time Service > Time Providers
```

- Modificare le Policy: *Configure Windows NTP Client* ed *Enable Windows NTP Server* con i parametri che trovate nell'immagine sottostante o sostituenmdoli con il server NTP che preferite:

![Policy NTP Server](/assets/2025-01-02/image02.png) 

- Assegnate come ultimo il filtro WMI creato al punto (1) alla GPO.

![Applicazione filtro WMI](/assets/2025-01-02/image03.png) 


3. ***Collegare il GPO all'Unità Organizzativa (OU) Appropriata:***

- In "Gestione Criteri di Gruppo", fare clic con il tasto destro sull'OU che contiene i controller di dominio e selezionare "Collega un GPO esistente".
- Selezionare il GPO creato in precedenza e fare clic su "OK".
- Associare il filtro WMI creato al GPO per garantire che le impostazioni si applichino solo all'emulatore PDC.

4. ***Forzare l'Aggiornamento dei Criteri di Gruppo:***

- Aprire un prompt dei comandi con privilegi elevati sull'emulatore PDC e digitare:

```
gpupdate /force
```

- Riavviare il servizio Ora di Windows con i comandi:

```
net stop w32time
net start w32time
```

5. ***Verificare la Configurazione:***

- Per controllare lo stato del servizio Ora di Windows, eseguire:

```
w32tm /query /status
```

- Per verificare i peer configurati:

```
w32tm /query /peers
```

Questa configurazione garantisce che l'emulatore PDC sincronizzi l'ora con fonti esterne affidabili, mentre gli altri dispositivi nel dominio seguiranno la gerarchia interna per la sincronizzazione dell'ora. È importante monitorare periodicamente la sincronizzazione dell'ora per assicurarsi che tutti i sistemi mantengano l'accuratezza temporale necessaria.