---
lab:
  title: 07 - Gruppi di sicurezza di rete e gruppi di sicurezza delle applicazioni
  module: Module 02 - Implement Platform Protection
ms.openlocfilehash: e33f0a1f5c30a86d2b2a47069c6f4d759d60e782
ms.sourcegitcommit: a8470295248a6363987bd5ea47154fe39f8535c3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/09/2022
ms.locfileid: "139703492"
---
# <a name="lab-07-network-security-groups-and-application-security-groups"></a>Lab 07: Gruppi di sicurezza di rete e gruppi di sicurezza delle applicazioni
# <a name="student-lab-manual"></a>Manuale del lab per gli studenti

## <a name="lab-scenario"></a>Scenario del lab

È stato chiesto di implementare l'infrastruttura di rete virtuale dell'organizzazione e di testarla per assicurarsi che funzioni correttamente. In particolare:

- L'organizzazione ha due gruppi di server: server Web e server di gestione.
- Ogni gruppo di server deve trovarsi in uno specifico gruppo di sicurezza delle applicazioni. 
- È necessario essere in grado di connettersi tramite RDP ai server di gestione, ma non ai server Web.
- I server Web devono visualizzare la pagina Web IIS quando vi si accede da Internet. 
- Per controllare l'accesso alla rete, devono essere usate regole del gruppo di sicurezza di rete. 

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## <a name="lab-objectives"></a>Obiettivi del lab

In questo lab verranno completati gli esercizi seguenti:

- Esercizio 1: Creare l'infrastruttura di rete virtuale
- Esercizio 2: Distribuire macchine virtuali e testare i filtri di rete

## <a name="network-and-application-security-groups-diagram"></a>Diagramma dei gruppi di sicurezza delle applicazioni e di rete

