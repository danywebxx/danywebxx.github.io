---
title: M365 - Abilitare i firewall reports
date: 2024-02-25
categories: [blogging, security]
tags: [cloud, microsoft, m365, audit]
description: Nel portale security di M365 è presente il report firewall pche mostra mostra le connessioni in entrata, in uscita e alle app bloccate.
---
Investigando sul portale `M365 Security` per una problematica di un collega mi sono imbattutto nei `Firewall Reports` che non avevo mai notato prima. I report in questione permettono di visualizzare le connessioni in entrata, in uscita e alle app, bloccate dal firewall avanzato in MS Windows con Defender attivo.

Peccato che nel mio caso il report fosse totalmente vuoto e non visualizzasse dati interessanti!

![Firewall reports](/assets/2024-02-25/mdb-firewall-report.png)
_Firewall reports_

## Premessa
Per poter abilitare i firewall reports nella sezione Security di M365 è necessario avere attivo uno dei tre piani di licenza Microsoft Defender:

- Microsoft Defender for Endpoint Plan 1
- Microsoft Defender for Endpoint Plan 2
- Microsoft Defender XDR

Fondamentale aver effettuato l'onboard dei dispostivi sul portale ed abilitato la raccolta dei log necessari.

## Raccolta dei log
Devo ammettere che è stato più difficile capire le guide Microsoft che non configurare il tutto. 

Per raccogliere i log necessari sarà sufficiente abilitare alcuni eventi eventi necessari nell'event viewer di Windows appartenenti a due categorie specifiche normalmente non abilitate:

- Audit Filtering Platform Packet Drop
- Audit Filtering Platform Connection

Queste categorie possono essere abilitate seguendo tre strade:

- Group Policy Object
- Local Security Policy 
- Powershell

Microsoft Defender penserà poi ad inviare i dati al portale di security.
### Abilitare audit tramite GPO
LA strada più semplice è creare una nuova group policy per questo scopo. Nella sezione Computer andranno settati i due oggetti che riguardano gli Audit filtering elencati sopra. Consigliato abilitare solo gli eventi in "failure" per evitare di avere troppi eventi nel registro; ogni connessione da e verso il computer potrebbe dare seguito ad un evento se vengono registrati anche quelle "success".

![Group Policy](/assets/2024-02-25/firewall-gpo.png)
_Group Policy Computer_

### Powerhsell
I comandi da eseguire sui dispositivi sono i seguenti:

```powershell
auditpol /set /subcategory:"Filtering Platform Packet Drop" /failure:enable
auditpol /set /subcategory:"Filtering Platform Connection" /failure:enable
```
### Event viewer
Una volta abilitati gli audit, nell'event viewer sarà possibile visualizzare dati dettagliati come l'ID 5152 - connessione bloccata - che riporto nell'immagine.

![Evento nell'event viewer di Windows](/assets/2024-02-25/firewall-registro.png)
_ID 5152 nell'event viewer di Windows_

Saranno questi eventi che renderanno possibile la creazione dei firewall reports nel portale Security di M365. Il nostro compito sarà quello di effettuare controlli approfonditi sul traffico presente nella nostra infrastruttura e riconoscere attività sospette quali port-scan e altre attività di ricognozione.

## Bibliografia
- [Microsoft - Host firewall reporting](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/host-firewall-reporting?view=o365-worldwide)
- [Audit Filtering Platform Packet Drop](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/audit-filtering-platform-packet-drop)
- [Audit Filtering Platform Connection](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/audit-filtering-platform-connection)

