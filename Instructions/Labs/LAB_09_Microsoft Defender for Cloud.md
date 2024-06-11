---
lab:
  title: 09 - Microsoft Defender per il cloud
  module: Module 03 - Manage security posture by using Microsoft Defender for Cloud
---

# Lab 09: Microsoft Defender per il cloud
# Manuale del lab per gli studenti

## Scenario laboratorio

È stata ricevuta la richiesta di creare un modello di verifica di un ambiente basato su Microsoft Defender for Cloud. In particolare, sarà necessario:

- Configurare Microsoft Defender for Cloud per monitorare una macchina virtuale.
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

#### Attività 1: Configurare Microsoft Defender for Cloud

In questa attività si eseguiranno l'onboarding e la configurazione di Microsoft Defender for Cloud.

1. Accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Microsoft Defender for Cloud** e premere **INVIO**.

3. Nel pannello di spostamento a sinistra fare clic su **Introduzione**. Nel pannello **Microsoft Defender per il cloud \| Introduzione** fare clic su **Aggiorna**.
     
4. Nel pannello **Microsoft Defender per il cloud \| Introduzione** scorrere verso il basso nella scheda Installa agenti e fare clic su **Installa agenti**. 

5. Nel pannello **Microsoft Defender per il cloud \| Introduzione**, nella scheda **Aggiorna** >> scorrere verso il basso fino a quando non è visibile la sezione **Seleziona aree di lavoro con funzionalità di sicurezza avanzate** >> attivare il **piano di Microsoft Defender** selezionando l'area di lavoro Log Analytics, quindi fare clic sul pulsante blu grande Aggiorna.  

    >**Nota**: esaminare tutte le funzionalità disponibili nell'ambito dei piani di Microsoft Defender. 

6. Passare a **Microsoft Defender per il cloud ** e fare clic su **Impostazioni ambiente** nel pannello di spostamento a sinistra nella sezione Gestione.

7. Nel pannello **Microsoft Defender per il cloud \| Impostazioni ambiente** scorrere verso il basso, espandere fino a visualizzare la sottoscrizione e fare clic sulla sottoscrizione pertinente. 

8. Nel pannello **Impostazioni \| Piani di Defender** selezionare **Abilita tutti i piani** e, se necessario, fare clic su **Salva**.

9. Tornare al pannello **Microsoft Defender per il cloud \| Impostazioni ambiente**, espandere fino a visualizzare la sottoscrizione e fare clic sulla voce che rappresenta l'area di lavoro Log Analytics creata nel lab precedente.

10. Nel pannello **Impostazioni \| Piani di Defender** assicurarsi che tutte le opzioni siano abilitate. Se necessario, fare clic su **Abilita tutti i piani** e quindi su **Salva**.

11. Selezionare **Raccolta dati** nel pannello **Impostazioni \| Piani di Defender**. Fare clic su **Tutti gli eventi** e **Salva**.

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
