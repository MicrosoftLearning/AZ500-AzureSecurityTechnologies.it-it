---
lab:
  title: 09 - Microsoft Defender per il cloud
  module: Module 03 - Manage security posture by using Microsoft Defender for Cloud
---

# Lab 09: Microsoft Defender per il cloud
# Manuale del lab per gli studenti

## Scenario laboratorio

È stata ricevuta la richiesta di creare un modello di verifica di un ambiente basato su Microsoft Defender for Cloud. In particolare, sarà necessario:

- Configurare Microsoft Defender per il cloud funzionalità di sicurezza avanzate per i server per monitorare una macchina virtuale.
- Esaminare le raccomandazioni di Microsoft Defender for Cloud per la macchina virtuale.
- Implementare le raccomandazioni per la configurazione guest e l'accesso Just-In-Time alle macchine virtuali. 
- Capire come poter usare il punteggio di sicurezza per determinare lo stato di avanzamento del processo di creazione di un'infrastruttura più sicura.

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Implementare Microsoft Defender for Cloud

## Diagramma di Microsoft Defender for Cloud

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/c31055cc-de95-41f6-adef-f09d756a68eb)

## Istruzioni

### Esercizio 1: Implementare Microsoft Defender for Cloud

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Configurare Microsoft Defender for Cloud
- Attività 2: Esaminare le raccomandazioni di Microsoft Defender for Cloud
- Attività 3: Implementare la raccomandazione di Microsoft Defender per il cloud per abilitare l'accesso JIT (Just-In-Time) alla macchina virtuale

#### Attività 1: Configurare Microsoft Defender per il cloud funzionalità di sicurezza avanzata per i server

In questa attività si eseguirà l'onboarding e si configurerà Microsoft Defender per il cloud funzionalità di sicurezza avanzata per i server.

1. Avviare una sessione del browser e accedere alla  [sottoscrizione di Azure.](https://azure.microsoft.com/en-us/free/?azure-portal=true) in cui si dispone dell'accesso amministrativo.

2. Nella casella di testo Cerca risorse, servizi e documentazione nella parte superiore della pagina del portale di Azure digitare Microsoft Defender for Cloud e premere INVIO.

3. Nel pannello Microsoft Defender per il cloud, Gestione passare a Impostazioni ambiente. Espandere le cartelle delle impostazioni dell'ambiente fino a visualizzare la sezione sottoscrizione, quindi fare clic sulla sottoscrizione per visualizzare i dettagli.

4. Nel pannello Impostazioni, in Piani di Defender espandere Cloud Workload Protection (CWP).
  
5. Nell'elenco Piano di protezione del carico di lavoro cloud (CWP) selezionare Server. Sul lato destro della pagina modificare lo stato da Disattivato a Sì, quindi fare clic su Salva.
  
6. Per esaminare i dettagli di Microsoft Defender per server piano 2, selezionare Cambia piano >.

>**Nota**: l'abilitazione del piano Server CWP (Cloud Workload Protection) da Off a On abilita Microsoft Defender per server piano 2.

#### Attività 2: Esaminare la raccomandazione di Microsoft Defender for Cloud

In questa attività si esamineranno le raccomandazioni di Microsoft Defender for Cloud. 

1. Nel portale di Azure tornare al pannello **Microsoft Defender for Cloud \| Panoramica**. 

2. Nel pannello **Microsoft Defender for Cloud \| Panoramica** esaminare il riquadro **Punteggio di sicurezza**.

    >**Nota**: registrare il punteggio corrente, se disponibile.

3. Tornare al pannello **Microsoft Defender per il cloud \| Panoramica** e fare clic su **Risorse valutate**.

4. Nel pannello **Inventario** fare clic sulla voce **myVM**.

    >**Nota**: potrebbe essere necessario attendere alcuni minuti e aggiornare la pagina del browser per poter visualizzare la voce.
    
5. Nel pannello **Integrità delle risorse**, nella scheda **Raccomandazioni**, esaminare l'elenco delle raccomandazioni per **myVM**.

#### Attività 3: Implementare la raccomandazione di Microsoft Defender per il cloud per abilitare l'accesso JIT (Just-In-Time) alla macchina virtuale

In questa attività si implementerà la raccomandazione di Microsoft Defender per il cloud per abilitare l'accesso JIT (Just-In-Time) alla macchina virtuale. 

1. Nel portale di Azure tornare al pannello **Microsoft Defender per il cloud \| Panoramica** e fare clic su **Protezioni carico di lavoro** in **Sicurezza cloud** nel pannello di spostamento.

2. Nel pannello **Microsoft Defender per il cloud \| Protezioni carico di lavoro** scorrere verso il basso fino alla sezione **Protezione avanzata** e fare clic sul riquadro **Accesso Just-In-Time alla VM**.

3. Nel pannello **Accesso Just-In-Time alla VM**, nella sezione **Macchine virtuali** selezionare **Non configurato**, quindi selezionare la casella di controllo per la voce **myVM**.

    >**Nota**: Potrebbe essere necessario attendere alcuni minuti, aggiornare la pagina del browser e selezionare di nuovo **Non configurato** per poter visualizzare la voce.

4. Fare clic sull'opzione **Abilita JIT in 1 VM** all'estrema destra della sezione **Macchine virtuali**.

5. Nel pannello **Configurazione dell'accesso JIT alla VM**, all'estrema destra della riga che fa riferimento alla porta **22**, fare clic sul pulsante con i puntini di sospensione e quindi su **Elimina**.

6. Nel pannello **Configurazione dell'accesso JIT alla VM** fare clic su **Salva**.

    >**Nota**: monitorare lo stato di avanzamento della configurazione facendo clic sull'icona **Notifiche** sulla barra degli strumenti e visualizzando il pannello **Notifiche**. 

    >**Nota**: la propagazione nel punteggio di sicurezza delle raccomandazioni implementate in questo lab può richiedere del tempo. Controllare periodicamente il punteggio di sicurezza per verificare che sia stata presa in considerazione l'implementazione di queste funzionalità. 

> Risultati: è stato eseguito l'onboarding di Microsoft Defender for Cloud e sono state implementate le raccomandazioni per la macchina virtuale. 

    >**Note**: Do not remove the resources from this lab as they are needed for the Microsoft Sentinel lab.
