---
lab:
  title: 02 - Gruppi di sicurezza di rete e gruppi di sicurezza delle applicazioni
  module: Module 01 - Plan and implement security for virtual networks
---

# Lab 02: Gruppi di sicurezza di rete e gruppi di sicurezza delle applicazioni
# Manuale del lab per gli studenti

## Scenario laboratorio

È stato chiesto di implementare l'infrastruttura di rete virtuale dell'organizzazione e di testarla per assicurarsi che funzioni correttamente. In particolare:

- L'organizzazione ha due gruppi di server: server Web e server di gestione.
- Ogni gruppo di server deve trovarsi nel proprio gruppo di sicurezza delle applicazioni. 
- È necessario essere in grado di connettersi tramite RDP ai server di gestione, ma non ai server Web.
- I server Web devono visualizzare la pagina Web IIS quando vi si accede da Internet. 
- Per controllare l'accesso alla rete, devono essere usate regole del gruppo di sicurezza di rete. 

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verranno completati gli esercizi seguenti:

- Esercizio 1: Creare l'infrastruttura di rete virtuale
- Esercizio 2: Distribuire macchine virtuali e testare i filtri di rete

## Diagramma dei gruppi di sicurezza delle applicazioni e di rete

![Diagramma che mostra il flusso di processo delle attività del lab.](../media/network-and-application-security-groups-diagram.png)

## Istruzioni

### Esercizio 1: Creare l'infrastruttura di rete virtuale

### Tempo stimato: 20 minuti

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con l'insegnante che questa sia l'area da usare per la classe. 

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Creare una rete virtuale con una subnet.
- Attività 2: Creare due gruppi di sicurezza delle applicazioni.
- Attività 3: Creare un gruppo di sicurezza di rete e associarlo alla subnet della rete virtuale.
- Attività 4: Creare regole di sicurezza NSG in ingresso per tutto il traffico verso i server Web e le sessioni RDP verso i server di gestione.

#### Attività 1:Creare una rete virtuale

In questa attività si creerà una rete virtuale da usare con i gruppi di sicurezza di rete e delle applicazioni. 

1. Accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Nella portale di Azure casella **di testo Cerca risorse, servizi e documenti** nella parte superiore della pagina portale di Azure digitare +++Reti virtuali+++ e premere **INVIO**.

3. Nel pannello **Reti virtuali** fare clic su **+ Crea**.

4. Nella scheda **Informazioni di base** del pannello **Crea rete virtuale** specificare le impostazioni seguenti lasciando i valori predefiniti per le altre, quindi fare clic su **Avanti: Indirizzi IP**:

    |Impostazione|valore|
    |---|---|
    |Subscription | Nome della sottoscrizione di Azure in uso in questo lab |
    |Gruppo di risorse | Usare il gruppo di risorse fornito denominato **AZ500LAB07** |
    |Nome| +++myVirtualNetwork+++ |
    |Area geografica| **Stati Uniti orientali** |

5. Nella scheda **Indirizzi IP** del pannello **Crea rete virtuale** impostare **Spazio indirizzi IPv4** su **10.0.0.0/16** e, se necessario, nella colonna **Nome della subnet** fare clic su **predefinito**, quindi nel pannello **Modifica subnet** specificare le impostazioni seguenti e fare clic su **Salva**:

    |Impostazione|valore|
    |---|---|
    |Nome subnet|**default**|
    |Intervallo di indirizzi subnet|**10.0.0.0/24**|

6. **Nella scheda Indirizzi** IP della **schermata Crea rete** virtuale fare clic su **Rivedi e crea**.

7. Nella **scheda Rivedi e crea** della **schermata Crea rete** virtuale fare clic su **Crea**.

#### Attività 2: Creare gruppi di sicurezza delle applicazioni

In questa attività si creerà un gruppo di sicurezza delle applicazioni.

1. Nella casella di testo Cerca risorse, servizi e documenti** nella parte superiore della pagina portale di Azure portale di Azure **digitare +++Gruppi di sicurezza applicazioni+++ e premere **INVIO**.

2. Nel pannello **Gruppi di sicurezza delle applicazioni** fare clic su **+ Crea**.

3. Nella scheda **Informazioni di base** del pannello **Crea un gruppo di sicurezza delle applicazioni** specificare le impostazioni seguenti: 

    |Impostazione|valore|
    |---|---|
    | Gruppo di risorse | **AZ500LAB07** |
    | Nome | +++myAsgWebServers+++ |
    | Area geografica | **Stati Uniti orientali** |

    >**Nota**: questo gruppo sarà riservato ai server Web.

