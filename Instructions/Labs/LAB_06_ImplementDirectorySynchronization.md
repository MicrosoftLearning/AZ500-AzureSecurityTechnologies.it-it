---
lab:
  title: 06 - Implementare la sincronizzazione della directory
  module: Module 01 - Manage Identity and Access
---

# Lab 06: Implementare la sincronizzazione della directory
# Manuale del lab per gli studenti

## Scenario laboratorio

È stato chiesto di creare un modello di verifica che illustri come integrare un ambiente di Active Directory Domain Services (AD DS) locale con un tenant di Azure Active Directory (Azure AD). In particolare, sarà necessario:

- Implementare una foresta di AD DS a singolo dominio distribuendo una VM di Azure che ospita un controller di dominio di AD DS
- Creare e configurare il tenant di Azure AD
- Sincronizzare la foresta di AD DS con il tenant di Azure AD

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verranno completati gli esercizi seguenti:

- Esercizio 1: Distribuire una macchina virtuale di Azure che ospita un controller di dominio di Active Directory
- Esercizio 2: Creare e configurare un tenant di Azure Active Directory
- Esercizio 3: Sincronizzare la foresta di Active Directory con un tenant di Azure Active Directory

## Implementare la sincronizzazione della directory

![image](https://user-images.githubusercontent.com/91347931/157525374-8f740f14-c2db-47b3-98f8-7feb9bc122b5.png)

## Istruzioni

### Esercizio 1: Distribuire una macchina virtuale di Azure che ospita un controller di dominio di Active Directory

### Tempo stimato: 10 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Identificare un nome DNS disponibile per la distribuzione di una macchina virtuale di Azure
- Attività 2: Usare un modello di ARM per distribuire una macchina virtuale di Azure che ospita un controller di dominio di Active Directory

#### Attività 1: Identificare un nome DNS disponibile per la distribuzione di una macchina virtuale di Azure

In questa attività si identificherà un nome DNS per la distribuzione della macchina virtuale di Azure. 

1. Accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Aprire Cloud Shell facendo clic sulla prima icona in alto a destra nel portale di Azure. Se richiesto, fare clic su **PowerShell** e su **Crea risorsa di archiviazione**.

3. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

4. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per identificare un nome DNS disponibile che è possibile usare per la distribuzione di una macchina virtuale di Azure nell'attività successiva di questo esercizio:

    ```powershell
    Test-AzDnsAvailability -DomainNameLabel <custom-label> -Location '<location>'
    ```

    >**Nota**: sostituire il segnaposto `<custom-label>` con un nome DNS valido che sia univoco a livello globale. Sostituire il segnaposto `<location>` con il nome dell'area in cui si vuole distribuire la macchina virtuale di Azure che ospiterà il controller di dominio di Active Directory da usare in questo lab.

    >**Nota**: per identificare le aree di Azure in cui è possibile effettuare il provisioning di macchine virtuali di Azure, vedere [**https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/)

5. Verificare che il comando abbia restituito **True**. In caso contrario, eseguire di nuovo lo stesso comando con un valore diverso di `<custom-label>` finché non restituisce **True**.

6. Registrare il valore di `<custom-label>` che ha generato il risultato positivo. Sarà necessario nell'attività successiva.

7. Chiudere Cloud Shell.

#### Attività 2: Usare un modello di ARM per distribuire una macchina virtuale di Azure che ospita un controller di dominio di Active Directory

In questa attività si distribuirà una macchina virtuale di Azure che ospiterà un controller di dominio di Active Directory

1. Aprire un'altra scheda del browser nella stessa finestra e passare a **https://github.com/Azure/azure-quickstart-templates/tree/master/application-workloads/active-directory/active-directory-new-domain**.

2. Nella **pagina Crea una macchina virtuale di Azure con una nuova foresta** di Active Directory fare clic su **Distribuisci in Azure**. Il browser verrà reindirizzato automaticamente al pannello**Create an Azure VM with a new AD Forest** nel portale di Azure. 

3. Nel pannello **Create an Azure VM with a new AD Forest** fare clic su **Modifica parametri**.

4. Nel pannello **Modifica parametri** fare clic su **Carica file**, nella finestra di dialogo **Apri**passare alla cartella **\\\\AllFiles\Labs\\06\\active-directory-new-domain\\azuredeploy.parameters.json**, fare clic su **Apri** e quindi fare clic su **Salva**.

5. Nel pannello **Create an Azure VM with a new AD Forest** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Subscription|Nome della propria sottoscrizione di Azure|
   |Gruppo di risorse|Fare clic su **Crea nuovo** e digitare il nome **AZ500LAB06**|
   |Area|L'area di Azure identificata nell'attività precedente|
   |Nome utente amministratore|**Studente**|
   |Password amministratore|**Usare la password personale creata in Lab 04 > Esercizio 1 > Attività 1 > Passaggio 9.**|
   |Nome dominio|**adatum.com**|
   |Prefisso DNS|Il nome host DNS identificato nell'attività precedente|
   |Dimensioni macchina virtuale|**Standard_D2s_v3**|

6. Nel pannello **Create an Azure VM with a new AD Forest** fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: non attendere il completamento della distribuzione, ma procedere con l'esercizio successivo. La distribuzione può richiedere 15 minuti circa. La macchina virtuale distribuita in questa attività verrà usata nel terzo esercizio di questo lab.

> Risultato: completando questo esercizio, è stata avviata la distribuzione di una macchina virtuale di Azure che ospiterà un controller di dominio di Active Directory usando un modello di Azure Resource Manager


### Esercizio 2: Creare e configurare un tenant di Azure Active Directory 

### Tempo stimato: 20 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Creare un tenant di Azure Active Directory (AD)
- Attività 2: Aggiungere un nome DNS personalizzato al nuovo tenant di Azure AD
- Attività 3: Creare un utente di Azure AD con il ruolo di amministratore globale

#### Attività 1: Creare un tenant di Azure Active Directory (AD)

In questa attività si creerà un nuovo tenant di Azure AD da usare in questo lab. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Azure Active Directory** e premere **INVIO**.

2. Nel pannello **Panoramica** del tenant di Azure AD corrente fare clic su **Manage tenants** (Gestisci tenant) e quindi nella schermata successiva fare clic su **+ Crea**.

3. Nella scheda **Informazioni di base** del pannello **Crea un tenant** assicurarsi che sia selezionata l'opzione **Azure Active Directory** e fare clic su **Avanti: Configurazione >**.

4. Nella scheda **Configurazione** del pannello **Crea una directory** specificare le impostazioni seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome organizzazione|**AdatumSync**|
   |Nome di dominio iniziale|Un nome univoco costituito da una combinazione di lettere e numeri|
   |Paese o area geografica|**Stati Uniti**|

    >**Nota**: registrare il nome di dominio iniziale. Sarà necessario più avanti in questo lab.

    >**Nota**: il segno di spunta verde nella casella di testo **Nome di dominio iniziale** indicherà se il nome di dominio immesso è valido e univoco. Registrare il nome di dominio iniziale per usarlo in seguito.

5. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: attendere il completamento della creazione del tenant. Usare l'icona **Notifica** per monitorare lo stato della distribuzione. 

#### Attività 2: Aggiungere un nome DNS personalizzato al nuovo tenant di Azure AD

In questa attività si aggiungerà il nome DNS personalizzato al nuovo tenant di Azure AD. 

1. Sulla barra degli strumenti del portale di Azure fare clic sull'icona **Directory e sottoscrizione** a destra dell'icona Cloud Shell. 

2. Nel pannello **Directory e sottoscrizione** selezionare la riga del tenant appena creato **AdatumSync** e quindi fare clic sul pulsante**Cambia**.

    >**Nota**: se la voce **AdatumSync** non viene visualizzata nell'elenco di filtro **Directory e sottoscrizione**, può essere necessario aggiornare la finestra del browser.

3. Nel pannello **AdatumSync \| Azure Active Directory** fare clic su **Nomi di dominio personalizzati** nella sezione **Gestisci**.

4. Nel pannello **AdatumSync \| Nomi di dominio personalizzati** fare clic su **+ Aggiungi dominio personalizzato**.

5. Nella casella di testo **Nome di dominio personalizzato** del pannello **Nome dominio personalizzato** digitare **adatum.com** e fare clic su **Aggiungi dominio**.

6. Nel pannello **adatum.com** esaminare le informazioni necessarie per eseguire la verifica del nome di dominio di Azure AD e quindi selezionare **Elimina** due volte. 

    >**Nota**: non sarà possibile completare il processo di convalida perché non si è proprietari del nome di dominio DNS **adatum.com**. Questo non impedisce di sincronizzare il dominio di AD DS **adatum.com** con il tenant di Azure AD. A questo scopo si userà il nome DNS iniziale del tenant di Azure AD (il nome che termina con il suffisso **onmicrosoft.com**), identificato nell'attività precedente. Tenere tuttavia presente che, di conseguenza, il nome di dominio DNS del dominio di AD DS sarà diverso dal nome DNS del tenant di Azure AD. Gli utenti di Adatum dovranno quindi usare nomi diversi per accedere al dominio di AD DS e al tenant di Azure AD.

#### Attività 3: Creare un utente di Azure AD con il ruolo di amministratore globale

In questa attività si aggiungerà un nuovo utente di Azure AD che verrà assegnato al ruolo di amministratore globale. 

1. Nel pannello del tenant di Azure AD **AdatumSync** fare clic su **Utenti** nella sezione **Gestisci**.

2. **In Utenti | Pannello Tutti gli utenti**, fare clic su **+ Nuovo utente** e quindi su **Crea nuovo utente**.

3. **Nel pannello Nuovo utente verificare che l'opzione **Crea utente**** sia selezionata, specificare le impostazioni seguenti nella scheda Informazioni di base (lasciare tutti gli altri con i valori predefiniti) e fare clic su **Avanti: Proprietà >**:

   |Impostazione|Valore|
   |---|---|
   |Nome utente|**syncadmin**|
   |Nome|**syncadmin**|
   |Password|Assicurarsi che sia selezionata l'opzione **Password generata automaticamente** e fare clic su **Mostra password**|

    >**Nota**: registrare il nome utente completo. È possibile copiarne il valore facendo clic sul **pulsante Copia negli Appunti** sul lato destro dell'elenco a discesa che visualizza il nome di dominio e incollandolo in un documento del Blocco note. Sarà necessaria più avanti in questo lab.

    >**Nota**: registrare la password dell'utente facendo clic sul **pulsante Copia negli Appunti** sul lato destro della casella di testo Password e incollandolo in un documento del Blocco note. Sarà necessaria più avanti in questo lab.

4. Nella **scheda Proprietà** scorrere fino alla fine e specificare La posizione di utilizzo: **Stati Uniti** (lasciare tutti gli altri con i valori predefiniti) e fare clic su **Avanti: Assegnazioni >**.

5. Nella **scheda Assegnazioni** fare clic su **+ Aggiungi ruolo**, cercare e selezionare **Global Amministrazione istrator** e quindi fare clic su **Seleziona**. Fare clic su **Rivedi e crea** e quindi su **Crea**.
   
    >**Nota**: per implementare Azure AD Connect, è necessario un utente con il ruolo di amministratore globale.

6. Aprire una finestra del browser InPrivate.

7. Passare al portale di Azure all'indirizzo **`https://portal.azure.com/`** e accedere usando l'account **utente syncadmin**. Quando richiesto, modificare la password registrata in precedenza in questa attività con la propria password che soddisfi i requisiti di complessità e registrarla per riferimento futuro. Questa password verrà richiesta nelle attività successive.

    >**Nota**: per accedere è necessario specificare un nome completo dell'account utente **syncadmin**, incluso il nome di dominio DNS del tenant di Azure AD registrato in precedenza in questa attività. Questo nome utente è nel formato syncadmin@`<your_tenant_name>`.onmicrosoft.com, dove `<your_tenant_name>` è il segnaposto che rappresenta il nome univoco del tenant di Azure AD. 

8. Disconnettersi come **syncadmin** e chiudere la finestra del browser InPrivate.

> **Risultato**: con questo esercizio è stato creato un tenant di Azure AD, è stato visto come aggiungere un nome DNS personalizzato al nuovo tenant di Azure AD ed è stato creato un utente di Azure AD con il ruolo di amministratore globale.


### Esercizio 3: Sincronizzare la foresta di Active Directory con un tenant di Azure Active Directory

### Tempo stimato: 20 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Preparare AD DS per la sincronizzazione della directory
- Attività 2: Installare Azure AD Connect
- Attività 3: Verificare la sincronizzazione della directory

#### Attività 1: Preparare AD DS per la sincronizzazione della directory

In questa attività ci si connetterà alla macchina virtuale di Azure che esegue il controller di dominio di AD DS e si creerà un account di sincronizzazione della directory. 

   > Prima di iniziare questa attività, assicurarsi che la distribuzione del modello avviata nel primo esercizio di questo lab sia stata completata.

1. Nel portale di Azure impostare il filtro **Directory e sottoscrizione** sul tenant di Azure AD associato alla sottoscrizione di Azure in cui è stata distribuita la macchina virtuale di Azure nel primo esercizio di questo lab.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Macchine virtuali** e premere **INVIO**.

3. Nel pannello **Macchine virtuali** fare clic sulla voce **adVM**. 

4. Nel pannello **adVM** fare clic su **Connetti** e quindi su **RDP** nel menu a discesa. 

5. Nell'elenco a discesa Indirizzo** IP selezionare **Load Balancer public IP address (Indirizzo** IP pubblico del servizio di bilanciamento del **carico), quindi fare clic su **Download RDP File (Scarica file** RDP) e usarlo per connettersi alla **macchina virtuale di Azure adVM** tramite Desktop remoto. Quando viene chiesto di eseguire l'autenticazione, specificare le credenziali seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome utente|**Studente**|
   |Password|**Usare la password personale creata in Lab 04 > Esercizio 1 > Attività 1 > Passaggio 9.**|

    >**Nota**: attendere l'apertura della sessione Desktop remoto e il caricamento di **Server Manager**.  

    >**Nota**: i passaggi seguenti vengono eseguiti nella sessione Desktop remoto nella macchina virtuale di Azure **adVM**.

    >**Nota**: se l'indirizzo** IP pubblico del **servizio di bilanciamento del carico non è disponibile nell'elenco **a discesa Indirizzo** IP del pannello RDP, nel portale di Azure cercare **Indirizzi** IP pubblici selezionare **adPublicIP** e prendere nota del relativo indirizzo IP. Fare clic sul pulsante Start, digitare **MSTSC** e premere **INVIO** per avviare il client desktop remoto. Digitare l'indirizzo IP pubblico del servizio di bilanciamento del carico nella **casella di testo Computer:** e fare clic su **Connessione**.

6. In **Server Manager** fare clic su **Strumenti** e quindi su **Centro di amministrazione di Active Directory**.

7. In **Centro di amministrazione di Active Directory** fare clic su **adatum (locale)**, nel riquadro **Attività** sotto il nome di dominio **adatum (locale)** fare clic su **Nuovo** e quindi nel menu a cascata fare clic su **Unità organizzativa**.

8. Nella casella di testo **Nome** della finestra **Crea unità organizzativa** digitare **ToSync** e fare clic su **OK**.

9. Fare doppio clic sull'unità organizzativa ToSync** appena creata **in modo che il relativo contenuto venga visualizzato nel riquadro dei dettagli della console di Active Directory Amministrazione istrative Center. 

10. Nella sezione **ToSync** del riquadro **Attività** fare clic su **Nuovo** e scegliere **Utente** dal menu a cascata.

11. Nella finestra **Crea utente** creare un nuovo account utente con le impostazioni seguenti, lasciando i valori predefiniti per le altre, e fare clic su **OK**:
    
    |Impostazione|Valore|
    |---|---|
    |Nome completo|**aduser1**|
    |Accesso UPN utente|**aduser1**|
    |Accesso utente SamAccountName|**aduser1**|
    |Password e Conferma password|**Usare la password personale creata in Lab 04 > Esercizio 1 > Attività 1 > Passaggio 9.**|
    |Altre opzioni password|**Nessuna scadenza password**|


#### Attività 2: Installare Azure AD Connect

In questa attività si installerà AD Connect nella macchina virtuale. 

1. Nella sessione Desktop remoto per **adVM** usare Microsoft Edge per passare al portale di Azure all'indirizzo **https://portal.azure.com** e accedere usando l'account utente **syncadmin** creato nell'esercizio precedente. Quando richiesto, specificare il nome completo dell'entità utente e la password registrati nell'esercizio precedente.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Azure Active Directory** e premere **INVIO**.

3. Nel pannello Panoramica di AdatumSync del portale di Azure **fare clic su **Azure AD Connessione** nel pannello di spostamento a sinistra in **Gestisci**.\|**

4. **Nel pannello Attività iniziali** di AAD Connessione \| fare clic su **Connessione Sincronizzazione** nel pannello di spostamento sinistro e quindi sul collegamento Scarica azure **AD Connessione**. Si verrà reindirizzati alla **pagina di download di Azure AD Connessione**.

5. Nella **pagina di download di Azure AD Connessione** fare clic su **Scarica**.

6. Quando richiesto, fare clic su **Esegui** per avviare procedura guidata **Microsoft Azure Active Directory Connect**.

7. Nella pagina **Azure AD Connect** della procedura guidata **Microsoft Azure Active Directory Connect** fare clic sulla casella di controllo **Accetto le condizioni di licenza e l'informativa sulla privacy** e quindi su **Continua**.

8. Nella pagina **Impostazioni rapide** della procedura guidata **Microsoft Azure Active Directory Connect** fare clic sull'opzione **Personalizza**.

9. Nella pagina **Installazione dei componenti necessari** lasciare deselezionate tutte le opzioni di configurazione facoltative e fare clic su **Installa**.

10. Nella pagina **Accesso utente** assicurarsi che sia abilitata solo l'opzione **Sincronizzazione dell'hash delle password** e fare clic su **Avanti**.

11. Nella pagina **Connessione da Azure AD** eseguire l'autenticazione usando le credenziali dell'account utente **syncadmin** creato nell'esercizio precedente e fare clic su **Avanti**. 

12. Nella pagina **Connessione delle directory** fare clic sul pulsante **Aggiungi directory** a destra della voce relativa alla foresta **adatum.com**.

13. Nella finestra **Account della foresta Active** assicurarsi che sia selezionata l'opzione **Crea un nuovo account AD**, specificare le credenziali seguenti e fare clic su **OK**:

    |Impostazione|Valore|
    |---|---|
    |Nome utente|**ADATUM \\ Student**|
    |Password|**Usare la password personale creata in Lab 06 > Esercizio 1 > Attività 2**|

14. Nella pagina **Connessione delle directory** assicurarsi che la voce **adatum.com** sia visualizzata come directory configurata e fare clic su **Avanti**

15. Nella pagina **Configurazione dell'accesso ad Azure AD**, nel messaggio di avviso **Gli utenti non potranno accedere ad Azure AD con le credenziali locali se il suffisso UPN non corrisponde a un dominio verificato**, abilitare la casella di controllo **Continua senza corrispondenza di tutti i suffissi UPN con domini verificati**, quindi fare clic su **Avanti**.

    >**Nota**: come indicato prima, questo comportamento è previsto, perché non è stato possibile verificare il dominio DNS personalizzato di Azure AD **adatum.com**.

16. **Nella pagina Filtro dominio e unità organizzative** fare clic sull'opzione **Sincronizza domini e unità organizzative** selezionate e deselezionare la casella di controllo accanto al nome **di dominio adatum.com**. Fare clic per espandere **adatum.com**, selezionare solo la casella di controllo accanto all'unità **organizzativa ToSync** e quindi fare clic su **Avanti**.

17. Nella pagina **Identificazione univoca per gli utenti** accettare le impostazioni predefinite e fare clic su **Avanti**.

18. Nella pagina **Filtro utenti e dispositivi** accettare le impostazioni predefinite e fare clic su **Avanti**.

19. Nella pagina **Funzionalità facoltative** accettare le impostazioni predefinite e fare clic su **Avanti**.

20. Nella pagina **Pronto per la configurazione** assicurarsi che sia selezionata la casella di controllo **Avvia il processo di sincronizzazione al termine della configurazione** e fare clic su **Installa**.

    >**Nota**: l'installazione dovrebbe richiedere circa 2 minuti.

21. Esaminare le informazioni nella pagina **Configurazione completata** e fare clic su **Esci** per chiudere la finestra **Microsoft Azure Active Directory Connect**.


#### Attività 3: Verificare la sincronizzazione della directory

In questa attività si verificherà il funzionamento della sincronizzazione della directory. 

1. Nella sessione Desktop remoto per **adVM**, nella finestra di Microsoft Edge che visualizza il portale di Azure passare al pannello **Utenti - Tutti gli utenti (anteprima)** del tenant di Azure AD Adatum Lab.

2. Nel pannello **Utenti \| Tutti gli utenti** si noti che l'elenco di oggetti utente include l'account **aduser1**. 

   >**Nota**: potrebbe essere necessario attendere alcuni minuti e selezionare **Aggiorna** per visualizzare l'account utente **aduser1**.

3. Fare clic sull'account **aduser1** e selezionare la **scheda Proprietà** . Scorrere verso il basso fino alla **sezione Locale** , notare che l'attributo abilitato per la **sincronizzazione** locale è impostato su **Sì**.

4. **Nel pannello aduser1**, nella **sezione Informazioni** processo, si noti che l'attributo **Department** non è impostato.

5. All'interno della sessione di Desktop remoto in **adVM** passare ad **Active Directory Amministrazione istrative Center**, selezionare la **voce aduser1** nell'elenco di oggetti nell'unità **organizzativa ToSync** e, nel **riquadro Attività**, nella **sezione aduser1** selezionare **Proprietà**.

6. Nella sezione **Organizzazione** della finestra **aduser1** digitare **Sales** nella casella di testo **Reparto** e fare clic su **OK**.

7. Nella sessione Desktop remoto per **adVM**avviare **Windows PowerShell**.

8. Nella console **Amministratore: Windows PowerShell** eseguire quanto segue per avviare la sincronizzazione differenziale di Azure AD Connect:

    ```powershell
    Import-Module -Name 'C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1'

    Start-ADSyncSyncCycle -PolicyType Delta
    ```

9. Passare alla finestra di Microsoft Edge che visualizza il **pannello aduser1** , aggiornare la pagina e notare che la proprietà Department è impostata su Sales.

    >**Nota**: potrebbe essere necessario attendere fino a tre minuti e aggiornare di nuovo la pagina se l'attributo **Department** non è impostato.

> **Risultato**: completando questo esercizio, è stato preparato AD DS per la sincronizzazione della directory, è stato installato Azure AD Connect ed è stata verificata la sincronizzazione della directory.


**Pulire le risorse**

>**Nota**: per iniziare, disabilitare la sincronizzazione di Azure AD

1. Nella sessione Desktop remoto per **adVM**avviare Windows PowerShell come amministratore.

2. Nella console di Windows PowerShell installare il modulo MsOnline PowerShell eseguendo quanto segue (quando richiesto, nella finestra di dialogo Per continuare è necessario il provider NuGet digitare **Sì** e premere INVIO):

    ```powershell
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
    Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
    Install-Module MsOnline -Force
    ```

3. Nella console di Windows PowerShell connettersi al tenant di Azure AD AdatumSync eseguendo quanto segue (quando richiesto, accedere con le credenziali **syncadmin**):

    ```powershell
    Connect-MsolService
    ```

4. Nella console di Windows PowerShell, disabilitare la sincronizzazione di Azure AD Connect eseguendo quanto segue:

    ```powershell
    Set-MsolDirSyncEnabled -EnableDirSync $false -Force
    ```

5. Nella console di Windows PowerShell verificare che l'operazione sia stata completata correttamente eseguendo quanto segue:

    ```powershell
    (Get-MSOLCompanyInformation).DirectorySynchronizationEnabled
    ```
    >**Nota**: il risultato dovrà essere `False`. Se non è questo il caso, attendere un minuto ed eseguire di nuovo il comando.

    >**Nota**: successivamente, rimuovere le risorse di Azure
6. Chiudere la sessione Desktop remoto.

7. Nel portale di Azure impostare il filtro **Directory e sottoscrizione** sul tenant di Azure AD associato alla sottoscrizione di Azure in cui è stata distribuita la macchina virtuale di Azure **adVM**.

8. Nel portale di Azure aprire Cloud Shell facendo clic sulla prima icona nell'angolo in alto a destra. 

9. Nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell selezionare **PowerShell** e, quando richiesto, fare clic su **Conferma**. 

10. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il gruppo di risorse creato in questo lab:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB06" -Force -AsJob
    ```
11. Chiudere il riquadro **Cloud Shell**.

    >**Nota**: rimuovere infine il tenant di Azure AD
    
    >**Nota 2**: l'eliminazione di un tenant è un processo molto difficile, quindi non può mai essere eseguito in modo accidentale o doloso.  Questo significa che la rimozione del tenant come parte di questo lab non sempre funziona.  Anche se sono disponibili i passaggi per eliminare il tenant, questa operazione non è necessaria per considerare completato questo lab. Se è necessario rimuovere un tenant nella realtà operativa, su DOCS.Microsoft sono disponibili articoli che possono risultare utili.

12. Tornare nel portale di Azure e usare il filtro **Directory e sottoscrizione** per passare al tenant di Azure Active Directory **AdatumSync**.

13. Nel portale di Azure passare al pannello **Utenti - Tutti gli utenti**, fare clic sulla voce che rappresenta l'account utente **syncadmin**, quindi nel pannello **syncadmin - Profilo** fare clic su **Elimina** e, quando richiesto, fare clic su **Sì** per confermare.

14. Ripetere la stessa sequenza di passaggi per eliminare l'account utente **aduser1** e l'**account del servizio di sincronizzazione directory locale**.

15. Passare al pannello **AdatumSync - Panoramica** del tenant di Azure AD, fare clic su **Manage tenants** (Gestisci tenant) e selezionare la casella di controllo della directory **AdatumSync**, fare clic su **Elimina** nel pannello **Elimina tenant "AdatumSync"**, fare clic sul collegamento **Ottieni l'autorizzazione per eliminare le risorse di Azure**, nel pannello **Proprietà** di Azure Active Directory impostare **Gestione degli accessi per le risorse di Azure** su **Sì** e fare clic su **Salva**.

    >**Nota**: se durante l'eliminazione viene visualizzato un avviso simile a **Elimina tutti gli utenti**, procedere con l'eliminazione degli utenti creati oppure se l'avviso indica **Elimina applicazione LinkedIn**, fare clic sul messaggio e confermare l'eliminazione dell'applicazione LinkedIn. Per completare l'eliminazione del tenant è necessario rispondere a tutti gli avvisi.

16. Disconnettersi dal portale di Azure e accedere di nuovo. 

17. Tornare al pannello **Elimina tenant "AdatumSync"** e fare clic su **Elimina**.

> Per altre informazioni su questa attività, vedere [https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto)
