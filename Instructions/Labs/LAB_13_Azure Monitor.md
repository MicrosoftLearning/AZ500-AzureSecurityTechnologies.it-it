---
lab:
  title: 13 - Monitoraggio di Azure
  module: Module 04 - Manage security operations
---

# <a name="lab-13-azure-monitor"></a>Lab 13: Monitoraggio di Azure
# <a name="student-lab-manual"></a>Manuale del lab per gli studenti

## <a name="lab-scenario"></a>Scenario del lab

È stato chiesto di creare un modello di verifica per il monitoraggio delle prestazioni delle macchine virtuali. In particolare, sarà necessario:

- Configurare una macchina virtuale in modo che sia possibile raccogliere dati di telemetria e log.
- Mostrare i dati di telemetria e i log che è possibile raccogliere.
- Mostrare come i dati possono essere usati e sottoposti a query. 

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## <a name="lab-objectives"></a>Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Raccogliere dati da una macchina virtuale di Azure con Monitoraggio di Azure

## <a name="azure-monitor"></a>Monitoraggio di Azure

![image](https://user-images.githubusercontent.com/91347931/157536648-0a286514-a7e2-4058-9dea-e42da21eef76.png)

## <a name="instructions"></a>Istruzioni

### <a name="exercise-1-collect-data-from-an-azure-virtual-machine-with-azure-monitor"></a>Esercizio 1: Raccogliere dati da una macchina virtuale di Azure con Monitoraggio di Azure

### <a name="exercise-timing-20-minutes"></a>Durata dell'esercizio: 20 minuti

In questo esercizio verranno eseguite le attività seguenti: 

- Attività 1: Distribuire una macchina virtuale di Azure 
- Attività 2: Creare un'area di lavoro Log Analytics
- Attività 3: Abilitare l'estensione macchina virtuale Log Analytics
- Attività 4: Raccogliere dati relativi a eventi e prestazioni della macchina virtuale
- Attività 5: Visualizzare ed eseguire query sui dati raccolti 

#### <a name="task-1-deploy-an-azure-virtual-machine"></a>Attività 1: Distribuire una macchina virtuale di Azure

1. Accedere al portale di Azure **`https://portal.azure.com/`** .

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Aprire Cloud Shell facendo clic sulla prima icona in alto a destra nel portale di Azure. Se richiesto, selezionare **PowerShell** e **Crea risorsa di archiviazione**.

3. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

4. Nella sessione di PowerShell all'interno del pannello Cloud Shell eseguire il comando seguente per creare un gruppo di risorse che verrà usato in questo lab:
  
    ```powershell
    New-AzResourceGroup -Name AZ500LAB131415 -Location 'EastUS'
    ```

    >**Nota**: questo gruppo di risorse verrà usato per i lab 13, 14 e 15. 

5. Nella sessione di PowerShell all'interno del pannello Cloud Shell eseguire il comando seguente per creare una nuova macchina virtuale di Azure. 

    ```powershell
    New-AzVm -ResourceGroupName "AZ500LAB131415" -Name "myVM" -Location 'EastUS' -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName   "myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress" -PublicIpSku Standard -OpenPorts 80,3389 -Size Standard_DS1_v2 
    ```
    
6.  Verranno chieste le seguenti credenziali.

    |Impostazione|Valore|
    |---|---|
    |User |**localadmin**|
    |Password|**Usare la password personale creata in Lab 04 > Esercizio 1 > Attività 1 > Passaggio 9.**|

    >**Nota**: attendere il completamento della distribuzione. 

7. Nella sessione di PowerShell all'interno del pannello Cloud Shell eseguire il comando seguente per verificare che sia stata creata la macchina virtuale denominata **myVM** e che il parametro **ProvisioningState** sia impostato su **Operazione riuscita**.

    ```powershell
    Get-AzVM -Name 'myVM' -ResourceGroupName 'AZ500LAB131415' | Format-Table
    ```

8. Chiudere il riquadro Cloud Shell. 

#### <a name="task-2-create-a-log-analytics-workspace"></a>Attività 2: Creare un'area di lavoro Log Analytics

In questa attività si creerà un'area di lavoro Log Analytics. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **aree di lavoro Log Analytics** e premere **INVIO**.

2. Nel pannello **Aree di lavoro Log Analytics** fare clic su  **+ Crea**.

3. Nella scheda **Dati principali** del pannello **Crea area di lavoro Log Analytics** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |---|---|
    |Subscription|Nome della sottoscrizione di Azure usata in questo lab|
    |Resource group|**AZ500LAB131415**|
    |Nome|Qualunque nome univoco a livello globale, valido|
    |Region|**Stati Uniti orientali**|

4. Selezionare **Rivedi e crea**.

5. Nella scheda **Rivedi e crea** del pannello **Crea area di lavoro Log Analytics** selezionare **Crea**.

#### <a name="task-3-enable-the-log-analytics-virtual-machine-extension"></a>Attività 3: Abilitare l'estensione macchina virtuale Log Analytics

In questa attività si abiliterà l'estensione macchina virtuale Log Analytics. Questa estensione installa l'agente di Log Analytics in macchine virtuali Windows e Linux. L'agente raccoglie i dati dalla macchina virtuale e li trasferisce all'area di lavoro Log Analytics designata. Al termine dell'installazione, l'agente viene automaticamente aggiornato per garantire la disponibilità delle funzionalità e delle correzioni più recenti. 

1. Nel portale di Azure tornare al pannello **Area di lavoro Log Analytics** e nell'elenco delle aree di lavoro fare clic sulla voce che rappresenta l'area di lavoro creata nell'attività precedente.

2. Nella pagina **Panoramica** del pannello Area di lavoro Log Analytics, nella sezione **Connettere un'origine dati** fare clic sulla voce **Macchine virtuali di Azure**.

    >**Nota**: per la corretta installazione dell'agente è necessario che la macchina virtuale sia in esecuzione.

3. Nell'elenco delle macchine virtuali individuare la voce che rappresenta la macchina virtuale di Azure **myVM** distribuita nella prima attività di questo esercizio e osservare come sia elencata come **Non connessa**.

4. Fare clic sulla voce **myVM** e quindi, nel pannello **myVM**, fare clic su **Connetti**. 

5. Attendere che la macchina virtuale si connetta all'area di lavoro Log Analytics.

    >**Nota**: questa operazione potrebbe richiedere alcuni minuti. Lo **Stato** visualizzato nel pannello **myVM** passerà da **In fase di connessione** a **Questa area di lavoro**. 

#### <a name="task-4-collect-virtual-machine-event-and-performance-data"></a>Attività 4: Raccogliere dati relativi a eventi e prestazioni della macchina virtuale

In questa attività verrà configurata la raccolta del Registro di sistema di Windows e vari contatori delle prestazioni comuni. Verranno esaminate anche altre origini disponibili.

1. Nel portale di Azure tornare all'area di lavoro Log Analytics creata in precedenza in questo esercizio.

2. Nel riquadro Area di lavoro Log Analytics, nella sezione**Impostazioni** fare clic su **Gestione degli agenti legacy**.

3. Nel pannello **Configurazione agenti** consultare le impostazioni configurabili, ad esempio Registri eventi di Windows, Contatori delle prestazioni di Windows, Contatori delle prestazioni di Linux, Log IIS e Syslog. 

4. Assicurarsi che l'opzione **Registri eventi di Windows** sia selezionata, fare clic su **+ Aggiungi registro eventi di Windows** e, nell'elenco dei tipi di log eventi, selezionare **Sistema**.

    >**Nota**: in questo modo vengono aggiunti i registri eventi all'area di lavoro. Altre opzioni includono, ad esempio, **Eventi hardware** o il **Servizio di gestione delle chiavi**.  

5. Deselezionare la casella di controllo **Informazioni**, lasciando selezionate le caselle di controllo **Errore** e **Avviso**.

6. Selezionare **Contatori delle prestazioni di Windows**, fare clic su **+ Aggiungi contatore delle prestazioni**, consultare l'elenco dei contatori delle prestazioni disponibili e aggiungere i contatori seguenti:

    - Memoria(\*)\MB disponibili memoria
    - Processo(\*)\\% tempo processore
    - Event Tracing for Windows\Utilizzo memoria totale --- Pool non di paging
    - Event Tracing for Windows\Utilizzo memoria totale --- Pool di paging

    >**Nota**: i contatori vengono aggiunti e configurati con un intervallo di raccolta dei campioni di 60 secondi.
  
7. Nel pannello **Configurazione agenti** fare clic su **Applica**.

#### <a name="task-5-view-and-query-collected-data"></a>Attività 5: Visualizzare ed eseguire query sui dati raccolti

In questa attività si eseguirà una ricerca log nella raccolta dati. 

1. Nel portale di Azure tornare all'area di lavoro Log Analytics creata in precedenza in questo esercizio.

2. Nel pannello Area di lavoro Log Analytics, nella sezione **Generale**, fare clic su **Log**.

3. Se necessario, chiudere la finestra di **benvenuto a Log Analysis**. 

4. Nel pannello **Query**, nella colonna **Tutte le query**, scorrere verso il basso fino alla fine dell'elenco dei tipi di risorse e fare clic su **Macchine virtuali**.
    
5. Esaminare l'elenco delle query predefinite, selezionare **Uso memoria e CPU** e fare clic sul pulsante **Esegui** corrispondente.

    >**Nota**: è possibile iniziare con la query **Memoria disponibile della macchina virtuale**. Se non si ottiene alcun risultato, verificare che l'ambito sia impostato sulla macchina virtuale.

6. La query verrà automaticamente aperta in una nuova scheda di query. 

    >**Nota**: Log Analytics usa il linguaggio di query Kusto. È possibile personalizzare query esistenti oppure crearne nuove. 

    >**Nota**: i risultati della query selezionata vengono visualizzati automaticamente sotto il pannello della query. Per eseguire nuovamente la query, fare clic su **Esegui**.

    >**Nota**: poiché la macchina virtuale è stata appena creata, è possibile che non contenga ancora dati. 

    >**Nota**: è possibile visualizzare i dati in vari formati. È anche possibile creare una regola di avviso in base ai risultati della query.

    >**Nota**: è possibile generare un carico aggiuntivo sulla macchina virtuale di Azure distribuita nei passaggi precedenti di questo lab seguendo questa procedura:

    1. Passare al pannello della macchina virtuale di Azure.
    2. Nel panello della macchina virtuale di Azure, nella sezione **Operazioni**, selezionare **Esegui comando** e nel pannello **RunPowerShellScript** digitare lo script seguente e fare clic su **Esegui**:
    3. 
       ```cmd
       cmd
       :loop
       dir c:\ /s > SWAP
       goto loop
       ```
       
    4. Tornare al pannello Log Analytics ed eseguire nuovamente la query. Potrebbe essere necessario attendere alcuni minuti per poter raccogliere i dati ed eseguire nuovamente la query.

> Risultati: è stata usata un'area di lavoro Log Analytics per configurare origini dati e log di query. 

**Pulire le risorse**

>**Nota**: non rimuovere le risorse da questo lab poiché saranno necessarie per i lab sul Centro sicurezza di Azure e su Azure Sentinel.
 
