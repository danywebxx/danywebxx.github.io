---
title: Utilizzare Intune per la distribuzione via PKCS di certificati on-premises su dispositivi remoti
date: 2024-10-26
categories: [blogging, tutorial]
tags: [m365, intune]
description: L'articolo esplora come distribuire certificati digitali gestiti da una Certification Authority (CA) on-premises per l'autenticazione VPN o Wi-Fi, anche su dispositivi remoti, grazie a Microsoft Intune.
image:
     path: /assets/2024-10-26/image00.png
---

Quando l'orologio fa tictac, la rete aziendale (network) non può restare indietro sopratutto quando alle 18:00 vuoi uscire dall'ufficio per tornare a casa a rilassarti. La sincronizzazione accurata dell'ora all'interno di un dominio **Active Directory** (AD) è fondamentale per garantire la coerenza tra i sistemi, facilitare la risoluzione dei problemi e mantenere alto il livello di sicurezza. 
In un dominio AD, il controller di dominio che detiene il ruolo di emulatore Primary Domain Controller (**PDC emulator**) serve come riferimento autorevole per l'ora per i client del dominio; è quindi essenziale configurare correttamente il servizio Network Time Protocol (**NTP**) su questo server affinchè possa  sincronizzarsi con un'origine esterna affidabile.

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

- Apri un prompt dei comandi con privilegi elevati sul PDC emulator e digita:

```
gpupdate /force
```

- Riavviare il servizio Ora di Windows con i comandi:

```
net stop w32time
net start w32time
```

### Verificare la Configurazione:

- Per controllare lo stato del **Servizio Ora di Windows** (su qualsiasi client), esegui da prompt dei comandi:

```
w32tm /query /status
```

![PDC status](/assets/2025-01-02/image05.png)
_Verifica dello stato su PDC_

![Client status](/assets/2025-01-02/image04.png) 
_Verifica dello stato su client_

- Per verificare i peer configurati:

```
w32tm /query /peers
```
![Applicazione filtro WMI](/assets/2025-01-02/image06.png)

## Gestione di Virtual Machines:

Se sei abituato a lavorare con **Virtual Machines** (VM) e hai configurato la GPO di questo articolo potresti riscontrare problemi di aggiormaneto se lasci che la macchina guest sincronizzi l'ora con l'host. Evita questo conflitto e disattiva la sincronizzazione dell'ora sull'hyper-visor.

![Applicazione filtro WMI](/assets/2025-01-02/image07.png)
_Sincronizzazione dell'ora in Hyper-V_ 

## Conclusioni

Grazie a questa configurazione garantirai che il PDC emulator sincronizzi la sua ora con con fonti esterne affidabili; tutti gli altri dispositivi nel dominio seguiranno la gerarchia interna per la sincronizzazione dell'ora. Controlla periodicamente che l'ora della tua network sia allineata con i provider NTP pubblici. 