![image](https://user-images.githubusercontent.com/91347931/157526438-6da4f68b-db88-4931-a041-8474e66d3fe5.png)

## <a name="instructions"></a>Istruzioni

### <a name="exercise-1-create-the-virtual-networking-infrastructure"></a>Esercizio 1: Creare l'infrastruttura di rete virtuale

### <a name="estimated-timing-20-minutes"></a>Tempo stimato: 20 minuti

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Creare una rete virtuale con una subnet.
- Attività 2: Creare due gruppi di sicurezza delle applicazioni.
- Attività 3: Creare un gruppo di sicurezza di rete e associarlo alla subnet della rete virtuale.
- Attività 4: Creare regole di sicurezza NSG in ingresso per tutto il traffico verso i server Web e le sessioni RDP verso i server di gestione.

#### <a name="task-1--create-a-virtual-network"></a>Attività 1:Creare una rete virtuale

In questa attività si creerà una rete virtuale da usare con i gruppi di sicurezza di rete e delle applicazioni. 

1. Accedere al portale di Azure **`https://portal.azure.com/`** .

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Reti virtuali** e premere **INVIO**.

3. Nel pannello **Reti virtuali** fare clic su **+ Crea**.

4. Nella scheda **Informazioni di base** del pannello **Crea rete virtuale** specificare le impostazioni seguenti lasciando i valori predefiniti per le altre, quindi fare clic su **Avanti: Indirizzi IP**:

    |Impostazione|Valore|
    |---|---|
    |Subscription|Nome della sottoscrizione di Azure usata in questo lab|
    |Resource group|Fare clic su **Crea nuovo** e digitare il nome **AZ500LAB07**|
    |Nome|**myVirtualNetwork**|
    |Region|**Stati Uniti orientali**|

5. Nella scheda **Indirizzi IP** del pannello **Crea rete virtuale** impostare **Spazio indirizzi IPv4** su **10.0.0.0/16** e, se necessario, nella colonna **Nome della subnet** fare clic su **predefinito**, quindi nel pannello **Modifica subnet** specificare le impostazioni seguenti e fare clic su **Salva**:

    |Impostazione|valore|
    |---|---|
    |Nome della subnet|**default**|
    |Intervallo di indirizzi subnet|**10.0.0.0/24**|

6. Nella scheda **Indirizzi IP** del pannello **Crea rete virtuale** fare clic su **Rivedi e crea**.

7. Nella scheda **Rivedi e crea** del pannello **Crea rete virtuale** fare clic su **Crea**.

#### <a name="task-2--create-application-security-groups"></a>Attività 2: Creare gruppi di sicurezza delle applicazioni

In questa attività si creerà un gruppo di sicurezza delle applicazioni.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Gruppi di sicurezza delle applicazioni** e premere **INVIO**.

2. Nel pannello **Gruppi di sicurezza delle applicazioni** fare clic su **+ Crea**.

3. Nella scheda **Informazioni di base** del pannello **Crea un gruppo di sicurezza delle applicazioni** specificare le impostazioni seguenti: 

    |Impostazione|Valore|
    |---|---|
    |Resource group|**AZ500LAB07**|
    |Nome|**myAsgWebServers**|
    |Region|**Stati Uniti orientali**|

    >**Nota**: questo gruppo sarà riservato ai server Web.

4. Fare clic su **Rivedi e crea** e quindi su **Crea**.

5. Tornare nel pannello **Gruppi di sicurezza delle applicazioni** fare clic su **+ Crea**.

6. Nella scheda **Informazioni di base** del pannello **Crea un gruppo di sicurezza delle applicazioni** specificare le impostazioni seguenti: 

    |Impostazione|Valore|
    |---|---|
    |Resource group|**AZ500LAB07**|
    |Nome|**myAsgMgmtServers**|
    |Region|**Stati Uniti orientali**|

    >**Nota**: questo gruppo sarà riservato ai server di gestione.

7. Fare clic su **Rivedi e crea** e quindi su **Crea**.

#### <a name="task-3--create-a-network-security-group-and-associate-the-nsg-to-the-subnet"></a>Attività 3: Creare un gruppo di sicurezza di rete e associarlo alla subnet

In questa attività si creerà un gruppo di sicurezza di rete. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Gruppi di sicurezza di rete** e premere **INVIO**.

2. Nel pannello **Gruppi di sicurezza di rete** fare clic su **+ Crea**.

3. Nella scheda **Informazioni di base** del pannello **Crea gruppo di sicurezza di rete** specificare le impostazioni seguenti: 

    |Impostazione|Valore|
    |---|---|
    |Subscription|Nome della sottoscrizione di Azure usata in questo lab|
    |Resource group|**AZ500LAB07**|
    |Nome|**myNsg**|
    |Region|**Stati Uniti orientali**|

4. Fare clic su **Rivedi e crea** e quindi su **Crea**.

5. Nel portale di Azure tornare nel pannello **Gruppi di sicurezza di rete** e fare clic sulla voce **myNsg**.

6. Nella sezione **Impostazioni** del pannello **myNsg** fare clic su **Subnet** e quindi su **+ Associa**. 

7. Nel pannello **Associa subnet** specificare le impostazioni seguenti e fare clic su **OK**:

    |Impostazione|valore|
    |---|---|
    |Rete virtuale|**myVirtualNetwork**|
    |Subnet|**default**|

#### <a name="task-4-create-inbound-nsg-security-rules-to-all-traffic-to-web-servers-and-rdp-to-the-management-servers"></a>Attività 4: Creare regole di sicurezza NSG in ingresso per tutto il traffico verso i server Web e le sessioni RDP verso i server di gestione. 

1. Nella sezione **Impostazioni** del pannello **myNsg** fare clic su **Regole di sicurezza in ingresso**.

2. Esaminare le regole di sicurezza in ingresso predefinite e quindi fare clic su **+ Aggiungi**.

3. Nel pannello **Aggiungi regola di sicurezza in ingresso** specificare le impostazioni seguenti per autorizzare le porte TCP 80 e 443 per il gruppo di sicurezza delle applicazioni **myAsgWebServers**, lasciando i valori predefiniti per tutte le altre impostazioni: 

    |Impostazione|Valore|
    |---|---|
    |Destination|Nell'elenco a discesa selezionare **Gruppo di sicurezza delle applicazioni** e quindi fare clic su **myAsgWebServers**|
    |Intervalli di porte di destinazione|**80,443**|
    |Protocollo|**TCP**|
    |Priorità|**100**|                                                    
    |Nome|**Allow-Web-All**|

4. Nel pannello **Aggiungi regola di sicurezza in ingresso** fare clic su **Aggiungi** per creare la nuova regola in ingresso. 

5. Nella sezione **Impostazioni** del pannello **myNsg** fare clic su **Regole di sicurezza in ingresso**, quindi fare clic su **+Aggiungi**.

6. Nel pannello **Aggiungi regola di sicurezza in ingresso** specificare le impostazioni seguenti per autorizzare la porta RDP (TCP 3389) per il gruppo di sicurezza delle applicazioni **myAsgMgmtServers**, lasciando i valori predefiniti per tutte le altre impostazioni: 

    |Impostazione|Valore|
    |---|---|
    |Destination|Nell'elenco a discesa selezionare **Gruppo di sicurezza delle applicazioni** e quindi fare clic su **myAsgMgmtServers**|
    |Intervalli di porte di destinazione|**3389**|
    |Protocollo|**TCP**|
    |Priorità|**110**|                                                    
    |Nome|**Allow-RDP-All**|

7. Nel pannello **Aggiungi regola di sicurezza in ingresso** fare clic su **Aggiungi** per creare la nuova regola in ingresso. 

> Risultato: è stata completata la distribuzione di una rete virtuale, delle regole in ingresso per la sicurezza della rete e di due gruppi di sicurezza delle applicazioni. 

### <a name="exercise-2-deploy-virtual-machines-and-test-network-filters"></a>Esercizio 2: Distribuire macchine virtuali e testare i filtri di rete

### <a name="estimated-timing-25-minutes"></a>Tempo stimato: 25 minuti

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Creare una macchina virtuale da usare come server Web.
- Attività 2: Creare una macchina virtuale da usare come server di gestione. 
- Attività 3: Associare l'interfaccia di rete di ogni macchina virtuale al gruppo di sicurezza delle applicazioni.
- Attività 4: Testare il filtro del traffico di rete.

#### <a name="task-1-create-a-virtual-machine-to-use-as-a-web-server"></a>Attività 1: Creare una macchina virtuale da usare come server Web.

In questa attività si creerà una macchina virtuale da usare come server Web.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Macchine virtuali** e premere **INVIO**.

2. Nel pannello **Macchine virtuali** fare clic su **+ Crea** e nell'elenco a discesa fare clic su **+ Macchina virtuale**.

3. Nella scheda **Informazioni di base** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Subscription|Nome della sottoscrizione di Azure che verrà usata nel lab|
   |Resource group|**AZ500LAB07**|
   |Nome macchina virtuale|**myVmWeb**|
   |Region|**(Stati Uniti) Stati Uniti orientali**|
   |Immagine|**Windows Server 2022 Datacenter - Gen2**|
   |Dimensione|**Standard D2s v3**|
   |Username|**Studente**|
   |Password|**Usare di nuovo la password creata nel lab del modulo 6 (Esercizio 1, Attività 2)**|
   |Conferma password|**Digitare nuovamente la password**|
   |Porte in ingresso pubbliche|**Nessuno**|
   |Usare una licenza esistente di Windows Server |**No**|

    >**Nota**: per le porte in ingresso pubbliche, verrà usato il gruppo di sicurezza di rete creato in precedenza. 

4. Fare clic su **Avanti: Dischi >** e nella scheda **Dischi** del pannello **Crea macchina virtuale** impostare **Tipo di disco del sistema operativo** su **HDD Standard**, quindi fare clic su **Avanti: Rete >** .

5. Nella scheda **Rete** del pannello **Crea macchina virtuale** selezionare la rete **myVirtualNetwork** creata in precedenza.

6. In **Gruppo di sicurezza di rete della scheda di interfaccia di rete** selezionare **Nessuno**.

7. Fare clic su **Avanti: Gestione >** e quindi nella scheda **Gestione** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti:

   |Impostazione|Valore|
   |---|---|
   |Diagnostica di avvio|**Abilita con account di archiviazione gestito (scelta consigliata)**|

8. Fare clic su **Rivedi e crea**, nel pannello **Rivedi e crea** assicurarsi che la convalida sia riuscita e fare clic su **Crea**.

#### <a name="task-2-create-a-virtual-machine-to-use-as-a-management-server"></a>Attività 2: Creare una macchina virtuale da usare come server di gestione. 

In questa attività si creerà una macchina virtuale da usare come server di gestione.

1. Nel portale di Azure passare al pannello **Macchine virtuali**, fare clic su **+ Crea** e nell'elenco a discesa fare clic su **+ Macchina virtuale**.

2. Nella scheda **Informazioni di base** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Subscription|Nome della sottoscrizione di Azure che verrà usata nel lab|
   |Resource group|**AZ500LAB07**|
   |Nome macchina virtuale|**myVMMgmt**|
   |Region|(Stati Uniti) Stati Uniti orientali|
   |Immagine|**Windows Server 2022 Datacenter - Gen 2**|
   |Dimensione|**Standard D2s v3**|
   |Username|**Studente**|
   |Password|**Usare di nuovo la password creata nel lab del modulo 6 (Esercizio 1, Attività 2)**|
   |Porte in ingresso pubbliche|**Nessuno**|
   |Si ha già una licenza di Windows Server|**No**|

    >**Nota**: per le porte in ingresso pubbliche, verrà usato il gruppo di sicurezza di rete creato in precedenza. 

3. Fare clic su **Avanti: Dischi >** e nella scheda **Dischi** del pannello **Crea macchina virtuale** impostare **Tipo di disco del sistema operativo** su **HDD Standard**, quindi fare clic su **Avanti: Rete >** .

4. Nella scheda **Rete** del pannello **Crea macchina virtuale** selezionare la rete **myVirtualNetwork** creata in precedenza.

5. In **Gruppo di sicurezza di rete della scheda di interfaccia di rete** selezionare **Nessuno**.

6. Fare clic su **Avanti: Gestione >** e quindi nella scheda **Gestione** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti

   |Impostazione|Valore|
   |---|---|
   |Diagnostica di avvio|**Abilita con account di archiviazione gestito (scelta consigliata)**|

7. Fare clic su **Rivedi e crea**, nel pannello **Rivedi e crea** assicurarsi che la convalida sia riuscita e fare clic su **Crea**.

    >**Nota**: attendere che venga effettuato il provisioning di entrambe le macchine virtuali prima di continuare. 

#### <a name="task-3-associate-each-virtual-machines-network-interface-to-its-application-security-group"></a>Attività 3: Associare l'interfaccia di rete di ogni macchina virtuale al gruppo di sicurezza delle applicazioni.

In questa attività l'interfaccia di rete di ogni macchina virtuale verrà associata al gruppo di sicurezza delle applicazioni corrispondente. L'interfaccia della macchina virtuale myVMWeb verrà associata al gruppo di sicurezza delle applicazioni myAsgWebServers. L'interfaccia della macchina virtuale myVMWeb verrà associata al gruppo di sicurezza delle applicazioni myAsgMgmtServers. 

1. Nel portale di Azure tornare nel pannello **Macchine virtuali** e verificare che lo stato di entrambe le macchine sia **In esecuzione**.

2. Nell'elenco di macchine virtuali fare clic sulla voce **myVMWeb**.

3. Nel pannello **myVMWeb**, nella sezione **Impostazioni**, fare clic su **Rete** e quindi nel pannello **myVMWeb \| Rete** fare clic sulla scheda **Gruppi di sicurezza delle applicazioni**.

4. Fare clic su **Configura i gruppi di sicurezza delle applicazioni**, selezionare **myAsgWebServers** nell'elenco a discesa **Gruppo di sicurezza delle applicazioni** e quindi fare clic su **Salva**.

5. Tornare nel pannello **Macchine virtuali** e fare clic sulla voce **myVMMgmt** nell'elenco di macchine virtuali.

6. Nel pannello **myVMMgmt**, nella sezione **Impostazioni**, fare clic su **Rete** e quindi nel pannello **myVMMgmt \| Rete** fare clic sulla scheda **Gruppi di sicurezza delle applicazioni**.

7. Fare clic su **Configura i gruppi di sicurezza delle applicazioni**, selezionare **myAsgMgmtServers** nell'elenco a discesa **Gruppo di sicurezza delle applicazioni** e quindi fare clic su **Salva**.

#### <a name="task-4-test-the-network-traffic-filtering"></a>Attività 4: Testare il filtro del traffico di rete

In questa attività si testeranno i filtri del traffico di rete. Si dovrà essere in grado di accedere tramite RDP alla macchina virtuale myVMMgmnt. Si dovrà essere in grado di connettersi da Internet alla macchina virtuale myVMWeb e visualizzare la pagina Web IIS predefinita.  

1. Tornare nel pannello della macchina virtuale **myVMMgmt**.

2. Nel pannello **myVMMgmt** fare clic su **Connetti** e nel menu a discesa fare clic su **RDP**. 

3. Fare clic su **Scarica file RDP** e usare il file per connettersi alla macchina virtuale di Azure **myVMMgmt** tramite Desktop remoto. Quando viene chiesto di eseguire l'autenticazione, specificare le credenziali seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome utente|**Studente**|
   |Password|**Usare di nuovo la password creata nel lab del modulo 6 (Esercizio 1, Attività 2)**|

    >**Nota**: verificare che la connessione Desktop remoto sia riuscita. A questo punto è stato confermato che è possibile connettersi tramite Desktop remoto a myVMMgmt.

4. Nel portale di Azure passare al pannello della macchina virtuale **myVMWeb**.

5. Nel pannello **myVMWeb** fare clic su **Esegui comando** nella sezione **Operazioni** e nell'elenco dei comandi fare clic su **RunPowerShellScript**.

6. Nel riquadro **Esegui script di comandi** eseguire il comando seguente per installare il ruolo del server Web in **myVmWeb**:

    ```powershell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    ```

    >**Nota**: attendere il completamento dell'installazione. Questa operazione può richiedere alcuni minuti. A questo punto, è possibile verificare che myVMWeb è accessibile tramite HTTP/HTTPS.

7. Nel portale di Azure tornare nel pannello **myVMWeb**.

8. Nel pannello **myVMWeb** identificare il valore di **Indirizzo IP pubblico** della macchina virtuale di Azure myVmWeb.

9. Aprire un'altra scheda del browser e passare all'indirizzo IP identificato nel passaggio precedente.

    >**Nota**: la pagina del browser dovrebbe visualizzare la pagina predefinita di benvenuto di IIS perché la porta 80 è consentita in ingresso da Internet in base all'impostazione del gruppo di sicurezza delle applicazioni **myAsgWebServers**. L'interfaccia di rete della macchina virtuale di Azure myVMWeb è associata a tale gruppo di sicurezza delle applicazioni. 

> Risultato: è stato verificato che la configurazione del gruppo di sicurezza di rete e del gruppo di sicurezza delle applicazioni funziona e che il traffico viene gestito correttamente. 

**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti. 

1. Aprire Cloud Shell facendo clic sulla prima icona in alto a destra nel portale di Azure. Se richiesto, selezionare **PowerShell** e **Crea risorsa di archiviazione**.

2. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

3. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il gruppo di risorse creato in questo lab:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB07" -Force -AsJob
    ```

4.  Chiudere il riquadro **Cloud Shell**. 
