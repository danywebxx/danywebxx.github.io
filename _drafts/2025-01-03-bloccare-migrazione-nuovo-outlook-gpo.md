---
title: Bloccare la migrazione al nuovo Outlook tramite GPO - modo facile
date: 2025-01-03
categories: [blogging, tutorial]
tags: [microsoft, outlook]
description:
image:
     path: 
---

Se sei un amministratore IT e vuoi impedire la migrazione al nuovo Outlook nella tua organizzazione, puoi utilizzare una Group Policy (GPO) per distribuire una chiave di registro che blocca la migrazione. L'utilizzo della "vecchia" versione di Outlook garantisce la compatibilità con le configurazioni aziendali esistenti.

Ecco una guida passo-passo per configurare questa GPO:


## Passaggi per configurare la GPO

1. **Creazione della GPO**
   - Accedi al Domain Controller.
   - Apri l’“Editor Gestione Criteri di Gruppo” (Group Policy Management Console).
   - Crea una nuova GPO (ad esempio, "Blocca Migrazione Nuovo Outlook") o modifica una esistente applicata agli utenti o computer interessati.

2. **Configurazione delle impostazioni del Registro di Sistema**
   - All'interno dell'Editor delle GPO, vai su:
     ```
     Configurazione Utente o Configurazione Computer
     > Preferenze
     > Impostazioni di Windows
     > Registro di Sistema
     ```
   - Fai clic con il tasto destro su “Registro di Sistema” e seleziona “Nuovo > Elemento Registro”.
   
3. **Inserimento della chiave di registro**
   - Compila i campi come segue:
     - **Azione**: Aggiorna
     - **Hive**: HKEY_CURRENT_USER
     - **Percorso chiave**: `Software\Policies\Microsoft\Office\16.0\Outlook\Options\WXP`  
     - **Nome valore**: `NewOutlookOptOut`
     - **Tipo valore**: REG_DWORD
     - **Dati valore**: `1`

4. **Collegamento della GPO**
   - Torna alla console principale di Gestione Criteri di Gruppo.
   - Collega la GPO alla OU (Organizational Unit) contenente gli utenti o i computer a cui desideri applicare il blocco.

5. **Verifica dell’applicazione della GPO**
   - Assicurati che la GPO venga applicata eseguendo il comando:
     ```
     gpupdate /force
     ```
   - Puoi anche utilizzare lo strumento “Resultant Set of Policy” (RSoP) o il comando `gpresult` per confermare che le impostazioni siano attive.

#### Test della configurazione
Prima di distribuire la GPO in produzione, testala su un numero limitato di utenti o computer. Verifica che la chiave di registro sia presente e che il nuovo Outlook non sia accessibile.

#### Conclusioni
Impedire la migrazione al nuovo Outlook tramite GPO è un metodo efficace per mantenere un controllo centralizzato sulle modifiche dell’applicazione. Questo approccio garantisce che gli utenti possano continuare a utilizzare l’Outlook esistente senza interruzioni.

## Bibliografia
1. **Accesso a un Domain Controller**: Devi avere i permessi per creare e gestire GPO.
2. **Conoscenza dell’Editor di Gestione Criteri di Gruppo**: Assicurati di sapere come navigare nell’Editor GPO.
3. **Documentazione ufficiale di Microsoft**: Puoi trovare ulteriori dettagli su [questa pagina](https://learn.microsoft.com/en-us/microsoft-365-apps/outlook/get-started/control-install?WT.mc_id=M365-MVP-5005337#opt-out-of-new-outlook-migration).