4. Fare clic su **Rivedi e crea** e quindi su **Crea**.

5. Tornare nel pannello **Gruppi di sicurezza delle applicazioni** fare clic su **+ Crea**.

6. Nella scheda **Informazioni di base** del pannello **Crea un gruppo di sicurezza delle applicazioni** specificare le impostazioni seguenti: 

    |Impostazione|valore|
    |---|---|
    |Gruppo di risorse|**AZ500LAB07**|
    |Nome| +++myAsgMgmtServers+++ |
    |Area geografica|**Stati Uniti orientali**|

    >**Nota**: questo gruppo sarà riservato ai server di gestione.

7. Fare clic su **Rivedi e crea** e quindi su **Crea**.

#### Attività 3: Creare un gruppo di sicurezza di rete e associarlo alla subnet

In questa attività si creerà un gruppo di sicurezza di rete. 

1. Nella casella di testo Cerca risorse, servizi e documenti** nella parte superiore della pagina portale di Azure portale di Azure **digitare +++Gruppi di sicurezza di rete+++ e premere **INVIO**.

2. Nel pannello **Gruppi di sicurezza di rete** fare clic su **+ Crea**.

3. Nella scheda **Informazioni di base** del pannello **Crea gruppo di sicurezza di rete** specificare le impostazioni seguenti: 

    |Impostazione|valore|
    |---|---|
    | Subscription | Nome della sottoscrizione di Azure in uso in questo lab |
    | Gruppo di risorse | **AZ500LAB07** |
    | Nome | +++myNsg+++ |
    | Area geografica | **Stati Uniti orientali** |

4. Fare clic su **Rivedi e crea** e quindi su **Crea**.

5. Nel portale di Azure tornare al **pannello Gruppi** di sicurezza di rete e selezionare la **voce myNsg**. In alternativa, selezionare **Vai alla risorsa** , se disponibile.

6. Nella sezione Impostazioni del pannello **myNsg** fare clic su **Subnet** e quindi selezionare **+ Associa**.**** 

7. Nel pannello **Associa subnet** specificare le impostazioni seguenti e selezionare **OK**:

    |Impostazione|valore|
    |---|---|
    |Rete virtuale|**myVirtualNetwork**|
    |Subnet|**default**|

#### Attività 4: Creare regole di sicurezza in ingresso per un NSG per tutto il traffico verso i server Web e RDP verso i server. 

1. Nella sezione **Impostazioni** del pannello **myNsg** fare clic su **Regole di sicurezza in ingresso**.

2. Esaminare le regole di sicurezza in ingresso predefinite e quindi fare clic su **+ Aggiungi**.

3. Nel pannello **Aggiungi regola di sicurezza in ingresso** specificare le impostazioni seguenti per autorizzare le porte TCP 80 e 443 per il gruppo di sicurezza delle applicazioni **myAsgWebServers**, lasciando i valori predefiniti per tutte le altre impostazioni: 

    |Impostazione|valore|
    |---|---|
    | Origine | **Any** |
    | Intervalli di porte di origine | * |
    |Destinazione|Nell'elenco a discesa selezionare **Gruppo di sicurezza delle applicazioni** e quindi fare clic su **myAsgWebServers**|
    | Servizioo | **Personalizzazione** |
    |Intervalli porte di destinazione|**80,443**|
    |Protocollo|**TCP**|
    | Azione | **Consenti** |
    |Priorità|**100**|
    |Nome|**Allow-Web-All**|

4. Selezionare il **pulsante Aggiungi** nella **pagina Aggiungi regola** di sicurezza in ingresso per creare la nuova regola in ingresso.

5. Nella sezione **Impostazioni** del pannello **myNsg** fare clic su **Regole di sicurezza in ingresso**, quindi fare clic su **+Aggiungi**.

6. Nel pannello **Aggiungi regola di sicurezza in ingresso** specificare le impostazioni seguenti per autorizzare la porta RDP (TCP 3389) per il gruppo di sicurezza delle applicazioni **myAsgMgmtServers**, lasciando i valori predefiniti per tutte le altre impostazioni: 

    |Impostazione|valore|
    |---|---|
    | Origine | **Any** |
    | Intervalli di porte di origine | * |
    |Destinazione|Nell'elenco a discesa selezionare **Gruppo di sicurezza delle applicazioni** e quindi fare clic su **myAsgMgmtServers**|
    | Servizioo | **Personalizzazione** |
    |Intervalli di porte di destinazione|**3389**|
    |Protocollo|**TCP**|
    | Azione | **Consenti** |
    |Priorità|**110**|
    |Nome|**Allow-RDP-All**|

