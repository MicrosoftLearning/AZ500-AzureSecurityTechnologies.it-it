---
lab:
  title: 12 - Microsoft Defender per il cloud
  module: Module 04 - Microsoft Defender for Cloud
---

# Lab 12: Microsoft Defender per il cloud
# Manuale del lab per gli studenti

## Scenario laboratorio

È stata ricevuta la richiesta di creare un modello di verifica di un ambiente basato su Microsoft Defender for Cloud. In particolare, sarà necessario:

- Configurare Microsoft Defender for Cloud per monitorare una macchina virtuale.
- Esaminare le raccomandazioni di Microsoft Defender for Cloud per la macchina virtuale.
- Implementare raccomandazioni per la configurazione guest e l'accesso JUST-in-time alle macchine virtuali. 
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
- Attività 3: Implementare la raccomandazione Microsoft Defender per il cloud per abilitare l'accesso JUST-in-time alle macchine virtuali

#### Attività 1: Configurare Microsoft Defender for Cloud

In questa attività si eseguiranno l'onboarding e la configurazione di Microsoft Defender for Cloud.

1. Accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Microsoft Defender for Cloud** e premere **INVIO**.

3. Nel pannello di spostamento sinistro fare clic su **Attività iniziali**. **Nel pannello Microsoft Defender per il cloud \| Attività iniziali** fare clic su **Aggiorna**.
     
4. **Nel pannello Microsoft Defender per il cloud \| Introduzione**, nella scheda Installa agenti scorrere verso il basso e fare clic su **Installa agenti**. 

5. **Nel pannello Attività iniziali** Microsoft Defender per il cloud\|, nella **scheda Aggiorna** >> scorrere verso il basso fino a quando la **sezione Seleziona aree di lavoro con funzionalità** di sicurezza avanzate è visibile >> attivare il **piano** di Microsoft Defender selezionando l'area di lavoro Log Analytics, quindi fare clic sul pulsante Grande Aggiornamento blu.  

    >**Nota**: esaminare tutte le funzionalità disponibili nell'ambito dei piani di Microsoft Defender. 

6. Passare a **Microsoft Defender per il cloud** e, nel pannello di spostamento sinistro nella sezione Gestione fare clic su **Ambiente Impostazioni**.

7. Nel pannello **impostazioni Microsoft Defender per il cloud \| Ambiente** scorrere verso il basso, espandere fino a visualizzare la sottoscrizione e fare clic sulla sottoscrizione pertinente. 

8. **Nel pannello Impostazioni \| Piani di Defender** selezionare **Abilita tutti i piani** e, se necessario, fare clic su **Salva**.

9. Tornare al pannello Impostazioni dell'ambiente **di Microsoft Defender per il cloud\|**, espandere fino a quando non viene visualizzata la sottoscrizione e fare clic sulla voce che rappresenta l'area di lavoro Log Analytics creata nel lab precedente.

10. Nel pannello **Impostazioni \| Piani** di Defender verificare che tutte le opzioni siano "Sì". Se necessario, fare clic su **Abilita tutti i piani** e quindi su **Salva**.

11. Selezionare **Raccolta dati** nel pannello **Impostazioni \| piani** di Defender. Fare clic su **Tutti gli eventi** e **salva**.

#### Attività 2: Esaminare la raccomandazione di Microsoft Defender for Cloud

In questa attività si esamineranno le raccomandazioni di Microsoft Defender for Cloud. 

1. Nel portale di Azure tornare al pannello **Microsoft Defender for Cloud \| Panoramica**. 

2. Nel pannello **Microsoft Defender for Cloud \| Panoramica** esaminare il riquadro **Punteggio di sicurezza**.

    >**Nota**: registrare il punteggio corrente, se disponibile.

3. Tornare al **pannello Panoramica** Microsoft Defender per il cloud\|, fare clic su **Risorse** valutate.

4. Nel pannello **Inventario** fare clic sulla **voce myVM** .

    >**Nota**: potrebbe essere necessario attendere alcuni minuti e aggiornare la pagina del browser per poter visualizzare la voce.
    
5. Nel pannello **Integrità delle risorse**, nella scheda **Raccomandazioni**, esaminare l'elenco delle raccomandazioni per **myVM**.

#### Attività 3: Implementare la raccomandazione Microsoft Defender per il cloud per abilitare l'accesso JUST-in-time alle macchine virtuali

In questa attività si implementerà la raccomandazione Microsoft Defender per il cloud per abilitare l'accesso JUST-in-time alle macchine virtuali nella macchina virtuale. 

1. Nel portale di Azure tornare al **pannello Panoramica** Microsoft Defender per il cloud \| e fare clic su **Protezione** del carico di lavoro in **Sicurezza** cloud nel pannello di spostamento a sinistra.

2. **Nel pannello Microsoft Defender per il cloud** Protezioni del carico di \| lavoro scorrere verso il basso fino alla **sezione Protezione** avanzata e fare clic sul **riquadro Accesso** JUST-in-time alle macchine virtuali.

3. Nel pannello **Accesso JUST-in-time alla macchina virtuale** , nella **sezione Macchine** virtuali selezionare **Non configurato** e quindi selezionare la casella di controllo per la **voce myVM** .

    >**Nota**: potrebbe essere necessario attendere alcuni minuti, aggiornare la pagina del browser e selezionare **di nuovo Non configurato** per visualizzare la voce.

4. Fare clic sull'opzione **Abilita JIT in 1 VM** all'estrema destra della sezione **Macchine virtuali**.

5. Nel pannello **Configurazione dell'accesso JIT alla VM**, all'estrema destra della riga che fa riferimento alla porta **22**, fare clic sul pulsante con i puntini di sospensione e quindi su **Elimina**.

6. Nel pannello **Configurazione dell'accesso JIT alla VM** fare clic su **Salva**.

    >**Nota**: monitorare lo stato di avanzamento della configurazione facendo clic sull'icona **Notifiche** sulla barra degli strumenti e visualizzando il pannello **Notifiche**. 

    >**Nota**: la propagazione nel punteggio di sicurezza delle raccomandazioni implementate in questo lab può richiedere del tempo. Controllare periodicamente il punteggio di sicurezza per verificare che sia stata presa in considerazione l'implementazione di queste funzionalità. 

> Risultati: è stato eseguito l'onboarding di Microsoft Defender for Cloud e sono state implementate le raccomandazioni per la macchina virtuale. 

    >**Note**: Do not remove the resources from this lab as they are needed for the Microsoft Sentinel lab.
