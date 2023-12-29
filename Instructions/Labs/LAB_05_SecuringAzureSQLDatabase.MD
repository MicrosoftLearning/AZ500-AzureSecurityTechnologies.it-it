---
lab:
  title: 05 - Protezione delle database SQL di Azure
  module: Module 02 - Plan and implement security for Azure SQL Database and Azure SQL Managed Instance
---

# Lab 05: Protezione dei database SQL di Azure
# Manuale del lab per gli studenti

## Scenario laboratorio

È stato chiesto di esaminare le funzionalità di sicurezza per il database SQL di Azure. In particolare, si è interessati a:

- Protezione da attacchi come SQL injection e l'esfiltrazione di dati. 
- Possibilità di individuare e classificare le informazioni del database in categorie come riservate. 
- Possibilità di controllare le query del server di database e del database e gli eventi di log. 

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Implementare funzionalità di sicurezza per il database SQL

## Diagramma della protezione del database SQL di Azure

![image](https://user-images.githubusercontent.com/91347931/157533836-7250fa58-a109-4882-a55b-d3fa3baf34ab.png)

## Istruzioni

## File del lab:

- **\\Allfiles\\Labs\\11\\azuredeploy.json**

### Esercizio 1: Implementare funzionalità di sicurezza per il database SQL

### Tempo stimato: 30 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Distribuire un database SQL di Azure
- Attività 2: Configurare la protezione dei dati avanzata
- Attività 3: Configurare la classificazione dei dati
- Attività 4: Configurare il controllo

#### Attività 1: Distribuire un database SQL di Azure

In questa attività si userà un modello per distribuire l'infrastruttura del lab. 

1. Accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Distribuire un modello personalizzato** e premere **INVIO**.

3. Nel pannello **Distribuzione personalizzata** fare clic sull'opzione **Creare un modello personalizzato nell'editor**.

4. Nel pannello **Modifica modello** fare clic su **Carica file**, individuare il file **\\Allfiles\\Labs\\11\\azuredeploy.json** e fare clic su **Apri**.

    >**Nota**: esaminare il contenuto del modello e notare che distribuisce un database SQL di Azure.

5. Nel pannello **Modifica modello** fare clic su **Salva**.

6. Nel pannello **Distribuzione personalizzata** assicurarsi che siano configurate le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Subscription|Nome della sottoscrizione di Azure che verrà usata nel lab|
   |Gruppo di risorse|Fare clic su **Crea nuovo** e digitare il nome **AZ500LAB11**|
   |Ufficio|**(Stati Uniti) Stati Uniti orientali**|

7. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: attendere il completamento della distribuzione. 

#### Attività 2: Configurare la protezione dei dati avanzata

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Gruppi di risorse** e premere **INVIO**.

2. Nel pannello **Gruppi di risorse** fare clic sulla voce **AZ500LAB11** nell'elenco dei gruppi di risorse.

3. Nel pannello **AZ500LAB11** fare clic sulla voce che rappresenta la nuova istanza di SQL Server creata.

4. Nella sezione **Sicurezza** del pannello di SQL Server fare clic su **Microsoft Defender for Cloud** e selezionare **Abilitare Microsoft Defender for SQL**.

      >**Nota**: attendere fino a quando la notifica non indica che l'abilitazione di Azure Defender per SQL è avvenuta correttamente.

5. Nel pannello SQL Server, nella **sezione Sicurezza** della **pagina Microsoft Defender per il cloud**, nel **parametro Microsoft Defender for SQL: Abilitato a livello di sottoscrizione (Configura)** fare clic su **(configura)**.  

      >**Nota**: se l'opzione **(configura)** non è visualizzata, aggiornare il browser.
    
6. Nel pannello **Impostazioni server** esaminare le informazioni sui prezzi e il periodo di valutazione, **IMPOSTAZIONI DI VALUTAZIONE DELLA VULNERABILITÀ** e **IMPOSTAZIONI DI ADVANCED THREAT PROTECTION**.

7. Tornare al pannello **Microsoft Defender for Cloud** ed esaminare **Raccomandazioni** e **Avvisi sicurezza**.

      >**Nota**: per la visualizzazione delle raccomandazioni nel pannello **Microsoft Defender for Cloud** possono essere necessari 10-15 minuti. Anziché attendere, passare all'attività successiva, ma provare a tornare a questo pannello al termine di tutte le attività rimanenti.


#### Attività 3: Configurare la classificazione dei dati

In questa attività si individueranno e classificheranno le informazioni nel database SQL per la conformità al GDPR e alla protezione dei dati.

1. Nella sezione **Impostazioni** del pannello di SQL Server fare clic su **Database SQL**.

2. Nell'elenco dei database selezionare la voce **AZ500LabDb**. 

3. Nella sezione **Sicurezza** nel pannello del database SQL **AZ500LabDb** fare clic su **Individuazione dati e classificazione**.

4. Nel pannello **Individuazione dati e classificazione** fare clic sulla scheda **Classificazione**.

    >**Nota**: il motore di classificazione esegue l'analisi del database per identificare le colonne contenenti dati potenzialmente sensibili e fornisce un elenco delle classificazioni delle colonne consigliate.

5. Fare clic sul messaggio di testo **Sono state trovate 15 colonne con consigli per la classificazione** visualizzato sulla barra blu nella parte superiore del pannello.

6. Esaminare le colonne elencate e l'etichetta di riservatezza consigliata. 

7. Selezionare la casella di controllo **Seleziona tutto** e quindi fare clic su **Accetta i consigli selezionati**.

    >**Nota**: in alternativa, è possibile selezionare solo determinate colonne e ignorarne altre. 

    >**Nota**: è possibile modificare il tipo di informazioni e l'etichetta di riservatezza. 

8. Al termine, fare clic su **Salva**. 

    >**Nota**: la classificazione verrà completata e le colonne del database verranno etichettate in modo permanente con i nuovi metadati di classificazione. 

9. Tornare alla scheda **Panoramica** del pannello **Individuazione dati e classificazione** e osservare che i dati sono stati aggiornati in base alle informazioni di classificazione più recenti. 

#### Attività 4: Configurare il controllo 

In questa attività si configurerà prima il controllo a livello di server e quindi il controllo a livello di database. 

1. Nel portale di Azure tornare al pannello SQL Server.

2. Nella sezione **Sicurezza** nel pannello SQL Server fare clic su **Controllo**.

    >**Nota**: si tratta del controllo a livello di server. Le impostazioni di controllo predefinite includono tutte le query e le stored procedure eseguite sul database, nonché gli accessi riusciti e non riusciti.

3. Impostare l'opzione **Abilita Controllo di Azure SQL** su **SÌ** per abilitare il controllo. 

4. Selezionare la casella di controllo **Archiviazione** e verranno visualizzate le caselle per **Sottoscrizione** e **Account di archiviazione**.

5. Selezionare la propria sottoscrizione nell'elenco a discesa **Sottoscrizione**.

6. Fare clic su **Account di archiviazione** e scegliere **Crea nuovo**.

7. Nella casella **Nome** nel pannello **Crea account di archiviazione** digitare un nome univoco a livello globale costituito da 3-24 tra lettere minuscole e cifre e fare clic su **OK**, 

    >**Nota**: affinché l'account di archiviazione diventi disponibile, può essere necessario aggiornare il browser.

8. Tornare al pannello **Controllo** e in **Proprietà avanzate** impostare **Conservazione (giorni)** su **5**. 

9. Nel pannello Controllo fare clic su **Salva** per salvare le impostazioni di controllo. 

    >**Nota**: se viene visualizzato un messaggio di errore che indica che il percorso del contenitore di archiviazione non è valido, è possibile che non sia ancora stato effettuato il provisioning dell'account di archiviazione. Attendere alcuni minuti, fare clic su **Account di archiviazione**, nel pannello **Scegli account di archiviazione** selezionare il nuovo account di archiviazione creato e, tornando al pannello Controllo, fare clic su **Salva**.

10. Nella sezione **Impostazioni** nel pannello del server fare clic su **Database SQL**.

11. Nell'elenco dei database selezionare la voce **AZ500LabDb**. 

12. Nella sezione **Sicurezza** nel pannello del database SQL **AZ500LabDb** fare clic su **Controllo**. 

    >**Nota**: si tratta del controllo a livello di database. Il controllo a livello di server è già abilitato. 
  
    >**Nota**: i controlli possono essere scritti in un account di archiviazione di Azure, in un'area di lavoro Log Analytics o nell'hub eventi. È possibile configurare qualsiasi combinazione di queste opzioni.

    >**Nota**: se nel server è abilitato il controllo a livello di archiviazione, questo verrà sempre applicato al database, indipendentemente dalle impostazioni del database.
13. Nella pagina Panoramica del database SQL nella portale di Azure selezionare **Editor di query (anteprima)** dal menu a sinistra. Provare ad accedere, è possibile che si verifichi un errore nella password, nella regola del firewall per l'indirizzo IP, tutto viene controllato. Provare anche l'accesso riuscito, eseguire una query e si potrebbero trovare altri detial nei log di controllo

14. tornare a database, controllo e fare clic su **Visualizza log** di controllo.

15. Nel pannello **Record di controllo** è possibile passare tra il controllo del server e il controllo del database. 

> Risultati: sono stati creati un server e un database SQL e sono stati configurati la classificazione dei dati e il controllo. 


**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti. 

1. Nel portale di Azure aprire Cloud Shell facendo clic sulla prima icona nell'angolo in alto a destra. Se richiesto, fare clic su **PowerShell** e su **Crea risorsa di archiviazione**.

2. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

3. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il gruppo di risorse creato in questo lab:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB11" -Force -AsJob
    ```
4. Chiudere il riquadro **Cloud Shell**. 