7. Selezionare **Aggiungi** nella **pagina Aggiungi regola** di sicurezza in ingresso per creare la nuova regola in ingresso. 

> Risultato: è stata completata la distribuzione di una rete virtuale, delle regole in ingresso per la sicurezza della rete e di due gruppi di sicurezza delle applicazioni. 

### Esercizio 2: Distribuire macchine virtuali e testare i filtri di rete

### Tempo stimato: 25 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Creare una macchina virtuale da usare come server Web.
- Attività 2: Creare una macchina virtuale da usare come server di gestione.
- Attività 3: Associare l'interfaccia di rete di ogni macchina virtuale al gruppo di sicurezza delle applicazioni.
- Attività 4: Testare il filtro del traffico di rete.

#### Attività 1: Creare una macchina virtuale da usare come server Web

In questa attività si creerà una macchina virtuale da usare come server Web.

1. Nella casella di testo Cerca risorse, servizi e documenti** nella parte superiore della pagina portale di Azure portale di Azure **digitare +++Macchine virtuali+++ e premere **INVIO**.

2. Nel pannello **Macchine** virtuali fare clic su **+ Crea** e, nell'elenco a discesa, fare clic su **Macchina** virtuale.

3. Nella scheda **Informazioni di base** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|valore|
   |---|---|
   |Subscription|Nome della sottoscrizione di Azure che verrà usata nel lab|
   |Gruppo di risorse|**AZ500LAB07**|
   |Virtual machine name|**myVmWeb**|
   |Paese|**(Stati Uniti) Stati Uniti orientali**|
   |Opzioni di disponibilità|**La ridondanza dell'infrastruttura non è richiesta**
   |Tipo di sicurezza|**Standard**
   |Image|**Windows Server 2022 Datacenter: Azure Edition - x64 Gen 2**|
   |Dimensione|**Standard D2s v3**|
   |Username|**Student**|
   |Password|**Creare la password e prenderne nota per riferimento futuro nei lab successivi**|
   |Conferma password|**Digitare nuovamente la password**|
   |Porte in ingresso pubbliche|**Nessuno**|
   |Usare una licenza esistente di Windows Server |**No**|

    >**Nota**: per le porte in ingresso pubbliche, verrà usato il gruppo di sicurezza di rete creato in precedenza. 

5. Fare clic su **Avanti: Dischi >** e nella scheda **Dischi** del pannello **Crea macchina virtuale** impostare **Tipo di disco del sistema operativo** su **HDD Standard**, quindi fare clic su **Avanti: Rete >**.

6. Nella **scheda Rete** del **pannello Crea una macchina** virtuale selezionare la subnet **myVirtualNetwork** creata in precedenza e la **subnet predefinita (10.0.0.0/24).**

7. In **Gruppo di sicurezza di rete della scheda di interfaccia di rete** selezionare **Nessuno**.

8. Fare clic su **Avanti: Gestione >**, quindi fare clic su **Avanti: Monitoraggio >**. Nella scheda **Gestione** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti:

   |Impostazione|Valore|
   |---|---|
   |Diagnostica di avvio|**Abilita con account di archiviazione gestito (scelta consigliata)**|

9. Fare clic su **Rivedi e crea**, nel pannello **Rivedi e crea** assicurarsi che la convalida sia riuscita e fare clic su **Crea**.

#### Attività 2: Creare una macchina virtuale da usare come server di gestione. 

In questa attività si creerà una macchina virtuale da usare come server di gestione.

1. Nel portale di Azure tornare al **pannello Macchine** virtuali, fare clic su **+ Crea** e, nell'elenco a discesa, fare clic su **Macchina** virtuale.

2. Nella scheda **Informazioni di base** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|valore|
   |---|---|
   |Subscription|Nome della sottoscrizione di Azure che verrà usata nel lab|
   |Gruppo di risorse|**AZ500LAB07**|
   |Virtual machine name|**myVMMgmt**|
   |Paese|(Stati Uniti) Stati Uniti orientali|
   |Opzioni di disponibilità|**La ridondanza dell'infrastruttura non è richiesta**
   |Tipo di sicurezza|**Standard**
   |Image|**Windows Server 2022 Datacenter: Azure Edition - x64 Gen 2**|
   |Dimensione|**Standard D2s v3**|
   |Username|**Student**|
   |Password|**Usare la password personale creata in Lab 02 > Esercizio 2 > Attività 1 > Passaggio 3.**|
   |Porte in ingresso pubbliche|**Nessuno**|
   |Si ha già una licenza di Windows Server|**No**|

    >**Nota**: per le porte in ingresso pubbliche, verrà usato il gruppo di sicurezza di rete creato in precedenza. 

