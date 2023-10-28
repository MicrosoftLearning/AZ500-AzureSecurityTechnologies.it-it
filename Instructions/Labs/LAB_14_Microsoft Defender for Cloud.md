---
lab:
  title: 14 - Microsoft Defender for Cloud
  module: Module 04 - Microsoft Defender for Cloud
---

# Lab 14: Microsoft Defender for Cloud
# Manuale del lab per studenti

## Scenario del lab

È stata ricevuta la richiesta di creare un modello di verifica di un ambiente basato su Microsoft Defender for Cloud. In particolare, sarà necessario:

- Configurare Microsoft Defender for Cloud per monitorare una macchina virtuale.
- Esaminare le raccomandazioni di Microsoft Defender for Cloud per la macchina virtuale.
- Implementare raccomandazioni per la configurazione guest e l'accesso alle macchine virtuali just-in-time. 
- Capire come poter usare il punteggio di sicurezza per determinare lo stato di avanzamento del processo di creazione di un'infrastruttura più sicura.

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Implementare Microsoft Defender for Cloud

## Diagramma di Microsoft Defender for Cloud

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/c31055cc-de95-41f6-adef-f09d756a68eb)

## Istruzioni

### Esercizio 1: Implementare Microsoft Defender for Cloud

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Configurare Microsoft Defender for Cloud
- Attività 2: Esaminare le raccomandazioni di Microsoft Defender for Cloud
- Attività 3: Implementare la raccomandazione Microsoft Defender per cloud per abilitare l'accesso alle macchine virtuali just-in-time

#### Attività 1: Configurare Microsoft Defender for Cloud

In questa attività si eseguiranno l'onboarding e la configurazione di Microsoft Defender for Cloud.

1. Accedere al portale di Azure **`https://portal.azure.com/`** .

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Microsoft Defender for Cloud** e premere **INVIO**.

3. Nel pannello di spostamento a sinistra fare clic su **Introduzione**. Nel pannello **Microsoft Defender per l'introduzione al cloud \|** fare clic su **Aggiorna**.
     
4. Nel pannello **Microsoft Defender per l'introduzione al cloud\|**, nella scheda Installa agenti scorrere verso il basso e fare clic su **Installa agenti**. 

5. Nel pannello **Microsoft Defender per l'introduzione al cloud\|**, nella scheda **Aggiorna** >> scorrere verso il basso fino a quando la sezione **Seleziona aree di lavoro con funzionalità di sicurezza avanzate** è visibile >> attivare il **piano di Microsoft Defender** selezionando l'area di lavoro Log Analytics, quindi fare clic sul pulsante Grande aggiornamento blu.  

    >**Nota**: esaminare tutte le funzionalità disponibili nell'ambito dei piani di Microsoft Defender. 

6. Passare a **Microsoft Defender per Cloud** e, nel pannello di spostamento a sinistra nella sezione Gestione fare clic su **Impostazioni ambiente**.

7. Nel pannello **Microsoft Defender per le impostazioni dell'ambiente cloud \|** scorrere verso il basso, espandere fino a quando non viene visualizzata la sottoscrizione e fare clic sulla sottoscrizione pertinente. 

8. Nel pannello **Impostazioni \| piani Defender** selezionare **Abilita tutti i piani** e, se necessario, fare clic su **Salva**.

9. Tornare al pannello **Microsoft Defender per le impostazioni dell'ambiente cloud\|**, espandere finché non viene visualizzata la sottoscrizione e fare clic sulla voce che rappresenta l'area di lavoro Log Analytics creata nel lab precedente.

10. Nel pannello **Impostazioni \| piani Defender** verificare che tutte le opzioni siano "Attiva". Se necessario, fare clic su **Abilita tutti i piani** e quindi fare clic su **Salva**.

11. Selezionare **Raccolta dati** nel pannello **Impostazioni \| piani di Defender** . Fare clic su **Tutti gli eventi** e **salvare**.

#### Attività 2: Esaminare la raccomandazione di Microsoft Defender for Cloud

In questa attività si esamineranno le raccomandazioni di Microsoft Defender for Cloud. 

1. Nel portale di Azure tornare al pannello **Microsoft Defender for Cloud \| Panoramica**. 

2. Nel pannello **Microsoft Defender for Cloud \| Panoramica** esaminare il riquadro **Punteggio di sicurezza**.

    >**Nota**: registrare il punteggio corrente, se disponibile.

3. Tornare al **pannello Microsoft Defender per Panoramica cloud\|**, fare clic su **Risorse valutate**.

4. Nel **pannello Inventario** fare clic sulla voce **myVM** .

    >**Nota**: potrebbe essere necessario attendere alcuni minuti e aggiornare la pagina del browser per poter visualizzare la voce.
    
5. Nel pannello **Integrità delle risorse**, nella scheda **Raccomandazioni**, esaminare l'elenco delle raccomandazioni per **myVM**.

#### Attività 3: Implementare la raccomandazione Microsoft Defender per cloud per abilitare l'accesso alle macchine virtuali just-in-time

In questa attività si implementerà la raccomandazione Microsoft Defender for Cloud per abilitare l'accesso alle macchine virtuali just-in-time nella macchina virtuale. 

1. Nella portale di Azure tornare al pannello **Microsoft Defender per Panoramica cloud \|** e fare clic su **Protezione dei carichi di lavoro** **nel** pannello di spostamento a sinistra.

2. Nel pannello **Microsoft Defender per le protezioni del carico di lavoro cloud \|** scorrere verso il basso fino alla sezione **Protezione avanzata** e fare clic sul riquadro **Di accesso alle macchine virtuali just-in-time**.

3. Nel pannello **Accesso alle macchine virtuali just-in-time** selezionare **Non configurato** e quindi selezionare la casella di controllo per la voce **myVM**.****

    >**Nota**: potrebbe essere necessario attendere alcuni minuti, aggiornare la pagina del browser e selezionare **Non configurato** di nuovo per visualizzare la voce.

4. Fare clic sull'opzione **Abilita JIT in 1 VM** all'estrema destra della sezione **Macchine virtuali**.

5. Nel pannello **Configurazione dell'accesso JIT alla VM**, all'estrema destra della riga che fa riferimento alla porta **22**, fare clic sul pulsante con i puntini di sospensione e quindi su **Elimina**.

6. Nel pannello **Configurazione dell'accesso JIT alla VM** fare clic su **Salva**.

    >**Nota**: monitorare lo stato di avanzamento della configurazione facendo clic sull'icona **Notifiche** sulla barra degli strumenti e visualizzando il pannello **Notifiche**. 

    >**Nota**: la propagazione nel punteggio di sicurezza delle raccomandazioni implementate in questo lab può richiedere del tempo. Controllare periodicamente il punteggio di sicurezza per verificare che sia stata presa in considerazione l'implementazione di queste funzionalità. 

> Risultati: è stato eseguito l'onboarding di Microsoft Defender for Cloud e sono state implementate le raccomandazioni per la macchina virtuale. 

    >**Note**: Do not remove the resources from this lab as they are needed for the Microsoft Sentinel lab.
