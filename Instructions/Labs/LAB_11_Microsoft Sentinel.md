---
lab:
  title: 11 - Microsoft Sentinel
  module: Module 04 - Configure and manage security monitoring and automation solutions
---

# Lab 11: Microsoft Sentinel
# Manuale del lab per gli studenti

## Scenario laboratorio

**Nota:** **Azure Sentinel** è stato rinominato in **Microsoft Sentinel** 

È stata ricevuta la richiesta di creare un modello di verifica delle funzionalità di rilevamento e risposta alle minacce basate su Microsoft Sentinel. In particolare, sarà necessario:

- Iniziare raccogliendo i dati da Attività di Azure e Microsoft Defender for Cloud.
- Aggiungere avvisi predefiniti e personalizzati. 
- Capire come poter usare i playbook per automatizzare una risposta a un evento imprevisto.

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Implementare Microsoft Sentinel

## Diagramma di Microsoft Sentinel

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/509aa70d-de11-4470-a289-877fbfecbc00)

## Istruzioni

## File del lab:

- **\\Allfiles\\Labs\\15\\changeincidentseverity.json**

### Esercizio 1: Implementare Microsoft Sentinel

### Tempo stimato: 30 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Eseguire l'onboarding di Microsoft Sentinel
- Attività 2: Connettere Attività di Azure a Sentinel
- Attività 3: Creare una regola che usa il connettore dati Attività di Azure 
- Attività 4: Creare un playbook
- Attività 5: Creare un avviso personalizzato e configurare il playbook come risposta automatica
- Attività 6: Richiamare un evento imprevisto ed esaminare le azioni associate

#### Attività 1: Eseguire l'onboarding di Microsoft Sentinel

In questa attività si eseguirà l'onboarding di Microsoft Sentinel e si connetterà l'area di lavoro Log Analytics. 

1. Accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure, digitare **Microsoft Sentinel** e premere **INVIO**.

    >**Nota**: se questo è il primo tentativo di attivare Microsoft Sentinel nel dashboard di Azure, seguire questa procedura: Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure, digitare **Microsoft Sentinel** e premere **INVIO**. Selezionare **Microsoft Sentinel** dalla visualizzazione **Servizi**.
  
3. Nel pannello **Microsoft Sentinel** fare clic su **+ Crea**.

