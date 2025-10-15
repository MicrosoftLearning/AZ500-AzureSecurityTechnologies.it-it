---
lab:
  title: 09 - Configurazione di Microsoft Defender per il cloud funzionalità di sicurezza avanzata per i server
  module: Module 03 - Configure and manage threat protection by using Microsoft Defender for Cloud
---

# Lab 09: Configurazione di Microsoft Defender per il cloud funzionalità di sicurezza avanzata per i server

# Manuale del lab per gli studenti

## Scenario laboratorio

In qualità di tecnico della sicurezza di Azure per una società globale di e-commerce, si è responsabili della protezione dell'infrastruttura cloud aziendale. L'organizzazione si basa principalmente su macchine virtuali (VM) di Azure e server locali per eseguire applicazioni critiche, gestire i dati dei clienti ed elaborare le transazioni. Il Chief Information Security Officer (CISO) ha identificato la necessità di misure di sicurezza avanzate per proteggere queste risorse da minacce informatiche, vulnerabilità e configurazioni errate. È stato richiesto di abilitare Microsoft Defender per server in Microsoft Defender per il cloud per fornire la protezione avanzata dalle minacce e il monitoraggio della sicurezza sia per le macchine virtuali di Azure che per i server ibridi.

## Obiettivi del lab

- Configurare Microsoft Defender per il cloud funzionalità di sicurezza avanzata per i server
  
- Esaminare le funzionalità di sicurezza di ehanced per Microsoft Defender for Servers Piano 2

## Istruzioni per l'esercizio

### Configurare Microsoft Defender per il cloud funzionalità di sicurezza avanzata per i server

1. portale di Azure Nella casella di testo Cerca risorse, servizi e documenti nella parte superiore della pagina **portale di Azure digitare Microsoft Defender per il cloud** e premere **INVIO**.

2. **Nel pannello Microsoft Defender per il cloud**, **Gestione** passare a **Impostazioni** ambiente. Espandere le cartelle delle impostazioni dell'ambiente fino a visualizzare la **sezione sottoscrizione** , quindi fare clic sulla **sottoscrizione** per visualizzare i dettagli.

   ![Acquisizione dello schermo delle impostazioni dell'ambiente di Microsoft Defender per il cloud](../media/defender-for-cloud-environment-settings.png)
   
3. Nel pannello **Impostazioni, in **Piani** di Defender espandere **Cloud Workload Protection (CWP)****.

4. Nell'elenco **Piano** di protezione del carico di lavoro cloud (CWP) selezionare **Server**. Sul lato destro della pagina modificare lo stato da Disattivato** a **Sì**, quindi fare clic su **Salva**.**** **

5. Per esaminare i dettagli di **Microsoft Defender per server piano 2**, selezionare **Cambia piano >**.

   Nota: l'abilitazione del piano Server CWP (Cloud Workload Protection) da Off a On abilita Microsoft Defender per server piano 2.
 
   ![Acquisizione dello schermo della pagina di selezione del piano di Microsoft Defender per il cloud.](../media/defender-for-cloud-plan-selection.png)
   
> **Risultati**: è stato abilitato Microsoft Defender per server piano 2 nella sottoscrizione.
