---
lab:
  title: 08 - Firewall di Azure
  module: Module 02 - Implement Platform Protection
ms.openlocfilehash: cb13c319b70c994bed74b1079bc4ad8fe6209361
ms.sourcegitcommit: a8470295248a6363987bd5ea47154fe39f8535c3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/09/2022
ms.locfileid: "139703510"
---
# <a name="lab-08-azure-firewall"></a>Lab 08: Firewall di Azure
# <a name="student-lab-manual"></a>Manuale del lab per gli studenti

## <a name="lab-scenario"></a>Scenario del lab

È stato chiesto di installare Firewall di Azure. In questo modo, l'organizzazione potrà controllare l'accesso alla rete in ingresso e in uscita, una parte importante del piano di sicurezza di rete complessivo. In particolare, si vogliono creare e testare i componenti dell'infrastruttura seguenti:

- Una rete virtuale con una subnet per il carico di lavoro e una subnet per il jump host.
- Una macchina virtuale in ogni subnet. 
- Una route personalizzata che garantisce che tutto il traffico del carico di lavoro in uscita dalla subnet per il carico di lavoro usi il firewall.
- Regole dell'applicazione firewall che consentono solo il traffico in uscita verso www.bing.com. 
- Regole di rete del firewall che consentono ricerche di server DNS esterni.

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## <a name="lab-objectives"></a>Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Distribuire e testare un firewall di Azure

## <a name="azure-firewall-diagram"></a>Diagramma di firewall di Azure

