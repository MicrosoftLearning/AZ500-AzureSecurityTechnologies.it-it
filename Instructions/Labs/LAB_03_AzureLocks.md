---
lab:
  title: 03 - Blocchi di Resource Manager
  module: Module 01 - Manage Identity and Access
---

# Lab 03: Blocchi di Resource Manager
# Manuale del lab per gli studenti

## Scenario del lab 

È stato chiesto di creare un modello di verifica che mostra come è possibile usare blocchi delle risorse per impedire eliminazioni o modifiche accidentali. In particolare, è necessario:

- Creare un blocco ReadOnly

- Creare un blocco di eliminazione

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 
 
## Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Blocchi di Resource Manager

## Diagramma dei blocchi di Resource Manager

![image](https://user-images.githubusercontent.com/91347931/157514986-1bf6a9ea-4c7f-4487-bcd7-542648f8dc95.png)

## Istruzioni

### Esercizio 1: Blocchi di Resource Manager

#### Tempo stimato: 20 minuti

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Creare un gruppo di risorse con un account di archiviazione.
- Attività 2: Aggiungere un blocco ReadOnly nell'account di archiviazione. 
- Attività 3: Testare il blocco ReadOnly. 
- Attività 4: Rimuovere il blocco ReadOnly e creare un blocco di eliminazione.
- Attività 5: Testare il blocco di eliminazione.

#### Attività 1: Creare un gruppo di risorse con un account di archiviazione.

In questa attività si creeranno un gruppo di risorse e un account di archiviazione per il lab. 

1. Accedere al portale di Azure **`https://portal.azure.com/`** .

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

1. Aprire Cloud Shell facendo clic sulla prima icona in alto a destra nel portale di Azure. Se richiesto, selezionare **PowerShell** e **Crea risorsa di archiviazione**.

1. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

1. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per creare un gruppo di risorse (verificare con il docente il valore del parametro location):

    ```powershell
    New-AzResourceGroup -Name AZ500LAB03 -Location 'EastUS'
    
    Confirm
    Provided resource group already exists. Are you sure you want to update it?
    [Y] Yes [N] No [S] Suspend [?] Help (default is "Y"): Y
    ```
1. Nella sessione di PowerShell all'interno del riquadro Cloud Shell digitare **S** e premere il tasto INVIO.

1. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per creare un account di archiviazione nel nuovo gruppo di risorse creato:
    
    ```powershell
    New-AzStorageAccount -ResourceGroupName AZ500LAB03 -Name (Get-Random -Maximum 999999999999999) -Location  EastUS -SkuName Standard_LRS -Kind StorageV2 
    ```

   >**Nota**: attendere che l'account di archiviazione venga creato. Questa operazione può richiedere alcuni minuti. 

1. Chiudere il riquadro Cloud Shell.

#### Attività 2: Aggiungere un blocco ReadOnly nell'account di archiviazione. 

In questa attività si aggiungerà un blocco di sola lettura all'account di archiviazione. In questo modo, è possibile proteggere la risorsa da eliminazioni o modifiche accidentali. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Gruppi di risorse** e premere **INVIO**.

1. Nel pannello **Gruppi di risorse** selezionare la voce del gruppo di risorse **AZ500LAB03**.

1. Nel pannello del gruppo di risorse **AZ500LAB03** selezionare il nuovo account di archiviazione nell'elenco delle risorse. 

1. Nella sezione **Impostazioni** fare clic sull'icona "Blocchi".

1. Fare clic su **+ Aggiungi** e specificare le impostazioni seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome del blocco|**Blocco ReadOnly**|
   |Tipo di blocco|**Sola lettura**|

1. Fare clic su **OK**. 

   >**Nota**: l'account di archiviazione è ora protetto da eliminazioni e modifiche accidentali.

#### Attività 3: Testare il blocco ReadOnly 

1. Nella sezione **Impostazioni** del pannello dell'account di archiviazione fare clic su **Configurazione**.

1. Impostare l'opzione **Trasferimento sicuro obbligatorio** su **Disabilitato** e quindi fare clic su **Salva**.

1. Sarà possibile notare una notifica che indica **Non è stato possibile aggiornare l'account di archiviazione**.

1. Fare clic sull'icona **Notifiche** sulla barra degli strumenti nella parte superiore del portale di Azure ed esaminare la notifica, che sarà simile al testo seguente: 

    > **"Non è stato possibile aggiornare l'account di archiviazione 'xxxxxxxx'. Errore: L'ambito 'xxxxxxxx' non può eseguire l'operazione di scrittura perché gli ambiti seguenti sono bloccati: '/subscriptions/xxxxx-xxx-xxxx-xxxx-xxxxxxxx/resourceGroups/AZ500LAB03/providers/Microsoft.Storage/storageAccounts/xxxxxxx'. Rimuovere il blocco e riprovare."**

1. Tornare al pannello **Configurazione** dell'account di archiviazione e fare clic su **Ignora**. 

1. Nel pannello dell'account di archiviazione selezionare **Panoramica** e nel pannello **Panoramica** fare clic su **Elimina**.

1. Nel pannello **Elimina account di archiviazione** digitare il nome dell'account di archiviazione per confermare che si intende procedere e quindi fare clic su **Elimina**.

1. Esaminare la nuova notifica generata, che sarà simile al testo seguente: 

    > **"Non è stato possibile eliminare l'account di archiviazione 'xxxxxxxx'. Errore: L'ambito 'xxxxxxxx' non può eseguire l'operazione di eliminazione perché gli ambiti seguenti sono bloccati: '/subscriptions/xxxxx-xxx-xxxx-xxxx-xxxxxxxx/resourceGroups/AZ500LAB03/providers/Microsoft.Storage/storageAccounts/xxxxxxx'. Rimuovere il blocco e riprovare."**

   >**Nota**: si è ora verificato che un blocco ReadOnly impedisce eliminazioni e modifiche accidentali di una risorsa.

#### Attività 4: Rimuovere il blocco ReadOnly e creare un blocco di eliminazione.

In questa attività viene rimosso il blocco ReadOnly dall'account di archiviazione e viene creato un blocco di eliminazione. 

1. Nel portale di Azure tornare al pannello che mostra le proprietà del nuovo account di archiviazione creato.

1. Nella sezione **Impostazioni** selezionare **Blocchi**.  

1. Nel pannello **Blocchi** fare clic sull'icona **Elimina** all'estrema destra della voce **Blocco ReadOnly**.

1. Fare clic su **+ Aggiungi** e specificare le impostazioni seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome del blocco|**Blocco di eliminazione**|
   |Tipo di blocco|**Elimina**|

1. Fare clic su **OK**. 

#### Attività 5: Testare il blocco di eliminazione.

In questa attività si testerà il blocco di eliminazione. Sarà possibile modificare l'account di archiviazione, ma non eliminarlo. 

1. Nella sezione **Impostazioni** del pannello dell'account di archiviazione fare clic su **Configurazione**.

1. Impostare l'opzione **Trasferimento sicuro obbligatorio** su **Disabilitato** e quindi fare clic su **Salva**.

   >**Nota**: questa volta la modifica riuscirà.

1. Nel pannello dell'account di archiviazione selezionare **Panoramica** e nel pannello **Panoramica** fare clic su **Elimina**.

1. Esaminare la notifica simile al testo seguente: 

    > **Non è possibile eliminare l'account di archiviazione 'xxxxxx' a causa della presenza di un blocco di eliminazione nella risorsa o nel relativo elemento padre. Per poter eliminare la risorsa, è necessario rimuovere i blocchi."**

   >**Nota**: si è ora verificato che un blocco di **eliminazione**consente modifiche di configurazione, ma impedisce l'eliminazione accidentale.

   >**Nota**: usando i blocchi delle risorse è possibile implementare una linea di difesa aggiuntiva contro modifiche accidentali o dannose e/o l'eliminazione delle risorse più importanti. I blocchi delle risorse possono essere rimossi da qualsiasi utente con il ruolo **Proprietario**, ma questa operazione richiede un'attenzione consapevole. I blocchi completano il controllo degli accessi in base al ruolo. 

> Risultati: in questo esercizio si è appreso come usare blocchi di Resource Manager per proteggere le risorse da modifiche ed eliminazioni accidentali.

**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti.

1. Nel portale di Azure aprire Cloud Shell facendo clic sulla prima icona nell'angolo in alto a destra. Se richiesto, fare clic su **Riconnetti**.

1. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il blocco di eliminazione:

    ```powershell
    $storageaccountname = (Get-AzStorageAccount -ResourceGroupName AZ500LAB03).StorageAccountName

    $lockName = (Get-AzResourceLock -ResourceGroupName AZ500LAB03 -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts).Name

    Remove-AzResourceLock -LockName $lockName -ResourceName $storageAccountName  -ResourceGroupName AZ500LAB03 -ResourceType Microsoft.Storage/storageAccounts -Force
    ```

1.  Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il gruppo di risorse:

    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB03" -Force -AsJob
    ```

1.  Chiudere il riquadro **Cloud Shell**. 