4. Fare clic su **Avanti: Dischi >** e nella scheda **Dischi** del pannello **Crea macchina virtuale** impostare **Tipo di disco del sistema operativo** su **HDD Standard**, quindi fare clic su **Avanti: Rete >**.

5. Nella **scheda Rete** del **pannello Crea una macchina** virtuale selezionare la subnet **myVirtualNetwork** creata in precedenza e la **subnet predefinita (10.0.0.0/24).**

6. In **Gruppo di sicurezza di rete della scheda di interfaccia di rete** selezionare **Nessuno**.

7. Fare clic su **Avanti: Gestione >**, quindi fare clic su **Avanti: Monitoraggio >**. Nella scheda **Gestione** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti:

   |Impostazione|Valore|
   |---|---|
   |Diagnostica di avvio|**Abilita con account di archiviazione gestito (scelta consigliata)**|

8. Fare clic su **Rivedi e crea**, nel pannello **Rivedi e crea** assicurarsi che la convalida sia riuscita e fare clic su **Crea**.

    >**Nota**: attendere che venga effettuato il provisioning di entrambe le macchine virtuali prima di continuare. 

#### Attività 3: Associare l'interfaccia di rete di ogni macchina virtuale al relativo gruppo di sicurezza delle applicazioni.

In questa attività l'interfaccia di rete di ogni macchina virtuale verrà associata al gruppo di sicurezza delle applicazioni corrispondente. L'interfaccia della macchina virtuale myVMWeb verrà associata al gruppo di sicurezza delle applicazioni myAsgWebServers. L'interfaccia della macchina virtuale myVMWeb verrà associata al gruppo di sicurezza delle applicazioni myAsgMgmtServers. 

1. Nel portale di Azure tornare nel pannello **Macchine virtuali** e verificare che lo stato di entrambe le macchine sia **In esecuzione**.

2. Nell'elenco di macchine virtuali fare clic sulla voce **myVMWeb**.

3. Nel pannello **myVMWeb**, nella sezione **Networking**, fare clic su **Impostazioni di rete** e quindi, nel pannello **myVMWeb \| Impostazioni di rete**, fare clic sulla scheda **Gruppi di sicurezza delle applicazioni**.

4. Fare clic su + **Aggiungi i gruppi di sicurezza delle applicazioni**, selezionare **myAsgWebServers** nell'elenco **Gruppo di sicurezza delle applicazioni** e quindi fare clic su **Salva**.

5. Tornare nel pannello **Macchine virtuali** e fare clic sulla voce **myVMMgmt** nell'elenco di macchine virtuali.

6. Nel pannello **myVMMgmt**, nella sezione **Networking**, fare clic su **Impostazioni di networking** e quindi, nel pannello **myVMMgmt \| Impostazioni di rete**, fare clic sulla scheda **Gruppi di sicurezza delle applicazioni**.

7. Fare clic su + **Aggiungi gruppi** di sicurezza delle applicazioni, nell'elenco **Gruppo** di sicurezza delle applicazioni selezionare **myAsgMgmtServers** e quindi fare clic su **Aggiungi**.

#### Attività 4: Testare il filtro del traffico di rete

In questa attività si testeranno i filtri del traffico di rete. Si dovrà essere in grado di accedere tramite RDP alla macchina virtuale myVMMgmnt. Si dovrà essere in grado di connettersi da Internet alla macchina virtuale myVMWeb e visualizzare la pagina Web IIS predefinita.  

1. Tornare nel pannello della macchina virtuale **myVMMgmt**.

2. Nel pannello **Panoramica di myVMMgmt** fare clic su **Connetti e scegliere Connetti**** dal menu **a discesa. 

3. Scaricare il file RDP e usarlo per connettersi alla **macchina virtuale di Azure myVMMgmt** tramite Desktop remoto. Quando viene chiesto di eseguire l'autenticazione, specificare le credenziali seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome utente|**Student**|
   |Password|**Usare la password personale creata in Lab 02 > Esercizio 1 > Attività 1 > Passaggio 9.**|

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

1. Aprire Cloud Shell facendo clic sulla prima icona in alto a destra nel portale di Azure. Se richiesto, selezionare **PowerShell** e **Nessun account di archiviazione necessario**, selezionare il nome della sottoscrizione e quindi selezionare **Applica**.

2. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

3. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il gruppo di risorse creato in questo lab:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB07" -Force -AsJob
    ```

4.  Chiudere il riquadro **Cloud Shell**. 
