---
lab:
  title: 01 - Controllo degli accessi in base al ruolo
  module: Module 01 - Manage Identity and Access
---


# Lab 01: Controllo degli accessi in base al ruolo
# Manuale del lab per gli studenti

## Scenario laboratorio

È stato chiesto di creare un modello di verifica che mostra come vengono creati utenti e gruppi di Azure e come viene usato il controllo degli accessi in base al ruolo per assegnare ruoli a gruppi. In particolare, è necessario:

- Creare un gruppo Senior Admins contenente l'account utente di Joseph Price come membro.
- Creare un gruppo Junior Admins contenente l'account utente di Isabel Garcia come membro.
- Creare un gruppo Service Desk contenente l'account utente di Dylan Williams come membro.
- Assegnare il ruolo Collaboratore macchina virtuale al gruppo Service Desk. 

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verranno completati gli esercizi seguenti:

- Esercizio 1: Creare il gruppo Senior Admins con l'account utente Joseph Price come membro (portale di Azure). 
- Esercizio 2: Creare il gruppo Junior Admins con l'account utente Isabel Garcia come membro (PowerShell).
- Esercizio 3: Creare il gruppo Service Desk con l'utente Dylan Williams come membro (interfaccia della riga di comando di Azure). 
- Esercizio 4: Assegnare il ruolo Collaboratore macchina virtuale al gruppo Service Desk.

## Diagramma dell'architettura del controllo degli accessi in base al ruolo

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/506cde9c-5242-4438-a793-f88a5434a2b2)

## Istruzioni

### Esercizio 1: Creare il gruppo Senior Admins con l'account utente Joseph Price come membro. 

#### Tempo stimato: 10 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Usare il portale di Azure per creare un account utente per Joseph Price.
- Attività 2: usare il portale di Azure per creare un gruppo Senior Admins e aggiungere l'account utente di Joseph Price al gruppo.

#### Attività 1: Usare il portale di Azure per creare un account utente per Joseph Price 

In questa attività si creerà un account utente per Joseph Price. 

1. Avviare una sessione del browser e accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: Accedere al portale di Azure usando un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per questo lab e il ruolo Amministratore globale nel tenant di Microsoft Entra associato alla sottoscrizione.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Microsoft Entra ID** e premere **Invio**.

3. Nel pannello **Panoramica** del tenant di Microsoft Entra nella sezione **Gestione**, selezionare **Utenti**, quindi selezionare **+ Nuovo utente**.

4. Nel pannello **Nuovo utente** verificare che l'opzione **Crea utente** sia selezionata e specificare le impostazioni seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome utente|**Joseph**|
   |Nome|**Joseph Price**|

5. Fare clic sull'icona di copia accanto a **Nome utente** per copiare l'utente completo.

6. Assicurarsi che sia selezionata l'opzione **Password generata automaticamente** e selezionare la casella di controllo **Mostra password** per identificare la password generata automaticamente. Sarà necessario fornire questa password, insieme al nome utente, a Joseph. 

7. Cliccare su **Crea**.

8. Aggiornare il pannello **Utenti \| Tutti gli utenti** per verificare che il nuovo utente sia stato creato nel tenant di Microsoft Entra.

#### Attività 2: Usare il portale di Azure per creare un gruppo Senior Admins e aggiungere l'account utente di Joseph Price al gruppo.

In questa attività si creerà il gruppo *Senior Admins*, si aggiungerà l'account utente di Joseph Price al gruppo e si configurerà questo utente come proprietario del gruppo.

1. Nel portale di Azure tornare al pannello che mostra il tenant di Microsoft Entra ID. 

2. Nella sezione **Gestisci** fare clic su **Gruppi** e quindi selezionare **+ Nuovo gruppo**.
 
3. Nel pannello **Nuovo gruppo** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Tipo di gruppo|**Sicurezza**|
   |Nome del gruppo|**Senior Admins**|
   |Tipo di appartenenza|**Assegnate**|
    
