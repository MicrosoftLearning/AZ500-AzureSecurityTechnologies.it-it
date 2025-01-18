---
lab:
  title: 04 - Configurazione e protezione del Registro Azure Container e del servizio Azure Kubernetes
  module: 'Module 01 - Secure compute, storage, and databases'
---

# Lab 04: Configurazione e protezione del Registro Azure Container e del servizio Azure Kubernetes
# Manuale del lab per gli studenti

## Scenario laboratorio

È stato richiesto di distribuire un modello di verifica con Registro Azure Container e il servizio Azure Kubernetes. In particolare, il modello di verifica deve dimostrare:

- L'uso di Dockerfile per creare un'immagine.
- L'uso di Registro Azure Container per archiviare immagini.
- La configurazione di un servizio Azure Kubernetes.
- Protezione e accesso alle applicazioni contenitore sia dall'interno che dall'esterno. 

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Configurazione e protezione del Registro Azure Container e del servizio Azure Kubernetes

## Diagramma di configurazione e protezione del Registro Azure Container e del servizio Azure Kubernetes

![image](https://user-images.githubusercontent.com/91347931/157532250-1104a829-792a-4b6d-beff-fe976e2d5d4b.png)

## Istruzioni

## File del lab:

- **\\Allfiles\\Labs\\09\\nginxexternal.yaml**
- **\\Allfiles\\Labs\\09\\nginxinternal.yaml**

### Esercizio 1: Configurazione e protezione del Registro Azure Container e del servizio Azure Kubernetes

### Tempo stimato: 45 minuti

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Creare un Registro Azure Container
- Attività 2: Creare un Dockerfile, creare un contenitore ed eseguirne il push in Registro Azure Container
- Attività 3: Creare un cluster del servizio Azure Kubernetes
- Attività 4: Concedere al cluster del servizio Azure Kubernetes le autorizzazioni per accedere a Registro Azure Container
- Attività 5: Distribuire un servizio esterno nel servizio Azure Kubernetes
- Attività 6: Verificare che sia possibile accedere a un servizio esterno ospitato dal servizio Azure Kubernetes
- Attività 7: Distribuire un servizio interno nel servizio Azure Kubernetes
- Attività 8: Verificare che sia possibile accedere a un servizio interno ospitato dal servizio Azure Kubernetes

#### Attività 1: Creare un Registro Azure Container

In questa attività verrà creato un gruppo di risorse per il lab e un'istanza di Registro Azure Container.

1. Accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: Accedere al portale di Azure usando un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per questo lab e il ruolo Amministratore globale nel tenant di Microsoft Entra associato alla sottoscrizione.

2. Nel portale di Azure aprire Cloud Shell facendo clic sulla prima icona nell'angolo in alto a destra. Se richiesto, fare clic su **Bash** e su **Crea archivio**.

3. Accertarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **Bash**.

4. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per creare un nuovo gruppo di risorse per questo lab:

    ```sh
    az group create --name AZ500LAB09 --location eastus
    ```

5. Nella sessione di Bash all'interno del riquadro Cloud Shell, eseguire questo comando per verificare la creazione del gruppo di risorse:

    ```
    az group list --query "[?name=='AZ500LAB09']" -o table
    ```

6. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per creare una nuova istanza di Registro Azure Container che abbia un nome univoco a livello globale: 

    ```sh
    az acr create --resource-group AZ500LAB09 --name az500$RANDOM$RANDOM --sku Basic
    ```

7. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per verificare la creazione del nuovo Registro Azure Container:

    ```sh
    az acr list --resource-group AZ500LAB09
    ```

    >**Nota**: registrare il nome del Registro Azure Container. Sarà necessario nell'attività successiva.

#### Attività 2: Creare un Dockerfile, creare un contenitore ed eseguirne il push in Registro Azure Container

In questa attività verrà creato un Dockerfile, da cui verrà creata un'immagine che verrà distribuita nel Registro Azure Container. 

1. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per creare un Dockerfile per la creazione di un'immagine basata su Nginx: 

    ```sh
    echo FROM nginx > Dockerfile
    ```

2. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per creare un'immagine dal Dockerfile ed eseguirne il push nel nuovo Registro Azure Container. 

    >**Nota**: il punto finale al termine della riga di comando è necessario. Definisce la directory corrente come percorso del Dockerfile. 

    ```sh
    ACRNAME=$(az acr list --resource-group AZ500LAB09 --query '[].{Name:name}' --output tsv)

    az acr build --resource-group AZ500LAB09 --image sample/nginx:v1 --registry $ACRNAME --file Dockerfile .
    ```

    >**Nota**: attendere il completamento del comando. L'operazione potrebbe richiedere circa due minuti.

3. Chiudere il riquadro Cloud Shell.

4. Nel portale di Azure passare al gruppo di risorse **AZ500Lab09** e nell'elenco delle risorse fare clic sulla voce che rappresenta l'istanza di Registro Azure Container di cui è stato effettuato il provisioning nell'attività precedente.

5. Nella sezione **Servizi** del pannello Registro container fare clic su **Repository**. 

6. Verificare che l'elenco di repository includa la nuova immagine del contenitore denominata **sample/nginx**.

7. Fare clic sulla voce **sample/nginx** e verificare la presenza del tag **v1** che identifica la versione dell'immagine.

8. Fare clic sulla voce **v1** per visualizzare il manifesto dell'immagine.

    >**Nota**: il manifesto include il digest sha256, la data di creazione del manifesto e le voci della piattaforma. 

#### Attività 3: Creare un cluster del servizio Azure Kubernetes

In questa attività verrà creato un servizio Azure Kubernetes e verranno esaminate le risorse distribuite. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **servizi Kubernetes** e premere **INVIO**.

2. Nel pannello **Servizi Kubernetes** fare clic su **+ Crea** e nel menu a discesa fare clic su **+ Crea un cluster Kubernetes**

3. Nella scheda **Informazioni di base** del pannello **Crea un cluster Kubernetes** selezionare **Configurazione predefinita del cluster** e selezionare **Sviluppo/test ($)**. Specificare ora le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni:

    |Impostazione|Valore|
    |----|----|
    |Subscription|Nome della sottoscrizione di Azure usata in questo lab|
    |Gruppo di risorse|**AZ500LAB09**|
    |Nome del cluster Kubernetes|**MyKubernetesCluster**|
    |Paese|**(Stati Uniti) Stati Uniti orientali**|
    |Zone di disponibilità |**Nessuno**|
    |Metodo di scalabilità|**Manualee**|
    |Numero di nodi|**1**|

4. Fare clic su **Avanti: Pool di nodi >** e, nella scheda **Pool di nodi** del pannello **Crea cluster Kubernetes**, specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |----|----|
    |Abilitare i nodi virtuali|Casella di controllo deselezionata|
    
5. Fare clic su **Avanti: Accesso >**, nella scheda **Accesso** del pannello **Crea cluster Kubernetes** accettare le impostazioni predefinite e fare clic su **Avanti: Rete >**. 

6. Nella scheda **Rete** del pannello **Crea cluster Kubernetes** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |----|----|
    |Configurazione di rete|**Sovrimpressione di Azure CNI**|
    |Prefisso nome DNS|**Lasciare il valore predefinito**|

    >**Nota**: il servizio Azure Kubernetes può essere configurato come cluster privato. In questo modo viene assegnato un indirizzo IP privato al server API per garantire che il traffico di rete tra il server API e i pool di nodi rimanga solo nella rete privata. Per altre informazioni, visitare la pagina [Creare un cluster privato del servizio Azure Kubernetes](https://docs.microsoft.com/en-us/azure/aks/private-clusters).

7. Fare clic su **Avanti: Integrazioni >**, nella scheda **Integrazioni** del pannello **Crea cluster Kubernetes** impostare **Monitoraggio contenitori** su **Disabilitato**. 

    >**Nota**: negli scenari di produzione è necessario abilitare il monitoraggio. Il monitoraggio è disabilitato in questo caso perché non è trattato nel lab. 

8. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: attendere il completamento della distribuzione. L'operazione potrebbe richiedere circa 10 minuti.

9. Al termine della distribuzione, nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Gruppi di risorse** e premere **INVIO**.

10. Nel pannello **Gruppi di risorse**, nell'elenco dei gruppi di risorse, prendere nota del nuovo gruppo di risorse denominato **MC_AZ500LAB09_MyKubernetesCluster_eastus** che contiene i componenti dei nodi del servizio Azure Kubernetes. Esaminare le risorse nel gruppo di risorse. 
    
11. Tornare al pannello **Gruppi di risorse** e fare clic sulla voce **AZ500LAB09**. 

    >**Nota**: nell'elenco delle risorse prendere nota del cluster del servizio Azure Kubernetes e della rete virtuale corrispondente.

12. Nel portale di Azure aprire una sessione di Bash in Cloud Shell. 

    >**Nota**: accertarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **Bash**.

13. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per connettersi al cluster Kubernetes:

    ```sh
    az aks get-credentials --resource-group AZ500LAB09 --name MyKubernetesCluster
    ```

14. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per elencare i nodi del cluster Kubernetes: 

    ```sh
    kubectl get nodes
    ```

    >**Nota**: verificare che lo **Stato** del nodo del cluster sia elencato come **Pronto**.

#### Attività 4: Concedere al cluster del servizio Azure Kubernetes l'autorizzazione per accedere al Registro Azure Container e gestirne la rete virtuale

In questa attività verrà concessa al cluster del servizio Azure Kubernetes l'autorizzazione per accedere al Registro Azure Container e gestirne la rete virtuale. 

1. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per configurare il cluster del servizio Azure Kubernetes per l'uso dell'istanza di Registro Azure Container creata in precedenza in questo lab. 

    ```sh
    ACRNAME=$(az acr list --resource-group AZ500LAB09 --query '[].{Name:name}' --output tsv)

    az aks update -n MyKubernetesCluster -g AZ500LAB09 --attach-acr $ACRNAME
    ```

    >**Nota**: questo comando concede l'assegnazione di ruolo "acrpull" al Registro Azure Container. 

    >**Nota**: il completamento del comando può richiedere alcuni minuti. 

2. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per concedere il ruolo Collaboratore alla rete virtuale del cluster del servizio Azure Kubernetes. 

    ```sh
    RG_AKS=AZ500LAB09

    RG_VNET=MC_AZ500LAB09_MyKubernetesCluster_eastus    

    AKS_VNET_NAME=aks-vnet-30198516
    
    AKS_CLUSTER_NAME=MyKubernetesCluster
    
    AKS_VNET_ID=$(az network vnet show --name $AKS_VNET_NAME --resource-group $RG_VNET --query id -o tsv)
    
    AKS_MANAGED_ID=$(az aks show --name $AKS_CLUSTER_NAME --resource-group $RG_AKS --query identity.principalId -o tsv)
    
    az role assignment create --assignee $AKS_MANAGED_ID --role "Contributor" --scope $AKS_VNET_ID
    ```

#### Attività 5: Distribuire un servizio esterno nel servizio Azure Kubernetes

In questa attività verrà eseguito il download dei file manifesto, verrà modificato il file YAML e verranno applicate le modifiche al cluster. 

1. Nella sessione di Bash all'interno del riquadro Cloud Shell fare clic sull'icona **Carica/Scarica file**, nel menu a discesa fare clic su **Carica**, nella finestra di dialogo **Apri** selezionare **\\Allfiles\\Labs\\09\\nginxexternal.yaml** e quindi fare clic su **Apri**. Successivamente, selezionare **\\Allfiles\\Labs\\09\\nginxinternal.yaml** e fare clic su **Apri**.

2. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per identificare il nome dell'istanza di Registro Azure Container:

    ```sh
    echo $ACRNAME
    ```

    >**Nota**: registrare il nome dell'istanza di Registro Azure Container. Questo valore sarà necessario più avanti in questa attività.

3. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per aprire il file nginxexternal.yaml e modificarne il contenuto. 

    ```sh
    code ./nginxexternal.yaml
    ```

    >**Nota**: si tratta del file yaml *esterno*.

4. Nel riquadro dell'editor scorrere fino alla **riga 24** e sostituire il segnaposto **`<ACRUniquename>`** con il nome del Registro Azure Container.

5. Nell'angolo in alto a destra del riquadro dell'editor fare clic sull'icona con i **puntini di sospensione**, fare clic su **Salva** e quindi su **Chiudi editor**. 

6. Nella sessione di Bash nel riquadro Cloud Shell eseguire questo comando per applicare la modifica al cluster:

    ```sh
    kubectl apply -f nginxexternal.yaml
    ```

7. Nella sessione di Bash all'interno del riquadro Cloud Shell, esaminare l'output del comando eseguito nell'attività precedente per verificare che la distribuzione e il servizio corrispondente siano stati creati. 

    ```
    deployment.apps/nginxexternal created
    service/nginxexternal created
    ```

#### Attività 6: Verificare che sia possibile accedere a un servizio esterno ospitato dal servizio Azure Kubernetes

In questa attività verificare che il contenitore sia accessibile esternamente usando l'indirizzo IP pubblico.

1. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per recuperare informazioni sul servizio nginxexternal, quali nome, tipo, indirizzi IP e porte. 

    ```sh
    kubectl get service nginxexternal
    ```

2. Nella sessione di Bash all'interno del riquadro Cloud Shell esaminare l'output e registrare il valore nella colonna External-IP. Saranno necessari nel passaggio successivo. 

3. Aprire una nuova scheda del browser e passare all'indirizzo IP identificato nel passaggio precedente.

4. Assicurarsi che la pagina **Benvenuti in nginx!** sia visualizzata. 

#### Attività 7: Distribuire un servizio interno nel servizio Azure Kubernetes

In questa attività verrà distribuito il servizio interno nel servizio Azure Kubernetes. 

1. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per aprire il file nginxinternal.yaml e modificarne il contenuto. 

    ```sh
    code ./nginxinternal.yaml
    ```

    >**Nota**: si tratta del file yaml *interno*.

2. Nel riquadro dell'editor scorrere fino alla riga contenente il riferimento all'immagine del contenitore e sostituire il segnaposto **`<ACRUniquename>`** con il nome del Registro Azure Container.

3. Nell'angolo in alto a destra del riquadro dell'editor fare clic sull'icona con i **puntini di sospensione**, fare clic su **Salva** e quindi su **Chiudi editor**. 

4. Nella sessione di Bash nel riquadro Cloud Shell eseguire questo comando per applicare la modifica al cluster:

    ```sh
    kubectl apply -f nginxinternal.yaml
    ```

5.  Nella sessione di Bash all'interno del riquadro Cloud Shell esaminare l'output per verificare che la distribuzione e il servizio siano stati creati:

    ```
    deployment.apps/nginxinternal created
    service/nginxinternal created
    ```

6. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per recuperare informazioni sul servizio nginxinternal, quali nome, tipo, indirizzi IP e porte. 

    ```sh
    kubectl get service nginxinternal
    ```

7. Esaminare l'output nella sessione di Bash all'interno del riquadro Cloud Shell. In questo caso External-IP è un indirizzo IP privato. Se lo stato è **In sospeso**, eseguire di nuovo il comando precedente.

    >**Nota**: registrare questo indirizzo IP. Sarà necessario nell'attività successiva. 

    >**Nota**: per accedere all'endpoint servizio interno, connettersi in modo interattivo a uno dei pod in esecuzione nel cluster. 

    >**Nota**: in alternativa è possibile usare l'indirizzo CLUSTER-IP.

#### Attività 8: Verificare che sia possibile accedere a un servizio interno ospitato dal servizio Azure Kubernetes

In questa attività si userà uno dei pod in esecuzione nel cluster del servizio Azure Kubernetes per accedere al servizio interno. 

1. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per elencare i pod nello spazio dei nomi predefinito nel cluster del servizio Azure Kubernetes:

    ```sh
    kubectl get pods
    ```

2. Nell'elenco dei pod copiare la prima voce nella colonna **NAME**.

    >**Nota**: si tratta del pod che verrà usato nei passaggi successivi.

3. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per connettersi in modo interattivo al primo pod (sostituire il segnaposto `<pod_name>` con il nome copiato nel passaggio precedente):

    ```sh
    kubectl exec -it <pod_name> -- /bin/bash
    ```

4. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire questo comando per verificare che il sito Web nginx sia disponibile tramite l'indirizzo IP privato del servizio (sostituire il segnaposto `<internal_IP>` con l'indirizzo IP registrato nell'attività precedente):

    ```sh
    curl http://<internal_IP>
    ```

5. Chiudere il riquadro Cloud Shell.

> Risultato: il Registro Azure Container e il servizio Azure Kubernetes sono stati configurati e protetti.


**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti.

1. Nel portale di Azure aprire Cloud Shell facendo clic sulla prima icona nell'angolo in alto a destra. 

2. Nel menu a discesa in alto a sinistra del riquadro Cloud Shell selezionare **PowerShell** e, quando richiesto, fare clic su **Conferma**.

3. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il gruppo di risorse creato in questo lab:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB09" -Force -AsJob
    ```

4.  Chiudere il riquadro **Cloud Shell**. 