4. Nel pannello **Add Microsoft Sentinel to a workspace** (Aggiungi Microsoft Sentinel a un'area di lavoro) selezionare l'area di lavoro Log Analytics creata nel lab su Monitoraggio di Azure e fare clic su **Aggiungi**.

    >**Nota**: Microsoft Sentinel ha requisiti molto specifici per le aree di lavoro. Non è ad esempio possibile usare le aree di lavoro create da Microsoft Defender for Cloud. Per altre informazioni, vedere [Avvio rapido: Microsoft Sentinel a bordo](https://docs.microsoft.com/en-us/microsoft/sentinel/quickstart-onboard)
    
#### Attività 2: Configurare Microsoft Sentinel per usare il connettore dati Attività di Azure. 

In questa attività si configurerà Sentinel in modo da usare il connettore dati Attività di Azure.  

1. Nel pannello **Microsoft Sentinel \| Panoramica** del portale di Azure fare clic su **Hub dei contenuti** nella sezione **Gestione dei contenuti**.

2. Nel pannello **Microsoft Sentinel \| Hub dei contenuti** esaminare l'elenco dei contenuti disponibili.

3. Digitare **Azure** nella barra di ricerca e selezionare la voce che rappresenta l'**attività di Azure**. Esaminare la descrizione nella parte all'estrema destra, quindi fare clic su **Installa**.

4. Attendere la notifica di **Installazione riuscita**. Nel pannello di spostamento a sinistra, nella sezione **Configurazione**, fare clic su **Connettori dati**.

5. Nel pannello **Microsoft Sentinel \| Connettori dati** fare clic su **Aggiorna** ed esaminare l'elenco dei connettori disponibili. Selezionare la voce che rappresenta il connettore **Attività di Azure** (nascondere la barra dei menu a sinistra usando \<<, se necessario), esaminare la descrizione e lo stato nella parte all'estrema destra, quindi fare clic sulla **pagina Apri connettore**.

6. Nel pannello **Attività di Azure** deve essere selezionata la scheda **Istruzioni**, consultare i **Prerequisiti** e scorrere verso il basso fino a **Configurazione**. Prendere nota delle informazioni che descrivono l'aggiornamento del connettore. La sottoscrizione non ha mai usato il metodo di connessione legacy per ignorare il passaggio 1 (il pulsante Disconnetti tutto** verrà disattivato) e procedere con il **passaggio 2.

7. Nel passaggio 2 **Connettere gli abbonamenti tramite la nuova pipeline delle impostazioni di diagnostica** leggere le istruzioni "Avviare l'assegnazione guidata di criteri di Azure e seguire la procedura" e quindi fare clic su **Avvia l'assegnazione guidata Criteri di Azure\>**.

8. Nella scheda **Informazioni di base** della finestra **Configura i log attività di Azure per il flusso nell'area di lavoro Log Analytics specificata** (pagina Assegna criteri) fare clic sul pulsante **puntini di sospensione (...) di Ambito**. **Nella pagina Ambito** scegliere la sottoscrizione dall'elenco a discesa sottoscrizione e fare clic sul **pulsante Seleziona** nella parte inferiore della pagina.

    >**Nota**: *non* scegliere un gruppo di risorse

9. Fare clic due volte sul pulsante **Avanti** nella parte inferiore della scheda **Basics** per passare alla scheda **Parametri**. Nella scheda **Parametri** fare clic sul pulsante **Area di lavoro Log Analytics primaria ellissi (...)**. **Nella pagina Area di lavoro** Log Analytics primaria verificare che la sottoscrizione sia selezionata e usare l'elenco **a discesa aree di lavoro** per selezionare l'area di lavoro Log Analytics in uso per Sentinel. Al termine, fare clic sul pulsante **Seleziona** nella parte inferiore della pagina.

10. Fare clic sul pulsante **Avanti** nella parte inferiore della scheda **Parametri** per passare alla scheda **Correzione**. Nella scheda **Correzione** selezionare la casella di controllo **Crea un'attività di correzione**. In questo modo verrà abilitata l'opzione "Configura i log attività di Azure per il flusso nell'area di lavoro Log Analytics specificata" nell'elenco a discesa **Criteri da correggere**. Nell'elenco a discesa **Posizione identità assegnata dal sistema** selezionare l'area geografica (ad esempio, Stati Uniti orientali) selezionata in precedenza per l'area di lavoro Log Analytics.

11. Fare clic sul pulsante **Avanti** nella parte inferiore della scheda **Correzione** per passare alla scheda **Messaggio di non conformità**. Immettere un messaggio di non conformità (facoltativo) e fare clic sul pulsante **Rivedi e crea** nella parte inferiore della scheda **Messaggio di non conformità**.

12. Fare clic sul pulsante **Crea**. Vengono visualizzati tre messaggi di stato con esito positivo: **La creazione dell'assegnazione dei criteri è riuscita, La creazione delle assegnazioni di ruolo è riuscita e La creazione dell'attività di correzione è riuscita**.

    >**Nota**: è possibile selezionare l'icona a forma di campanello delle Notifiche per verificare le tre attività con esito positivo.

13. Verificare che nel pannello **Attività di Azure** venga visualizzato il grafico **Dati ricevuti** (potrebbe essere necessario aggiornare la pagina del browser).  

    >**Nota**: potrebbero essere necessari più di 15 minuti prima che lo Stato risulti impostato su "Connesso" e venga visualizzato il grafico Dati ricevuti.

#### Attività 3: Creare una regola che usa il connettore dati Attività di Azure 

In questa attività si esaminerà e creerà una regola che usa il connettore dati Attività di Azure. 

1. Nel pannello **Microsoft Sentinel \| Configurazione** fare clic su**Analisi**. 

2. Nel pannello **Microsoft Sentinel \| Analisi** fare clic sulla scheda **Modelli di regola**. 

    >**Nota**: verificare i tipi di regola che è possibile creare. Ogni regola è associata a un'origine dati specifica.

3. Nell'elenco dei modelli di regola digitare **Sospetto** nella barra di ricerca e fare clic sulla voce **Suspicious number of resource creation or deployment** (Numero sospetto di attività di creazione o distribuzione di risorse) associata all'origine dati **Attività di Azure**. Nel pannello con le proprietà del modello di regola fare quindi clic su **Crea regola** (se necessario, scorrere nella parte destra della pagina).

    >**Nota**: questa regola ha un livello di gravità medio. 

4. Nella scheda **Generale** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata**, accettare le impostazioni predefinite e fare clic su **Avanti: Imposta la logica della regola>**.

5. Nella scheda **Imposta la logica della regola** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata**, accettare le impostazioni predefinite e fare clic su **Avanti: Impostazioni eventi imprevisti (Anteprima) > **.

6. Nella scheda **Impostazioni eventi imprevisti** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata**, accettare le impostazioni predefinite e fare clic su **Avanti: Risposta automatica>**. 

    >**Nota**: è possibile a questo punto aggiungere a una regola un playbook, implementato come app per la logica, per automatizzare la correzione di un problema.

7. Nella scheda **Risposta automatica** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata**, accettare le impostazioni predefinite e fare clic su **Avanti: Rivedi e crea > **. 

8. Nella scheda **Rivedi e crea** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata** fare clic su **Salva**.

    >**Nota**: è ora disponibile una regola attiva.

#### Attività 4: Creare un playbook

In questa attività verrà creato un playbook. Un playbook della sicurezza è una raccolta di attività che è possibile richiamare da Azure Sentinel in risposta a un avviso. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Distribuire un modello personalizzato** e premere **INVIO**.

2. Nel pannello **Distribuzione personalizzata** fare clic sull'opzione **Creare un modello personalizzato nell'editor**.

3. Nel pannello **Modifica modello** fare clic su **Carica file**, individuare il file **\\Allfiles\\Labs\\15\\changeincidentseverity.json** e fare clic su **Apri**.

    >**Nota**: è possibile trovare playbook di esempio all'indirizzo [https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks).

4. Nel pannello **Modifica modello** fare clic su **Salva**.

5. Nel pannello **Distribuzione personalizzata** assicurarsi che siano configurate le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |---|---|
    |Subscription|Nome della sottoscrizione di Azure usata in questo lab|
    |Gruppo di risorse|**AZ500LAB131415**|
    |Ufficio|**(Stati Uniti) Stati Uniti orientali**|
    |Nome del playbook|**Change-Incident-Severity**|
    |Nome utente|Indirizzo di posta elettronica|

6. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: attendere il completamento della distribuzione.

7. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Gruppi di risorse** e premere **INVIO**.

8. Nel pannello **Gruppi di risorse** fare clic sulla voce **AZ500LAB131415** nell'elenco dei gruppi di risorse.

9. Nel pannello del gruppo di risorse **AZ500LAB131415**, nell'elenco di risorse selezionare la voce che rappresenta l'app per la logica **Change-Incident-Severity** appena creata.

10. Nel pannello **Change-Incident-Severity** fare clic su **Modifica**.

    >**Nota**: Nel pannello **Progettazione app per la logica** per ognuna delle quattro connessioni viene visualizzato un avviso. con cui si indica che ciascuna di esse deve essere controllata e configurata.

11. Nel pannello **Progettazione app per la logica** fare clic sul primo passaggio **s**.

12. Fare clic su **Aggiungi nuovo**, verificare che la voce nell'elenco a discesa **Tenant** contenga il nome del tenant Azure AD e fare clic su **Accedi**.

13. Quando richiesto, accedere con l'account utente con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

14. Fare clic sul secondo passaggio **s**  e, nell'elenco di s, selezionare la seconda voce, che rappresenta l'oggetto creato nel passaggio precedente.

15. Ripetere il passaggio precedente per i due passaggi **s** rimanenti.

    >**Nota**: verificare che non sia stato visualizzato alcun avviso in nessuno dei passaggi.

16. Nel pannello **Progettazione app per la logica** fare clic su **Salva** per salvare le modifiche.

#### Attività 5: Creare un avviso personalizzato e configurare un playbook come risposta automatica

1. Nel portale di Azure tornare al pannello **Microsoft Sentinel \| Panoramica**.

2. Nel pannello **Microsoft Sentinel \| Panoramica** fare clic su **Analisi** nella sezione **Configurazione**.

3. Nel pannello **Microsoft Sentinel \| Analisi** fare clic su **+ Crea** e nel menu a discesa selezionare **Regola di query pianificata**. 

4. Nella scheda **Generale** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |---|---|
    |Nome|**Playbook Demo**|
    |Tattiche|**Accesso iniziale**|

5. Fare clic su **Avanti: Imposta la logica della regola >**.

6. Nella scheda **Imposta la logica della regola** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata**, nella casella di testo **Query delle regole**, copiare la query delle regole nella casella di testo. 

    ```
    AzureActivity
     | where ResourceProviderValue =~ "Microsoft.Security" 
     | where OperationNameValue =~ "Microsoft.Security/locations/jitNetworkAccessPolicies/delete" 
    ```

    >**Nota**: Questa regola identifica la rimozione dei criteri di accesso just-in-time alle macchine virtuali.

    >**Nota** se viene visualizzato un errore di analisi, è possibile che Intellisense abbia aggiunto valori alla query. Assicurarsi che la query corrisponda; in caso contrario, incollare la query nel Blocco note e quindi dal Blocco note alla query delle regole. 


7. Nella scheda **Imposta la logica della regola** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata** della sezione **Pianificazione query** impostare **Esegui la query ogni** su **5 minuti**.

8. Nella scheda **Imposta la logica della regola ** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata** accettare i valori predefiniti delle impostazioni rimanenti e fare clic su **Avanti: Impostazioni degli eventi imprevisti > **.

9. Nella scheda **Impostazioni eventi imprevisti** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata**, accettare le impostazioni predefinite e fare clic su **Avanti: Risposta automatica>**. 

10. Nella scheda **Risposta automatica** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata**, da **Regole di automazione** fare clic su **+ Aggiungi nuova**.

11. Nella finestra **Crea nuova regola di automazione** immettere **Esegui playbook sulla gravità della modifica** per il **Nome della regola di automazione**. Nel campo **Trigger** fare clic sul menu a discesa e selezionare **Quando viene creato l'avviso**.

12. Nella finestra **Crea nuova regola di automazione**, in **Azioni**, leggere la nota e quindi fare clic su **Gestisci autorizzazioni playbook**. Nella finestra **Gestisci autorizzazioni** selezionare la casella di controllo accanto al **gruppo di risorse creato in precedenza AZ500LAB1314151** e quindi fare clic su **Applica**.

13.  Nella finestra **Crea nuova regola di automazione**, in **Azioni**, fare clic sul secondo menu a discesa e selezionare l'app per la logica **Change-Incident-Severity**. Dalla finestra **Crea nuova regola di automazione**, fare clic su **Applica**.

14. Nella scheda **Risposta automatica** del pannello **Procedura guidata per la regola di analisi - Crea una nuova regola pianificata**, fare clic su **Avanti: Rivedi e crea > ** e quindi su **Salva**

    >**Nota**: è ora disponibile una nuova regola attiva denominata **Playbook Demo**. Se si verifica un evento identificato dalla logica della regola, verrà generato un avviso di gravità media, da cui avrà origine un corrispondente evento imprevisto.

#### Attività 6: Richiamare un evento imprevisto ed esaminare le azioni associate

1. Nel portale di Azure passare al pannello **Microsoft Defender for Cloud \| Panoramica**.

    >**Nota**: controllare il punteggio di sicurezza. A questo punto dovrebbe essere stato aggiornato. 

2. Nel pannello **Microsoft Defender per il cloud \| Panoramica** fare clic su **Protezioni carico di lavoro** in **Sicurezza del cloud** nel pannello di navigazione sulla sinistra.

3. Nel pannello **Microsoft Defender per il cloud \| Protezioni carico di lavoro** scorrere verso il basso e fare clic sul riquadro **Accesso just-in-time alla VM** da **Protezione avanzata**.

4. Nel pannello **Accesso just-in-time alla VM**, sul lato destro della riga che fa riferimento alla macchina virtuale **myVM**, fare clic sul pulsante con i **puntini di sospensione (...)**, scegliere **Rimuovi** e quindi **Sì**.

    >**Nota:** se la macchina virtuale non è elencata nel **pannello Macchine** virtuali JUST-in-time, passare al **pannello Macchina** virtuale e fare clic su Configurazione **, fare clic **sull'opzione **Abilita le macchine virtuali** JUST-in-time sotto l'accesso **** JUST-in-time della macchina virtuale. Ripetere il passaggio precedente per tornare a **Microsoft Defender for Cloud** e aggiornare la pagina. Verrà visualizzata la macchina virtuale.

5. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Log attività** e premere **INVIO**.

6. Passare al pannello **Log attività** e verificare che sia presente l'opzione **Delete JIT Network Access Policies** (Elimina criteri di accesso alla rete JIT). 

    >**Nota**: La visualizzazione dell'operazione potrebbe richiedere alcuni minuti. **Aggiornare** la pagina se non viene visualizzata.

7. Nel portale di Azure tornare al pannello **Microsoft Sentinel \| Panoramica**.

8. Nel pannello **Microsoft Sentinel \| Panoramica** esaminare il dashboard e verificare che sia presente un evento imprevisto corrispondente all'eliminazione dei criteri di accesso JIT alle macchine virtuali.

    >**Nota**: la visualizzazione degli avvisi nel pannello **Microsoft Sentinel \| Panoramica** può richiedere fino a 5 minuti. Se l'avviso non viene visualizzato, eseguire la regola di query a cui si faceva riferimento nell'attività precedente per verificare che l'attività di eliminazione dei criteri di accesso just-in-time sia stata propagata all'area di lavoro Log Analytics associata all'istanza di Microsoft Sentinel. In caso negativo, ricreare i criteri di accesso just-in-time alle macchine virtuali ed eliminarli nuovamente.

9. Nel pannello **Microsoft Sentinel \| Panoramica** fare clic su **Eventi imprevisti** nella sezione **Gestione delle minacce**.

10. Verificare che nel pannello sia visualizzato un evento imprevisto con livello di gravità medio o alto.

    >**Nota**: la visualizzazione degli eventi imprevisti nel pannello **Microsoft Sentinel \| Eventi imprevisti** può richiedere fino a 5 minuti. 

    >**Nota**: esaminare il pannello**Microsoft Sentinel \| Playbook**. è disponibile il numero di esecuzioni completate con esito positivo o negativo.

    >**Nota**: è possibile assegnare a un evento imprevisto uno stato e un livello di gravità diversi.

> Risultati: È stata creata un'area di lavoro Microsoft Sentinel, che è stata poi connessa ai log di Attività di Azure, sono stati creati un playbook e avvisi personalizzati che si attivano in risposta alla rimozione di criteri di accesso just-in-time alle macchine virtuali ed è stata verificata la validità della configurazione.

**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti. 

1. Nel portale di Azure aprire Cloud Shell facendo clic sulla prima icona nell'angolo in alto a destra. Se richiesto, fare clic su **PowerShell** e su **Crea risorsa di archiviazione**.

2. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

3. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il gruppo di risorse creato in questo lab:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB131415" -Force -AsJob
    ```
4. Chiudere il riquadro **Cloud Shell**.
