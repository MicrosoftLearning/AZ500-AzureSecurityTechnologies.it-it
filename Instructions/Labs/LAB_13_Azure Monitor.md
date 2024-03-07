---
lab:
  title: 13 - Monitoraggio di Azure
  module: Module 04 - Manage security operations
---

# Lab 13: Monitoraggio di Azure

# Manuale del lab per gli studenti

## Scenario laboratorio

È stato chiesto di raccogliere eventi e contatori delle prestazioni dalle macchine virtuali con l'agente di Monitoraggio di Azure.

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verranno completati gli esercizi seguenti:

- Esercizio 1: Distribuire una macchina virtuale di Azure
- Esercizio 2: Creare un'area di lavoro Log Analytics
- Esercizio 3: Creare un account di archiviazione di Azure
- Esercizio 4: Creare una regola di confronto dei dati.
  
## Istruzioni

### Esercizio 1: Distribuire una macchina virtuale di Azure

### Tempo esercizio: 10 minuti

In questo esercizio si completeranno le seguenti attività: 

- Attività 1: Distribuire una macchina virtuale di Azure. 

#### Attività 1: Distribuire una macchina virtuale di Azure

1. Accedere al portale di Azure **`https://portal.azure.com/`**.

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

    |Impostazione|valore|
    |---|---|
    |User |**localadmin**|
    |Password|**Usare la password personale creata in Lab 04 > Esercizio 1 > Attività 1 > Passaggio 9.**|

    >**Nota**: attendere il completamento della distribuzione. 

7. Nella sessione di PowerShell all'interno del pannello Cloud Shell eseguire il comando seguente per verificare che sia stata creata la macchina virtuale denominata **myVM** e che il parametro **ProvisioningState** sia impostato su **Operazione riuscita**.

    ```powershell
    Get-AzVM -Name 'myVM' -ResourceGroupName 'AZ500LAB131415' | Format-Table
    ```

8. Chiudere il riquadro Cloud Shell. 

### Esercizio 2: Creare un'area di lavoro Log Analytics

### Tempo esercizio: 10 minuti

In questo esercizio si completeranno le seguenti attività: 

- Attività 1: Creare un'area di lavoro Log Analytics.

#### Attività 1: Creare un'area di lavoro Log Analytics

In questa attività si creerà un'area di lavoro Log Analytics. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **aree di lavoro Log Analytics** e premere **INVIO**.

2. Nel pannello **Aree di lavoro Log Analytics** fare clic su  **+ Crea**.

3. Nella scheda **Dati principali** del pannello **Crea area di lavoro Log Analytics** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |---|---|
    |Abbonamento|Nome della sottoscrizione di Azure usata in questo lab|
    |Gruppo di risorse|**AZ500LAB131415**|
    |Name|Qualunque nome univoco a livello globale, valido|
    |Region|**Stati Uniti orientali**|

4. Selezionare **Rivedi e crea**.

5. Nella scheda **Rivedi e crea** del pannello **Crea area di lavoro Log Analytics** selezionare **Crea**.

### Esercizio 3: Creare un account di archiviazione di Azure

### Tempo stimato: 10 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Creare un account di archiviazione di Azure.

#### Attività 1: Creare un account di archiviazione di Azure

In questa attività si creerà un account di archiviazione.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Account di archiviazione** e premere **INVIO**.

2. Nel pannello **Archiviazione account** nel portale di Azure fare clic sul **pulsante + Crea** per creare un nuovo account di archiviazione.

    ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/73eb9241-d642-455a-a1ff-b504670395c0)

3. Nella scheda **Informazioni di base** del pannello **Crea account di archiviazione** specificare le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    |Impostazione|Valore|
    |---|---|
    |Abbonamento|Nome della sottoscrizione di Azure usata in questo lab|
    |Gruppo di risorse|**AZ500LAB131415**|
    |Nome account di archiviazione|Qualsiasi nome univoco globale composto da 3-24 lettere e numeri|
    |Ubicazione|**(Stati Uniti) Stati Uniti orientali**|
    |Prestazioni|**Standard (account v2 per utilizzo generico)**|
    |Ridondanza|**Archiviazione con ridondanza locale**|