4. Fare clic sul collegamento **Non sono stati selezionati proprietari**, nel pannello **Aggiungi proprietari** selezionare **Joseph Price** e fare clic su **Seleziona**.

5. Fare clic sul collegamento **Nessun membro selezionato**, nel pannello **Aggiungi membri** selezionare **Joseph Price** e fare clic su **Seleziona**.

6. Tornare nel pannello **Nuovo gruppo** e fare clic su **Crea**.

> Risultato: è stato usato il portale di Azure per creare un utente e un gruppo e assegnare l'utente al gruppo. 

### Esercizio 2: Creare un gruppo Junior Admins contenente l'account utente di Isabel Garcia come membro.

#### Tempo stimato: 10 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Usare PowerShell per creare un account utente per Isabel Garcia.
- Attività 2: Usare PowerShell per creare il gruppo Junior Admins e aggiungere l'account utente di Isabel Garcia al gruppo. 

#### Attività 1: Usare PowerShell per creare un account utente per Isabel Garcia.

In questa attività si creerà un account utente per Isabel Garcia usando PowerShell.

1. **Aprire Cloud Shell facendo clic sull'icona **** di Cloud Shell** nell'angolo superiore destro del portale di Azure.

2. **Se richiesto, configurare Cloud Shell creando un account** di archiviazione. Questa operazione è necessaria **solo la prima volta** che si avvia Cloud Shell.

3. Nel riquadro **Cloud Shell verificare che PowerShell sia selezionato** dal menu a discesa nell'angolo superiore sinistro.

   >**Nota**: per incollare il testo copiato in Cloud Shell, fare clic con il pulsante destro del mouse nella finestra del riquadro e scegliere **Incolla**. In alternativa, è possibile usare la combinazione di tasti **MAIUSC+INS**.

4. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per creare un oggetto profilo password:

    ```powershell
    $passwordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
    ```

5. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per impostare il valore della password all'interno dell'oggetto profilo:
    ```powershell
    $passwordProfile.Password = "Pa55w.rd1234"
    ```

6. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per connettersi a Microsoft Entra ID:

    ```powershell
    Connect-AzureAD
    ```
      
7. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per identificare il nome del tenant di Microsoft Entra: 

    ```powershell
    $domainName = ((Get-AzureAdTenantDetail).VerifiedDomains)[0].Name
    ```

8. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per creare un account utente per Isabel Garcia: 

    ```powershell
    New-AzureADUser -DisplayName 'Isabel Garcia' -PasswordProfile $passwordProfile -UserPrincipalName "Isabel@$domainName" -AccountEnabled $true -MailNickName 'Isabel'
    ```

9. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per elencare gli utenti di Microsoft Entra ID (gli account di Joseph e Isabel saranno inclusi tra quelli elencati): 

    ```powershell
    Get-AzureADUser -All $true | Where-Object {$_.UserPrincipalName -like "*43846135@LOD*"} 
    ```

#### Attività 2: Usare PowerShell per creare il gruppo Junior Admins e aggiungere l'account utente di Isabel Garcia al gruppo.

In questa attività si creerà il gruppo Junior Admins e si aggiungerà l'account utente di Isabel Garcia al gruppo usando PowerShell.

1. Nella stessa sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per **creare un nuovo gruppo di sicurezza** denominato Junior Admins:
   
   ```powershell
   New-AzureADGroup -DisplayName 'Junior Admins43846135' -MailEnabled $false -SecurityEnabled $true -MailNickName JuniorAdmins
   ```
   
2. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per **elencare i gruppi** nel tenant di Microsoft Entra (l'elenco includerà i gruppi Senior Admins e Junior Admins)
   
   ```powershell
   Get-AzureADGroup
   ```

3. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per **ottenere un riferimento** all'account utente di Isabel Garcia:

   ```powershell
   $user = Get-AzureADUser -Filter "UserPrincipalName eq 'Isabel-43846135@LODSPRODMCA.onmicrosoft.com'"
   ```

4. Nella sessione di PowerShell nel riquadro Cloud Shell eseguire quanto segue per aggiungere l'account utente di Isabel al gruppo Junior Admins43846135:
   ```powershell
   Add-AzADGroupMember -MemberUserPrincipalName $user.userPrincipalName -TargetGroupDisplayName "Junior Admins43846135"
   ```

5. Nella sessione di PowerShell nel riquadro Cloud Shell eseguire quanto segue per verificare che il gruppo Junior Admins43846135 contenga l'account utente di Isabel:
   
   ```powershell
    Get-AzADGroupMember -GroupDisplayName "Junior Admins43846135"
    ```
   
> Risultato: è stato usato PowerShell per creare un account utente e un account di gruppo e aggiungere l'account utente all'account di gruppo. 

### Esercizio 3: Creare un gruppo Service Desk contenente l'account utente di Dylan Williams come membro.

#### Tempo stimato: 10 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Usare l'interfaccia della riga di comando di Azure per creare un account utente per Dylan Williams.
- Attività 2: Usare l'interfaccia della riga di comando di Azure per creare il gruppo Service Desk e aggiungere l'account utente di Dylan al gruppo. 

#### Attività 1: Usare l'interfaccia della riga di comando di Azure per creare un account utente per Dylan Williams.

In questa attività si creerà un account utente per Dylan Williams.

1. Nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell selezionare **Bash** e, quando richiesto, fare clic su **Conferma**. 

2. Nella sessione Bash all'interno del riquadro Cloud Shell eseguire il comando seguente per identificare il nome del tenant di Microsoft Entra:

    ```cli
    DOMAINNAME=$(az ad signed-in-user show --query 'userPrincipalName' | cut -d '@' -f 2 | sed 's/\"//')
    ```

3. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire il comando seguente per creare l'utente Dylan Williams. Usare *dominio*.
 
    ```cli
    az ad user create --display-name "Dylan Williams" --password "Pa55w.rd1234" --user-principal-name Dylan@$DOMAINNAME
    ```
      
4. Nella sessione Bash all'interno del riquadro Cloud Shell eseguire il comando seguente per elencare gli account utente Microsoft Entra ID (l'elenco includerà gli account utente di Joseph, Isabel e Dylan)
    
    ```cli
    az ad user list --output table
    ```

#### Attività 2: Usare l'interfaccia della riga di comando di Azure per creare il gruppo Service Desk e aggiungere l'account utente di Dylan al gruppo. 

In questa attività si creerà il gruppo Service Desk e si assegnerà Dylan al gruppo. 

1. Nella stessa sessione di Bash all'interno del riquadro Cloud Shell eseguire il comando seguente per creare un nuovo gruppo di sicurezza denominato Service Desk.

    ```cli
    az ad group create --display-name "Service Desk" --mail-nickname "ServiceDesk"
    ```
 
2. Nella sessione Bash all'interno del riquadro Cloud Shell eseguire il comando seguente per elencare i gruppi di Microsoft Entra ID (l'elenco includerà i gruppi Service Desk, Senior Admins e Junior Admins):

    ```cli
    az ad group list -o table
    ```

3. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire il comando seguente per ottenere un riferimento all'account utente di Dylan Williams: 

    ```cli
    USER=$(az ad user list --filter "displayname eq 'Dylan Williams'")
    ```

4. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire il comando seguente per ottenere la proprietà objectId dell'account utente di Dylan Williams: 

    ```cli
    OBJECTID=$(echo $USER | jq '.[].id' | tr -d '"')
    ```

5. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire il comando seguente per aggiungere l'account utente di Dylan al gruppo Service Desk: 

    ```cli
    az ad group member add --group "Service Desk" --member-id $OBJECTID
    ```

6. Nella sessione di Bash all'interno del riquadro Cloud Shell eseguire il comando seguente per elencare i membri del gruppo Service Desk e verificare che includa l'account utente di Dylan:

    ```cli
    az ad group member list --group "Service Desk"
    ```