![image](https://user-images.githubusercontent.com/91347931/157529954-a1bc434b-2eca-41c1-b875-1f0c977d5e20.png)

## <a name="instructions"></a>Istruzioni

## <a name="lab-files"></a>File del lab:

- **\\Allfiles\\Labs\\08\\template.json**

### <a name="exercise-1-deploy-and-test-an-azure-firewall"></a>Esercizio 1: Distribuire e testare un firewall di Azure

### <a name="estimated-timing-40-minutes"></a>Tempo stimato: 40 minuti

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Usare un modello per distribuire l'ambiente lab. 
- Attività 2: Distribuire un firewall di Azure.
- Attività 3: Creare una route predefinita.
- Attività 4: Configurare una regola dell'applicazione.
- Attività 5: Configurare una regola di rete. 
- Attività 6: Configurare server DNS.
- Attività 7: Testare il firewall. 

#### <a name="task-1-use-a-template-to-deploy-the-lab-environment"></a>Attività 1: Usare un modello per distribuire l'ambiente lab. 

In questa attività si esaminerà e distribuirà l'ambiente lab. 

In questa attività si creerà una macchina virtuale usando un modello di ARM. La macchina virtuale verrà usata nell'ultimo esercizio per questo lab. 

1. Accedere al portale di Azure **`https://portal.azure.com/`** .

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Distribuire un modello personalizzato** e premere **INVIO**.

3. Nel pannello **Distribuzione personalizzata** fare clic sull'opzione **Creare un modello personalizzato nell'editor**.

4. Nel pannello **Modifica modello** fare clic su **Carica file**, individuare il file **\\Allfiles\\Labs\\08\\template.json** e fare clic su **Apri**.

    >**Nota**: esaminare il contenuto del modello e notare che distribuisce una macchina virtuale di Azure che ospita Windows Server 2019 Datacenter.

5. Nel pannello **Modifica modello** fare clic su **Salva**.

6. Nel pannello **Distribuzione personalizzata** assicurarsi che siano configurate le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Subscription|Nome della sottoscrizione di Azure che verrà usata nel lab|
   |Resource group|Fare clic su **Crea nuovo** e digitare il nome **AZ500LAB08**|
   |Location|**(Stati Uniti) Stati Uniti orientali**|

    >**Nota**: per identificare le aree di Azure in cui è possibile effettuare il provisioning di macchine virtuali di Azure, vedere [ **https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/)

7. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: attendere il completamento della distribuzione. L'operazione richiede circa 2 minuti. 

#### <a name="task-2-deploy-the-azure-firewall"></a>Attività 2: Distribuire il firewall di Azure

In questa attività si distribuirà il firewall di Azure nella rete virtuale. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Firewall** e premere **INVIO**.

2. Nel pannello **Firewall** fare clic su **+ Crea**.

3. Nella scheda **Informazioni di base** del pannello **Crea un firewall** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Resource group|**AZ500LAB08**|
   |Nome|**Test-FW01**|
   |Region|**(Stati Uniti) Stati Uniti orientali**|
   |Livello firewall|**Standard**|
   |Gestione del firewall|**Usare Regole del firewall (versione classica) per gestire questo firewall**|
   |Scegliere una rete virtuale|Fare clic sull'opzione **Usa esistente** e selezionare **Test-FW-VN** nell'elenco a discesa|
   |Indirizzo IP pubblico|Fare clic su **Aggiungi nuovo**, digitare il nome **TEST-FW-PIP** e fare clic su **OK**|

4. Fare clic su **Rivedi e crea** e quindi su **Crea**. 

    >**Nota**: attendere il completamento della distribuzione. L'operazione richiede circa 5 minuti. 

5. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Gruppi di risorse** e premere **INVIO**.

6. Nel pannello **Gruppi di risorse** fare clic sulla voce **AZ500LAB08** nell'elenco dei gruppi di risorse.

    >**Nota**: nel pannello del gruppo di risorse **AZ500LAB08** esaminare l'elenco delle risorse. È possibile ordinare le risorse per **Tipo**.

7. Nell'elenco delle risorse fare clic sulla voce che rappresenta il firewall **Test-FW01**.

8. Nel pannello **Test-FW01** identificare l'indirizzo **IP privato** assegnato al firewall. 

    >**Nota**: queste informazioni saranno necessarie nell'attività successiva.


#### <a name="task-3-create-a-default-route"></a>Attività 3: Creare una route predefinita

In questa attività si creerà una route predefinita per la subnet **Workload-SN**. Questa route configurerà il traffico in uscita attraverso il firewall.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Tabelle route** e premere **INVIO**.

2. Nel pannello **Tabelle route** fare clic su **+ Crea**.

3. Nel pannello **Crea tabella di route** specificare le impostazioni seguenti:

   |Impostazione|Valore|
   |---|---|
   |Resource group|**AZ500LAB08**|
   |Region| **Stati Uniti orientali**|
   |Name|**Firewall-route**|

4. Fare clic su **Rivedi e crea** e quindi su **Crea** e attendere il completamento del provisioning. 

5. Nel pannello **Tabelle route** fare clic su **Aggiorna** e quindi sulla voce **Firewall-route** nell'elenco delle tabelle di route.

6. Nella sezione **Impostazioni** nel pannello **Firewall-route** fare clic su **Subnet** e quindi fare clic su **+ Associa** nel pannello **Firewall-route \| Subnet**.

7. Nel pannello **Associa subnet** specificare le impostazioni seguenti:

   |Impostazione|valore|
   |---|---|
   |Rete virtuale|**Test-FW-VN**|
   |Subnet|**Workload-SN**|

    >**Nota**: assicurarsi che per questa route sia selezionata la subnet **Workload-SN** o il firewall non funzionerà correttamente.

8. Fare clic su **OK** per associare il firewall alla subnet della rete virtuale. 

9. Tornare alla sezione **Impostazioni** nel pannello **Firewall-route** e fare clic su **Route** e quindi su **+ Aggiungi**. 

10. Nel pannello **Aggiungi route** specificare le impostazioni seguenti:  

   |Impostazione|valore|
   |---|---|
   |Nome route|**FW-DG**|
   |Prefisso indirizzo|**0.0.0.0/0**
   |Tipo hop successivo|**Appliance virtuale**|
   |Indirizzo hop successivo|Indirizzo IP privato del firewall identificato nell'attività precedente|

    >**Nota**: Firewall di Azure è in realtà un servizio gestito, ma l'appliance virtuale è efficace in questa situazione.
    
11.  Fare clic su **OK** per aggiungere la route. 


#### <a name="task-4-configure-an-application-rule"></a>Attività 4: Configurare una regola dell'applicazione

In questa attività si creerà una regola dell'applicazione che consente l'accesso in uscita verso `www.bing.com`.

1. Nel portale di Azure tornare al firewall **Test-FW01**.

2. Nella sezione **Impostazioni** nel pannello **Test-FW01** fare clic su **Regole (versione classica)** .

3. Nel pannello **Test-FW01 \| Regole (versione classica)** fare clic sulla scheda **Raccolta regole dell'applicazione** e quindi fare clic su **+ Aggiungi raccolta regole dell'applicazione**.

4. Nel pannello **Aggiungi raccolta regole dell'applicazione** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Nome|**App-Coll01**|
   |Priorità|**200**|
   |Azione|**Consentito**|

5. Nel pannello **Aggiungi raccolta regole dell'applicazione** creare una nuova voce nella sezione **FQDN di destinazione** con le impostazioni seguenti, lasciando i valori predefiniti per le altre impostazioni:

   |Impostazione|valore|
   |---|---|
   |name|**AllowGH**|
   |Tipo di origine|**Indirizzo IP**|
   |Origine|**10.0.2.0/24**|
   |Porta protocollo IP|**http:80, https:443**|
   |FQDN di destinazione|**www.bing.com**|

6. Fare clic su **Aggiungi** per aggiungere la regola dell'applicazione basata sugli FQDN di destinazione.

    >**Nota**: Firewall di Azure include una raccolta di regole predefinite per gli FQDN dell'infrastruttura consentiti per impostazione predefinita. Questi nomi di dominio completi sono specifici per la piattaforma e non possono essere usati per altri scopi. 

#### <a name="task-5-configure-a-network-rule"></a>Attività 5: Configurare una regola di rete

In questa attività si creerà una regola di rete che consente l'accesso in uscita a due indirizzi IP sulla porta 53 (DNS).

1. Nel portale di Azure tornare al pannello **Test-FW01 \| Regole (versione classica)** .

2. Nel pannello **Test-FW01 \| Regole (versione classica)** fare clic sulla scheda **Raccolta regole di rete** e quindi fare clic su **+ Aggiungi raccolta regole di rete**.

3. Nel pannello **Aggiungi raccolta regole di rete** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Nome|**Net-Coll01**|
   |Priorità|**200**|
   |Azione|**Consentito**|

4. Nel pannello **Aggiungi raccolta regole di rete** creare una nuova voce nella sezione **Indirizzi IP** specificando le impostazioni seguenti e mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Nome|**AllowDNS**|
   |Protocollo|**UDP**|
   |Tipo di origine|**Indirizzo IP**|
   |Indirizzi di origine|**10.0.2.0/24**|
   |Tipo destinazione|**Indirizzo IP**|
   |Indirizzo di destinazione|**209.244.0.3,209.244.0.4**|
   |Porte di destinazione|**53**|

5. Fare clic su **Aggiungi** per aggiungere la regola di rete.

    >**Nota**: gli indirizzi di destinazione usati in questo caso sono server DNS pubblici noti. 

#### <a name="task-6-configure-the-virtual-machine-dns-servers"></a>Attività 6: Configurare i server DNS della macchina virtuale

In questa attività si configureranno gli indirizzi DNS primario e secondario per la macchina virtuale. Questo non è un requisito del firewall. 

1. Nel portale di Azure tornare al gruppo di risorse **AZ500LAB08**.

2. Nel pannello **AZ500LAB08** fare clic sulla macchina virtuale **Srv-Work** nell'elenco delle risorse.

3. Nella sezione **Impostazioni** nel pannello **Srv-Work** fare clic su **Rete**.

4. Nel pannello **Srv-Work \| Rete** fare clic sul collegamento accanto alla voce **Interfaccia di rete**.

5. Nella sezione **Impostazioni** nel pannello dell'interfaccia di rete fare clic su **Server DNS**, selezionare l'opzione **Personalizzato**, aggiungere i due server DNS cui fa riferimento la regola di rete, **209.244.0.3** e **209.244.0.4**, e fare clic su **Salva** per salvare la modifica.

6. Tornare alla pagina della macchina virtuale **Srv-Work**.

    >**Nota**: attendere il completamento dell'aggiornamento.

    >**Nota**: se si aggiornano i server DNS per un'interfaccia di rete, verrà automaticamente riavviata la macchina virtuale a cui è collegata e, se necessario, anche qualsiasi altra macchina virtuale nello stesso set di disponibilità.

#### <a name="task-7-test-the-firewall"></a>Attività 7: Testare il firewall

In questa attività si testerà il firewall per verificare che funzioni come previsto.

1. Nel portale di Azure tornare al gruppo di risorse **AZ500LAB08**.

2. Nel pannello **AZ500LAB08** fare clic sulla macchina virtuale **Srv-Jump** nell'elenco delle risorse.

3. Nel pannello **Srv-Jump** fare clic su **Connetti** e nel menu a discesa fare clic su **RDP**. 

4. Fare clic su **Scarica file RDP** e usare il file per connettersi alla macchina virtuale di Azure **Srv-Jump** tramite Desktop remoto. Quando viene chiesto di eseguire l'autenticazione, specificare le credenziali seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome utente|**localadmin**|
   |Password|**Pa55w.rd1234**|

    >**Nota**: i passaggi seguenti vengono eseguiti nella sessione di Desktop remoto nella macchina virtuale di Azure **Srv-Jump**. 

    >**Nota**: ci si connetterà alla macchina virtuale **Srv-Work**. Questa operazione viene eseguita in modo da poter testare la possibilità di accedere al sito Web bing.com.  

5. All'interno della sessione di Desktop remoto in **Srv-Jump** fare clic con il pulsante destro del mouse su **Avvia** e nel menu di scelta rapida fare clic su **Esegui**. Nella finestra di dialogo **Esegui** eseguire il comando seguente per connettersi a **Srv-Work**. 

    ```
    mstsc /v:Srv-Work
    ```

6. Quando viene chiesto di eseguire l'autenticazione, specificare le credenziali seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome utente|**localadmin**|
   |Password|**Pa55w.rd1234**|

    >**Nota**: attendere che venga stabilita la sessione di Desktop remoto e che venga caricata l'interfaccia Server Manager.

7. All'interno della sessione di Desktop remoto in **Srv-Work** fare clic su **Server locale** in **Server Manager** e quindi fare clic su **Protezione avanzata di Internet Explorer**.

8. Nella finestra di dialogo **Protezione avanzata di Internet Explorer** impostare entrambe le opzioni su **No** e fare clic su **OK**.

9. All'interno della sessione di Desktop remoto in **Srv-Work** avviare Internet Explorer e passare a **`https://www.bing.com`** . 

    >**Note**: il sito Web verrà visualizzato correttamente. Il firewall consente di accedere.

10. Passare a **`http://www.microsoft.com/`**

    >**Nota**: nella pagina del browser verrà visualizzato un messaggio simile a questo: `HTTP request from 10.0.2.4:xxxxx to microsoft.com:80. Action: Deny. No rule matched. Proceeding with default action.`Si tratta di un comportamento previsto, in quanto il firewall blocca l'accesso a questo sito Web. 

11. Terminare entrambe le sessioni di Desktop remoto.

> Risultato: è stato configurato e testato il firewall di Azure.

**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti. 

1. Nel portale di Azure aprire Cloud Shell facendo clic sulla prima icona nell'angolo in alto a destra. Se richiesto, fare clic su **PowerShell** e su **Crea risorsa di archiviazione**.

2. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

3. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il gruppo di risorse creato in questo lab:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB08" -Force -AsJob
    ```
4. Chiudere il riquadro **Cloud Shell**. 
