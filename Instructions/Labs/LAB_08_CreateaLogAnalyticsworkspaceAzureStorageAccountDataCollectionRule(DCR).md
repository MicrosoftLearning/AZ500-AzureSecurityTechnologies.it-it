---
lab:
  title: '08 - Creare un''area di lavoro Log Analytics, un account Archiviazione di Azure e una regola di raccolta dati'
  module: Module 03 - Configure and manage threat protection by using Microsoft Defender for Cloud
---

# Lab 08: Creare un'area di lavoro Log Analytics, un account Archiviazione di Azure e una regola di raccolta dati

# Manuale del lab per gli studenti

## Scenario laboratorio

In qualità di tecnico della sicurezza di Azure per un'azienda di tecnologia finanziaria, si ha l'incarico di migliorare il monitoraggio e la visibilità della sicurezza in tutte le macchine virtuali di Azure usate per l'elaborazione delle transazioni finanziarie e la gestione dei dati sensibili dei clienti. Il team di sicurezza richiede log dettagliati e metriche delle prestazioni da queste macchine virtuali per rilevare potenziali minacce e ottimizzare le prestazioni del sistema. Il Chief Information Security Officer (CISO) ha chiesto di implementare una soluzione che raccoglie eventi di sicurezza, log di sistema e contatori delle prestazioni. È stato assegnato per configurare l'agente di Monitoraggio di Azure insieme alle regole di raccolta dati (DCR) per centralizzare la raccolta dei log e il monitoraggio delle prestazioni.



> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verranno completati gli esercizi seguenti:

- Esercizio 1: Distribuire una macchina virtuale di Azure
- Esercizio 2: Creare un'area di lavoro Log Analytics
- Esercizio 3: Creare un account di archiviazione di Azure
- Esercizio 4: Creare una regola di confronto dei dati
  
## Istruzioni

### Esercizio 1: Distribuire una macchina virtuale di Azure

### Intervallo esercizio: 10 minuti

In questo esercizio si completeranno le seguenti attività: 

#### Attività 1: Distribuire una macchina virtuale di Azure

1. Accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Aprire Cloud Shell facendo clic sulla prima icona in alto a destra nel portale di Azure. Se richiesto, selezionare **PowerShell**.

3. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

4. **Nella finestra Attività iniziali** lasciare invariata l'impostazione predefinita: **Selezionare una sottoscrizione per iniziare. Facoltativamente, è possibile montare un account di archiviazione per rendere persistenti i file tra le sessioni. Nessun account di archiviazione necessario.**

5. **Dal menu a discesa Sottoscrizione** selezionare il **lodsubscription.**

6. Lasciare **deselezionata l'opzione Usa una rete** virtuale privata esistente, quindi fare clic su **Applica.**

7. Nella sessione di PowerShell all'interno del pannello Cloud Shell eseguire il comando seguente per creare un gruppo di risorse che verrà usato in questo lab:
  
    ```powershell
    New-AzResourceGroup -Name AZ500LAB131415 -Location 'EastUS'
    ```

    >**Nota**: questo gruppo di risorse verrà usato per i lab 8, 9 e 10.

8. Nella sessione di PowerShell nel riquadro Cloud Shell, eseguire quanto segue per abilitare la crittografia nell'host (EAH)
   
   ```powershell
    Register-AzProviderFeature -FeatureName "EncryptionAtHost" -ProviderNamespace Microsoft.Compute 
    ```

5. Nella sessione di PowerShell all'interno del pannello Cloud Shell eseguire il comando seguente per creare una nuova macchina virtuale di Azure. 

    ```powershell
    New-AzVm -ResourceGroupName "AZ500LAB131415" -Name "myVM" -Location 'EastUS' -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName   "myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress" -PublicIpSku Standard -OpenPorts 80,3389 -Size Standard_D2s_v3 
    ```
    
6.  Verranno chieste le seguenti credenziali.

    |Impostazione|Valore|
    |---|---|
    |User |**localadmin**|
    |Password|**Usare la password personale creata in Lab 02 > Esercizio 2 > Attività 1 > Passaggio 3.**|

    >**Nota**: attendere il completamento della distribuzione. 

7. Nella sessione di PowerShell all'interno del pannello Cloud Shell eseguire il comando seguente per verificare che sia stata creata la macchina virtuale denominata **myVM** e che il parametro **ProvisioningState** sia impostato su **Operazione riuscita**.

    ```powershell
    Get-AzVM -Name 'myVM' -ResourceGroupName 'AZ500LAB131415' | Format-Table
    ```

8. Chiudere il riquadro Cloud Shell. 

### Esercizio 2: Creare un'area di lavoro Log Analytics

### Intervallo esercizio: 10 minuti

In questo esercizio si completeranno le seguenti attività: 

#### Attività 1: Creare un'area di lavoro Log Analytics

In questa attività si creerà un'area di lavoro Log Analytics. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **aree di lavoro Log Analytics** e premere **INVIO**.

2. Nel pannello **Aree di lavoro Log Analytics** fare clic su  **+ Crea**.

3. Nella scheda **Dati principali** del pannello **Crea area di lavoro Log Analytics** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |---|---|
    |Subscription|Nome della sottoscrizione di Azure usata in questo lab|
    |Gruppo di risorse|**AZ500LAB131415**|
    |Nome|Qualunque nome univoco a livello globale, valido|
    |Area geografica|**Stati Uniti orientali**|