7. Chiudere il riquadro Cloud Shell.

> Risultato: usando l'interfaccia della riga di comando di Azure sono stati creati un account utente e un account di gruppo e l'account utente è stato aggiunto al gruppo. 


### Esercizio 4: Assegnare il ruolo Collaboratore macchina virtuale al gruppo Service Desk.

#### Tempo stimato: 10 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Creare un gruppo di risorse. 
- Attività 2: Assegnare le autorizzazioni Collaboratore macchina virtuale al gruppo di risorse Service Desk.  

#### Attività 1: Creare un gruppo di risorse

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Gruppi di risorse** e premere **INVIO**.

2. Nel pannello **Gruppi di risorse** fare clic su **+ Crea** e specificare le impostazioni seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome della sottoscrizione|Il nome della propria sottoscrizione di Azure|
   |Nome gruppo di risorse|**AZ500Lab01**|
   |Ufficio|**Stati Uniti orientali**|

3. Fare clic su **Rivedi e crea** e quindi su **Crea**.

   >**Nota**: attendere che il gruppo di risorse venga distribuito. Usare l'icona **Notifica** (in alto a destra) per controllare lo stato di avanzamento della distribuzione.

4. Tornare al pannello **Gruppi di risorse**, aggiornare la pagina e verificare che il nuovo gruppo di risorse venga visualizzato nell'elenco dei gruppi di risorse.


#### Attività 2: Assegnare a Service Desk le autorizzazioni Collaboratore macchina virtuale. 

1. Nel pannello **Gruppi di risorse** fare clic sulla voce del gruppo di risorse **AZ500LAB01**.

2. Nel pannello **AZ500Lab01** fare clic su **Controllo di accesso (IAM)** nel riquadro centrale.

3. Nel pannello **AZ500Lab01 \| Controllo di accesso (IAM)** fare clic su **+ Aggiungi** e nel menu a discesa fare clic su **Aggiungi un'assegnazione di ruolo**.

4. Nel pannello **Aggiungi assegnazione** di ruolo completare ognuna delle impostazioni seguenti prima di fare clic su Avanti:

   **Nota:** dopo aver completato tutti i passaggi, fare clic su **Avanti**.

   |Impostazione|Valore|
   |---|---|
   |Ruolo nella scheda di ricerca|**Collaboratore macchine virtuali**|
   |Assegna accesso a (nel riquadro Membri)|**Utente, gruppo o entità servizio**|
   |Seleziona (+Seleziona membri)|**Service Desk**|

6. Fare clic su **Verifica e assegna** due volte per creare l'assegnazione di ruolo.

7. Nel pannello **Controllo di accesso (IAM)** selezionare **Assegnazioni di ruolo**.

8. Nel pannello **AZ500Lab01 \| Controllo di accesso (IAM)** digitare **Dylan Williams** nella casella di testo **Cerca per nome o indirizzo di posta** nella scheda **Verifica l'accesso**.

9. Nell'elenco dei risultati della ricerca selezionare l'account utente di Dylan Williams e nel pannello **Assegnazioni di Dylan Williams - AZ500Lab01** visualizzare la nuova assegnazione creata.

10. Chiudere il pannello **Assegnazioni di Dylan Williams - AZ500Lab01**.

11. Ripetere gli stessi ultimi due passaggi per controllare l'accesso per **Joseph Price**. 

> Risultato: sono state assegnate e controllate le autorizzazioni di controllo degli accessi in base al ruolo. 

**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti.

1. Nel portale di Azure aprire Cloud Shell facendo clic sulla prima icona nell'angolo in alto a destra. 

2. Nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell selezionare **PowerShell** e, quando richiesto, fare clic su **Conferma**. 

3. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il gruppo di risorse creato in questo lab:
  
    ```
    Remove-AzResourceGroup -Name "AZ500LAB01" -Force -AsJob
    ```

4.  Chiudere il riquadro **Cloud Shell**. 