4. Nella **scheda Informazioni di base** del **pannello Crea account** di archiviazione fare clic su **Rivedi**, attendere il completamento del processo di convalida e fare clic su **Crea**.

     ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/d443821c-2ddf-4794-87fa-bfc092980eba)

    >**Nota**: attendere che l'account di archiviazione venga creato. L'operazione richiede circa 2 minuti.

### Esercizio 3: Creare una regola di raccolta dati

### Tempo stimato: 15 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Creare una regola di raccolta dati.

#### Attività 1: Creare una regola di raccolta dati.

In questa attività si creerà una regola di raccolta dati.

1. Nella portale di Azure casella **di testo Cerca risorse, servizi e documenti** nella parte superiore della pagina **portale di Azure digitare Monitoraggio** e premere **INVIO**.

2. Nel pannello **Monitoraggio Impostazioni** fare clic su **Regole di raccolta dati.**

  ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/d43e8f94-efb2-4255-9320-210c976fd45e)


3. Nella **scheda Informazioni di base** del pannello **Crea regola** di raccolta dati specificare le impostazioni seguenti:
  
    |Impostazione|valore|
    |---|---|
    |**Dettagli regola**|
    |Nome della regola|**DCR1**|
    |Abbonamento|Nome della sottoscrizione di Azure usata in questo lab|
    |Gruppo di risorse|**AZ500LAB131415**|
    |Region|**Stati Uniti orientali**|
    |Tipo di piattaforma|**Windows**|
    |Endpoint di raccolta dati|*Lasciare vuoto*|

    ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/9b58c4ce-b7a8-4acf-8289-d95b270a6083)


4. Fare clic sul pulsante con etichetta **Avanti: Risorse >** per continuare.

5. Nella **scheda Risorse** selezionare + Aggiungi risorse** e quindi selezionare **** Abilita endpoint raccolta dati.**

    ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/c8388619-c254-4c80-a1ff-dde2f35ed350)

6. Fare clic sul pulsante con etichetta **Avanti: Raccogliere e recapitare >** per continuare.

7. Fare clic su **+ Aggiungi origine** dati, quindi nella **pagina Aggiungi origine** dati modificare il **menu a discesa Tipo di** origine dati per visualizzare **i contatori delle prestazioni.** Lasciare le impostazioni predefinite seguenti:

    |Impostazione|valore|
    |---|---|
    |**Contatore delle prestazioni**|**Frequenza di campionamento (secondi)**|
    |CPU|60|
    |Memoria|60|
    |Disco|60|
    |Rete|60|

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/a24e44ad-1d10-4533-80e2-bae1b3f6564d)

8. Fare clic sul pulsante **Avanti: Destinazione >** per continuare.
  
9. Modificare il **menu a discesa Tipo di** destinazione per visualizzare **i log di Monitoraggio di Azure.** **Nella finestra Sottoscrizione** verificare che venga visualizzata la *sottoscrizione*, quindi modificare il menu a discesa Account o spazio dei nomi** in modo da riflettere l'area **di lavoro Log Analytics creata in precedenza.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/481843f5-94c4-4a8f-bf51-a10d49130bf8)

10. Fare clic su **Aggiungi origine** dati nella parte inferiore della pagina.
    
    ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/964091e7-bbbc-4ca8-8383-bb2871a1e7f0)

13. Fare clic su **Rivedi e crea**.

    ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/50dd8407-a106-4540-9e14-ae40a3c04830)

14. Fai clic su **Crea**.

> Risultati: è stata distribuita una macchina virtuale di Azure, un'area di lavoro Log Analytics, un account di archiviazione di Azure e una regola di raccolta dati per raccogliere gli eventi e i contatori delle prestazioni dalle macchine virtuali con l'agente di Monitoraggio di Azure.

>**Nota**: non rimuovere le risorse da questo lab perché sono necessarie per il lab Microsoft Defender per il cloud e il lab di Microsoft Sentinel.
 