4. Selezionare **Rivedi e crea**.

5. Nella scheda **Rivedi e crea** del pannello **Crea area di lavoro Log Analytics** selezionare **Crea**.

### Esercizio 3: Creare un account di archiviazione di Azure

### Tempo stimato: 10 minuti

In questo esercizio si completeranno le seguenti attività:

#### Attività 1: Creare un account di archiviazione di Azure

In questa attività si creerà un account di archiviazione.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Account di archiviazione** e premere **INVIO**.

2. Nel pannello **Account di archiviazione** nel portale di Azure, fare clic sul pulsante **+ Crea** per creare un nuovo account di archiviazione.

3. Nella scheda **Informazioni di base** del pannello **Crea account di archiviazione** specificare le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    |Impostazione|Valore|
    |---|---|
    |Subscription|Nome della sottoscrizione di Azure usata in questo lab|
    |Gruppo di risorse|**AZ500LAB131415**|
    **Dettagli** istanza |Nome dell'account di archiviazione|qualsiasi nome univoco globale compreso tra 3 e 24 in lunghezza costituito da lettere e cifre|  |Area|**(Stati Uniti) EastUS**|
    |Servizio primario |**Archiviazione BLOB di Azure o Azure Data Lake Storage Gen 2**|
    |Prestazioni |**Standard (account per utilizzo generico v2)**|
    |Ridondanza |**Archiviazione con ridondanza locale**|

5. Nella **scheda Informazioni di base** del pannello **Crea account** di archiviazione fare clic su **Rivedi e crea.** Al termine del processo di convalida, fare clic su **Crea.**

    >**Nota**: attendere che l'account di archiviazione venga creato. L'operazione richiede circa 2 minuti.

### Esercizio 4: Creare una regola di raccolta dati

### Tempo stimato: 15 minuti

In questo esercizio si completeranno le seguenti attività:

#### Attività 1: Creare una regola di raccolta dati.

In questa attività si creerà una regola di raccolta dati.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure, digitare **Monitora** e premere **Invio**.

2. Nel pannello **Impostazioni monitoraggio**, fare clic su  **Regole di raccolta dati.**

3. Fare clic sul **pulsante + Crea** per creare una nuova regola di raccolta dati.

4. Nella scheda **Generale** del pannello **Crea regola di raccolta dati**, specificare le impostazioni seguenti:
  
    |Impostazione|Valore|
    |---|---|
    **Dettagli** regola |Nome regola |**DCR1**|
    |Sottoscrizione|nome della sottoscrizione di Azure in uso in questo lab|  |Gruppo di risorse |****|
    AZ500LAB131415 |Area|**Stati Uniti**|
    orientali |Tipo di piattaforma |**Windows**|
    |Endpoint raccolta dati |*Lasciare vuoto*|

    ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/9b58c4ce-b7a8-4acf-8289-d95b270a6083)


5. Fare clic sul pulsante con etichetta **Avanti: Risorse >** per continuare.
   
6. Nella **pagina Risorse** selezionare + **Aggiungi risorse.**

7. Nella casella **Selezionare un modello di ambito** selezionare la **casella Sottoscrizione** nell'ambito **.**

8. Nella parte inferiore del riquadro **Selezionare un modello di ambito** fare clic su **Applica.**

9. Nella parte inferiore della **pagina Risorse** selezionare **Avanti: Raccogliere e recapitare >.**

10. Fare clic su **+ Aggiungi origine dati**, quindi nella pagina **Aggiungi origine dati**, modificare il menu a discesa **Tipo di origine dati** per visualizzare i **Contatori delle prestazioni.** Lasciare le impostazioni predefinite seguenti:

    |Impostazione|Valore|
    |---|---|
    |**Contatore delle prestazioni**|**Frequenza di campionamento (secondi)**|
    |CPU|60|
    |Memoria|60|
    |Disco|60|
    |Rete|60|

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/a24e44ad-1d10-4533-80e2-bae1b3f6564d)

11. Fare clic sul pulsante con etichetta **Avanti: Destinazione >** per continuare.
  
12. Fare clic su **+ Aggiungi destinazione, modificare il **menu a discesa Tipo di** destinazione** per visualizzare **i log di Monitoraggio di Azure.** Nella finestra **Abbonamento**, verificare che compaia l’*Abbonamento*, quindi modificare il menu a discesa **Account o spazio dei nomi** in modo da riflettere l'area di lavoro Log Analytics creata in precedenza.

13. Fare clic su **Aggiungi origine dati** nella parte inferiore della pagina.
    
    ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/964091e7-bbbc-4ca8-8383-bb2871a1e7f0)

14. Fare clic su **Rivedi e crea**.

    ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/50dd8407-a106-4540-9e14-ae40a3c04830)

15. Cliccare su **Crea**.

> Risultati: È stata distribuita una macchina virtuale di Azure, un'area di lavoro Log Analytics, un account di archiviazione di Azure e una regola di raccolta dati per raccogliere eventi e contatori delle prestazioni dalle macchine virtuali con l'agente di Monitoraggio di Azure.

>**Nota**: non rimuovere le risorse da questo lab, perché sono necessarie per il lab Microsoft Defender per il cloud, il lab "Abilita l'accesso JIT nelle macchine virtuali" e il lab di Microsoft Sentinel
 
