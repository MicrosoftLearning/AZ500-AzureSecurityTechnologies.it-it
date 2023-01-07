---
lab:
  title: '04 - MFA, accesso condizionale e AAD Identity Protection'
  module: Module 01 - Manage Identity and Access
---

# <a name="lab-04-mfa-conditional-access-and-aad-identity-protection"></a>Lab 04: MFA, accesso condizionale e AAD Identity Protection
# <a name="student-lab-manual"></a>Manuale del lab per gli studenti

## <a name="lab-scenario"></a>Scenario del lab

È stato chiesto di creare un modello di verifica delle funzionalità che ottimizzano l'autenticazione di Azure Active Directory (Azure AD). In particolare, sarà necessario valutare quanto segue:

- Autenticazione a più fattori di Azure AD
- Accesso condizionale di Azure AD
- Azure AD Identity Protection

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## <a name="lab-objectives"></a>Obiettivi del lab

In questo lab verranno completati gli esercizi seguenti:

- Esercizio 1: Distribuire una macchina virtuale di Azure usando un modello di Azure Resource Manager
- Esercizio 2: Implementare Azure MFA
- Esercizio 3: Implementare i criteri di accesso condizionale di Azure AD 
- Esercizio 4: Implementare Azure AD Identity Protection

## <a name="mfa---conditional-access---identity-protection-diagram"></a>Diagramma di MFA - Accesso condizionale - Identity Protection

![image](https://user-images.githubusercontent.com/91347931/157518628-8b4a9efe-0086-4ec0-825e-3d062748fa63.png)

## <a name="instructions"></a>Istruzioni

## <a name="lab-files"></a>File del lab:

- **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.json**
- **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.parameters.json** 

### <a name="exercise-1-deploy-an-azure-vm-by-using-an-azure-resource-manager-template"></a>Esercizio 1: Distribuire una macchina virtuale di Azure usando un modello di Azure Resource Manager

### <a name="estimated-timing-10-minutes"></a>Tempo stimato: 10 minuti

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Distribuire una macchina virtuale di Azure usando un modello di Azure Resource Manager.

#### <a name="task-1-deploy-an-azure-vm-by-using-an-azure-resource-manager-template"></a>Attività 1: Distribuire una macchina virtuale di Azure usando un modello di Azure Resource Manager

In questa attività si creerà una macchina virtuale usando un modello di ARM. La macchina virtuale verrà usata nell'ultimo esercizio per questo lab. 

1. Accedere al portale di Azure **`https://portal.azure.com/`** .

    >**Nota**: accedere al portale di Azure usando un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per questo lab e il ruolo Amministratore globale nel tenant di Azure AD associato alla sottoscrizione.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Distribuire un modello personalizzato**.

    >**Nota**: è anche possibile selezionare **Distribuzione modelli (distribuzione tramite modelli personalizzati)** nell'elenco **Marketplace**.

3. Nel pannello **Distribuzione personalizzata** fare clic sull'opzione **Creare un modello personalizzato nell'editor**.

4. Nel pannello **Modifica modello** fare clic su **Carica file**, individuare il file **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.json** e fare clic su **Apri**.

    >**Nota**: esaminare il contenuto del modello e notare che distribuisce una macchina virtuale di Azure che ospita Windows Server 2019 Datacenter.

5. Nel pannello **Modifica modello** fare clic su **Salva**.

6. Tornare nel pannello **Distribuzione personalizzata** e fare clic su **Modifica parametri**.

7. Nel pannello **Modifica parametri** fare clic su **Carica file**, individuare il file **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.json** e fare clic su **Apri**.

    >**Nota**: esaminare il contenuto del file dei parametri annotando i valori di adminUsername e adminPassword.

8. Nel pannello **Modifica parametri** fare clic su **Salva**.

9. Nel pannello **Distribuzione personalizzata** assicurarsi che siano configurate le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

>**Nota**: è necessario creare una password univoca che verrà usata per la creazione delle macchine virtuali per il resto del corso. La password deve avere una lunghezza di almeno 12 caratteri e soddisfare i requisiti di complessità definiti (la password deve includere tre dei seguenti elementi: un carattere minuscolo, un carattere maiuscolo, un numero e un carattere speciale). [Requisiti delle password delle macchine virtuali](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm-). Prendere nota della password.

   |Impostazione|Valore|
   |---|---|
   |Subscription|Nome della sottoscrizione di Azure che verrà usata nel lab|
   |Resource group|Fare clic su **Crea nuovo** e digitare il nome **AZ500LAB04**|
   |Location|**Stati Uniti orientali**|
   |Dimensioni macchina virtuale|**Standard_D2s_v3**|
   |Nome VM|**az500-04-vm1**|
   |Nome utente amministratore|**Studente**|
   |Password amministratore|**Creare la password e prenderne nota per riferimento futuro. Questa password verrà chiesta per l'accesso al lab necessario.**|
   |Nome della rete virtuale|**az500-04-vnet1**|

    >**Note**: To identify Azure regions where you can provision Azure VMs, refer to [**https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/)

10. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: non attendere il completamento della distribuzione, ma procedere con l'esercizio successivo. La macchina virtuale inclusa in questa distribuzione verrà usata nell'ultimo esercizio di questo lab.

> Risultato: è stata avviata la distribuzione di un modello di una VM di Azure **az500-04-vm1** che verrà usata nell'ultimo esercizio di questo lab.


### <a name="exercise-2-implement-azure-mfa"></a>Esercizio 2: Implementare Azure MFA

### <a name="estimated-timing-30-minutes"></a>Tempo stimato: 30 minuti

In questo esercizio verranno eseguite le attività seguenti

- Attività 1: Creare un nuovo tenant di Azure AD.
- Attività 2: Attivare la versione di valutazione di Azure AD Premium P2.
- Attività 3: Creare utenti e gruppi di Azure AD.
- Attività 4: Assegnare licenze di Azure AD Premium P2 agli utenti di Azure AD.
- Attività 5: Configurare le impostazioni di Azure MFA.
- Attività 6: Convalidare la configurazione di MFA

#### <a name="task-1-create-a-new-azure-ad-tenant"></a>Attività 1: Creare un nuovo tenant di Azure AD

In questa attività si creerà un nuovo tenant di Azure AD. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Azure Active Directory** e premere **INVIO**.

2. Nel pannello **Panoramica** del tenant di Azure AD corrente fare clic su **Manage tenants** (Gestisci tenant) e quindi nella schermata successiva fare clic su **+ Crea**.

3. Nella scheda **Informazioni di base** del pannello **Crea un tenant** assicurarsi che sia selezionata l'opzione **Azure Active Directory** e fare clic su **Avanti: Configurazione >** .

4. Nella scheda **Configurazione** del pannello **Crea un tenant** specificare le impostazioni seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome organizzazione|**AdatumLab500-04**|
   |Nome di dominio iniziale|Un nome univoco costituito da una combinazione di lettere e numeri|
   |Paese o area geografica|**Stati Uniti**|

    >**Nota**: registrare il nome di dominio iniziale. Sarà necessario più avanti in questo lab.

5. Fare clic su **Rivedi e crea** e quindi su **Crea**.
6. Aggiungere il codice Captcha nel pannello **Help us prove you're not a robot** (Non sono un robot) e quindi fare clic su pulsante **Invia**. 

    >**Nota**: attendere il completamento della creazione del tenant. Usare l'icona **Notifica** per monitorare lo stato della distribuzione. 


#### <a name="task-2-activate-azure-ad-premium-p2-trial"></a>Attività 2: Attivare la versione di valutazione di Azure AD Premium P2

In questa attività ci si iscriverà per ricevere la versione di valutazione gratuita di Azure AD Premium P2. 

1. Sulla barra degli strumenti del portale di Azure fare clic sull'icona **Directory e sottoscrizione** a destra dell'icona Cloud Shell. 

2. Nel pannello **Directory e sottoscrizione** fare clic sul tenant appena creato **AdatumLab500-04** e quindi fare clic sul pulsante **Cambia** per impostarlo come directory corrente.

    >**Nota**: se la voce **AdatumLab500-04** non viene visualizzata nell'elenco di filtro **Directory e sottoscrizione**, può essere necessario aggiornare la finestra del browser.

3. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Azure Active Directory** e premere **INVIO**. Nel pannello **AdatumLab500-04** fare clic su**Licenze** nella sezione **Gestisci**.

4. Nel pannello **License \| Panoramica** fare clic su **Tutti i prodotti** nella sezione **Gestisci** e quindi su **+ Prova/Acquista**.

5. Nella sezione Azure AD Premium P2 del pannello **Attiva** fare clic su **Versione di valutazione gratuita** e quindi su **Attiva**.


#### <a name="task-3-create-azure-ad-users-and-groups"></a>Attività 3: Creare utenti e gruppi di Azure AD.

In questa attività si creeranno tre utenti: aaduser1 (amministratore globale), aaduser2 (utente) e aaduser3 (utente). Per le attività successive saranno necessari il nome entità e la password di ogni utente. 

1. Tornare nel pannello di Azure Active Directory **AdatumLab500-04** e fare clic su **Utenti** nella sezione **Gestisci**.

2. Nel pannello **Utenti \| Tutti gli utenti** fare clic su **+ Nuovo utente**. 

3. Nel pannello **Nuovo utente** assicurarsi che sia selezionata l'opzione **Crea utente** e specificare le impostazioni seguenti, lasciando i valori predefiniti per le altre, quindi fare clic su **Crea**:

   |Impostazione|Valore|
   |---|---|
   |Nome utente|**aaduser1**|
   |Nome|**aaduser1**|
   |Password|Assicurarsi che sia selezionata l'opzione **Password generata automaticamente** e fare clic su **Mostra password**|
   |Gruppi|**0 gruppi selezionati**|
   |Ruoli|Fare clic su **Utente**, quindi su **Amministratore globale** e infine su **Seleziona**|
   |Località di utilizzo|**Stati Uniti**|  

    >**Nota**: registrare il nome utente completo. È possibile copiarne il valore facendo clic sul pulsante **Copia negli Appunti** a destra dell'elenco a discesa che visualizza il nome di dominio. 

    >**Nota**: registrare la password dell'utente. Sarà necessaria più avanti in questo lab. 

4. Nel pannello **Utenti \| Tutti gli utenti** fare clic su **+ Nuovo utente**. 

5. Nel pannello **Nuovo utente** assicurarsi che sia selezionata l'opzione **Crea utente** e specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Nome utente|**aaduser2**|
   |Nome|**aaduser2**|
   |Password|Assicurarsi che sia selezionata l'opzione **Password generata automaticamente** e fare clic su **Mostra password**|
   |Gruppi|**0 gruppi selezionati**|
   |Ruoli|**Utente**|
   |Località di utilizzo|**Stati Uniti**|  

    >**Nota**: registrare il nome utente completo e la password.

6. Nel pannello **Utenti \| Tutti gli utenti** fare clic su **+ Nuovo utente**. 

7. Fare clic su **Nuovo utente**, completare impostazioni di configurazione del nuovo utente e quindi fare clic su **Crea**.

   |Impostazione|Valore|
   |---|---|
   |Nome utente|**aaduser3**|
   |Nome|**aaduser3**|
   |Password|Assicurarsi che sia selezionata l'opzione **Password generata automaticamente** e fare clic su **Mostra password**|
   |Gruppi|**0 gruppi selezionati**|
   |Ruoli|**Utente**|
   |Località di utilizzo|**Stati Uniti**|  

    >**Nota**: registrare il nome utente completo e la password.

    >**Nota**: a questo punto, nella pagina **Utenti** dovrebbero essere elencati i tre nuovi utenti. 
    
#### <a name="task-4-assign-azure-ad-premium-p2-licenses-to-azure-ad-users"></a>Attività 4: Assegnare licenze di Azure AD Premium P2 agli utenti di Azure AD

In questa attività si assegnerà ogni utente alla licenza di Azure Active Directory Premium P2.

1. Nel pannello **Utenti \| Tutti gli utenti** fare clic sulla voce che rappresenta l'account utente. 

2. Nel pannello che visualizza le proprietà degli account utente fare clic su **Modifica proprietà**.  Verificare che l'opzione Località di utilizzo sia impostata su **Stati Uniti** e in caso contrario impostarla e fare clic su **Salva**.

3. Tornare nel pannello di Azure Active Directory **AdatumLab500-04** e fare clic su **Licenze** nella sezione **Gestisci**.

4. Nel pannello **Licenze \| Panoramica** fare clic su **Tutti i prodotti**, selezionare la casella di controllo **Azure Active Directory Premium P2** e fare clic su **+ Assegna**.

5. Nel pannello **Assegna licenza** fare clic su **+ Aggiungi utenti e gruppi**.

6. Nel pannello **Utenti** selezionare **aaduser1**, **aaduser2**, **aaduser3** e il proprio account utente e fare clic su **Seleziona**.

7. Nel pannello **Assegnazione licenze** fare clic su **Opzioni di assegnazione**, assicurarsi che tutte le opzioni siano abilitate, fare clic su **Verifica e assegna** e quindi su **Assegna**.

8. Disconnettersi dal portale di Azure accedere di nuovo usando lo stesso account. Questo passaggio è necessario per rendere effettiva l'assegnazione della licenza.

    >**Nota**: a questo punto, sono state assegnate le licenze di Azure Active Directory Premium P2 a tutti gli account utente che verranno usati in questo lab. Assicurarsi di disconnettersi e quindi accedere di nuovo. 

#### <a name="task-5-configure-azure-mfa-settings"></a>Attività 5: Configurare le impostazioni di Azure MFA.

In questa attività si configurerà MFA, che verrà abilitata per aaduser1. 

1. Nel portale di Azure tornare nel pannello del tenant di Azure Active Directory **AdatumLab500-04**.

    >**Nota**: assicurarsi di usare il tenant di Azure AD AdatumLab500-04.

2. Tornare nel pannello di Azure Active Directory **AdatumLab500-04** e fare clic su **Sicurezza** nella sezione **Gestisci**.

3. Nel pannello **Sicurezza \| Attività iniziali**, nella sezione **Gestisci** fare clic su **Autenticazione a più fattori**.

4. Nel pannello **Autenticazione a più fattori \| Attività iniziali** fare clic sul collegamento **Altre impostazioni di autenticazione a più fattori basate sul cloud**. 

    >**Nota**: si aprirà una nuova scheda del browser che visualizza la pagina **Autenticazione a più fattori**.

5. Nella pagina **Autenticazione a più fattori** fare clic sulla scheda **Impostazioni servizio**. Esaminare le **opzioni di verifica**. Si noti che sono abilitate le opzioni **SMS al telefono**, **Notifica tramite l'app per dispositivi mobili** e **Codice di verifica dall'app per dispositivi mobili o dal token hardware**. Fare clic su **Salva** e quindi su **Chiudi**.

6. Passare alla scheda **Utenti**, fare clic sulla voce **aaduser1**, fare clic sul collegamento **Abilita** e, quando richiesto, fare clic su **Abilita Multi-Factor Auth**.

7. Si noti che la colonna **Stato di Multi-Factor Auth** per **aaduser1** indica ora **Abilitato**.

8. Fare clic su **aaduser1** e notare che a questo punto è disponibile anche l'opzione **Applica**. 

    >**Nota**: la modifica dello stato utente da Abilitato a Applicato influisce solo sulle app Azure AD integrate legacy che non supportano Azure MFA e, quando lo stato cambia in Applicato, è necessario l'uso delle password dell'app.

9. Con la voce **aaduser1** selezionata, fare clic su **Gestisci impostazioni utente** ed esaminare le opzioni disponibili: 

   - Richiedi agli utenti selezionati di fornire nuovamente i metodi di contatto.

   - Elimina tutte le password di app generate dagli utenti selezionati.

   - Ripristina l'autenticazione a più fattori in tutti i dispositivi memorizzati.

10. Fare clic su **Annulla** e tornare nella scheda del browser che visualizza il pannello **Multi-Factor Authentication \| Attività iniziali** nel portale di Azure.

11. Nella sezione **Impostazioni** fare clic su **Avviso di illecito**.

12. Nel pannello **Multi-Factor Authentication \| Avviso di illecito** configurare le impostazioni seguenti:

   |Impostazione|Valore|
   |---|---|
   |Consenti agli utenti di inviare avvisi di illeciti|**Sì**|
   |Blocca automaticamente gli utenti per cui è stato segnalato un illecito|**Sì**|
   |Codice per segnalare illeciti durante il messaggio introduttivo iniziale|**0**|

13. Fare clic su **Save** (Salva).

    >**Nota**: a questo punto è stata abilitata la funzionalità MFA per aaduser1 e sono state configurate le impostazioni di avviso per gli illeciti. 

14. Tornare nel pannello del tenant di Azure Active Directory **AdatumLab500-04**, fare clic su **Proprietà** nella sezione **Gestisci**, fare clic sul collegamento **Gestisci le impostazioni predefinite per la sicurezza** nella parte inferiore del pannello e quindi, nel pannello **Abilita le impostazioni predefinite per la sicurezza**, fare clic su **No**. Selezionare **L'organizzazione dell'utente usa l'accesso condizionale** come motivo e quindi fare clic su **Salva**.

    >**Nota**: assicurarsi di aver eseguito l'accesso al tenant di Azure AD **AdatumLab500-04**. È possibile usare il filtro **Directory e sottoscrizione** per passare da un tenant di Azure AD a un altro. Assicurarsi di aver eseguito l'accesso come utente con il ruolo di amministratore globale nel tenant di Azure AD.

#### <a name="task-6-validate-mfa-configuration"></a>Attività 6: Convalidare la configurazione di MFA

In questa attività si convaliderà la configurazione di MFA testando l'accesso dell'account utente aaduser1. 

1. Aprire una finestra del browser InPrivate.

2. Passare al portale di Azure e accedere usando l'account utente **aaduser1**. 

    >**Nota**: per accedere è necessario specificare un nome completo dell'account utente **aaduser1**, incluso il nome di dominio DNS del tenant di Azure AD registrato in precedenza in questo lab. Questo nome utente è nel formato aaduser1@`<your_tenant_name>`.onmicrosoft.com, dove `<your_tenant_name>` è il segnaposto che rappresenta il nome univoco del tenant di Azure AD. 

3. Quando richiesto, nella finestra di dialogo **Sono necessarie altre informazioni** selezionare **Avanti**.

    >**Nota**: la sessione del browser verrà reindirizzata alla pagina **Verifica di sicurezza aggiuntiva**.

4. Nella pagina **Proteggi l'account** selezionare il collegamento **Si vuole configurare un metodo diverso**, selezionare **Telefono** nell'elenco a discesa **Specificare il metodo da usare**, quindi selezionare **Conferma**.

5. Nella pagina **Proteggi l'account** selezionare il paese o l'area geografica, digitare il proprio numero di cellulare nell'area **Immettere il numero di telefono**, assicurarsi che sia selezionata l'opzione **Invia un SMS** e fare clic su **Avanti**.
 
6. Nella pagina **Proteggi l'account** digitare il codice ricevuto nell'SMS sul telefono cellulare e fare clic su **Avanti**.

7. Nella pagina **Proteggi l'account** assicurarsi che la verifica sia riuscita e fare clic su **Avanti**.

8. Nella pagina **Proteggi l'account** fare clic su **Si vuole configurare un metodo diverso**, selezionare **Posta elettronica** nell'elenco a discesa, fare clic su **Conferma**, specificare l'indirizzo di posta elettronica da usare e fare clic su **Avanti**. Identificare il codice nel corpo del messaggio di posta elettronica corrispondente ricevuto e quindi fare clic su **Fine**.

9. Quando richiesto, modificare la password. Assicurarsi di registrare la nuova password.

10. Verificare di aver eseguito correttamente l'accesso al portale di Azure.

11. Disconnettersi come **aaduser1** e chiudere la finestra del browser InPrivate.

> Risultato: è stato creato un nuovo tenant di AD, sono stati configurati gli utenti di AD, è stata configurata la funzionalità MFA ed è stata testata l'esperienza di MFA per un utente. 


### <a name="exercise-3-implement-azure-ad-conditional-access-policies"></a>Esercizio 3: Implementare i criteri di accesso condizionale di Azure AD 

### <a name="estimated-timing-15-minutes"></a>Tempo stimato: 15 minuti

In questo esercizio verranno eseguite le attività seguenti 

- Attività 1: Configurare un criterio di accesso condizionale.
- Attività 2: Testare il criterio di accesso condizionale.

#### <a name="task-1---configure-a-conditional-access-policy"></a>Attività 1 - Configurare un criterio di accesso condizionale. 

In questa attività si esamineranno le impostazioni dei criteri di accesso condizionale e si creerà un criterio che richiede MFA per l'accesso al portale di Azure. 

1. Nel portale di Azure tornare nel pannello del tenant di Azure Active Directory **AdatumLab500-04**.

2. Nel pannello **AdatumLab500-04** fare clic su**Sicurezza** nella sezione **Gestisci**.

3. Nella sezione **Sicurezza \| Attività iniziali** fare clic su **Accesso condizionale** nella sezione **Proteggi**.

4. Nel pannello **Accesso condizionale \| Criteri** fare clic su **+ Nuovi criteri** e selezionare **Crea nuovo criterio** nell'elenco a discesa. 

5. Nel pannello **Nuovo** configurare le impostazioni seguenti:

   - Nella casella di testo **Nome** digitare **AZ500Policy1**
    
   - Fare clic su **Users or workload identities selected** (Utenti o identità carico di lavoro selezionati). In A cosa si applica questo criterio? >> Utenti e gruppi >> Includi >> abilitare **Seleziona utenti e gruppi** >> selezionare la casella di controllo **Utenti e gruppi**, quindi nel pannello **Seleziona** fare clic su **aaduser2** e quindi fare clic su **Seleziona**.
    
   - Fare clic su **Applicazioni cloud o azioni**, fare clic su **Selezionare le app**, fare clic su **Gestione di Microsoft Azure** nel pannello **Seleziona** e fare clic su **Seleziona**. 

    >**Nota**: leggere l'avviso che indica che questo criterio influisce sull'accesso al portale di Azure.
    
   - Fare clic su **Condizioni**, fare clic su **Rischio di accesso**, esaminare i livelli di rischio nel pannello **Rischio di accesso** ma non apportare modifiche e chiudere il pannello.
    
   - Fare clic su **Piattaforme del dispositivo**, esaminare le piattaforme del dispositivo che è possibile includere e fare clic su **Fine**.
    
   - Fare clic su **Località** ed esaminare le opzioni di località senza apportare modifiche.
    
   - Fare clic su **Concedi** nella sezione **Controlli di accesso**, nel pannello **Concedi** selezionare la casella di controllo **Richiedi autenticazione a più fattori** e fare clic su **Seleziona**
    
   - Impostare **Abilita criterio** su **Sì**.

6. Nel pannello **Nuovo** fare clic su **Crea**. 

    >**Nota**: a questo punto sono presenti criteri di accesso condizionale che richiedono MFA per l'accesso al portale di Azure. 

#### <a name="task-2---test-the-conditional-access-policy"></a>Attività 2: Testare il criterio di accesso condizionale.

In questa attività si accederà al portale di Azure come **aaduser2** e si verificherà che sia necessaria l'autenticazione MFA. Si eliminerà anche il criterio prima di proseguire con l'esercizio successivo. 

1. Aprire una finestra di Microsoft Edge InPrivate.

2. Nella nuova finestra del browser passare al portale di Azure e accedere con l'account utente **aaduser2**.

3. Quando richiesto, nella finestra di dialogo **Sono necessarie altre informazioni** selezionare **Avanti**.

    >**Nota**: la sessione del browser verrà reindirizzata alla pagina **Proteggi l'account**.
    
4. Nella pagina **Proteggi l'account** selezionare il collegamento **Si vuole configurare un metodo diverso**, selezionare **Telefono** nell'elenco a discesa **Specificare il metodo da usare**, quindi selezionare **Conferma**.

5. Nella pagina **Proteggi l'account** selezionare il paese o l'area geografica, digitare il proprio numero di cellulare nell'area **Immettere il numero di telefono**, assicurarsi che sia selezionata l'opzione **Invia un SMS** e fare clic su **Avanti**.

6. Nella pagina **Proteggi l'account** digitare il codice ricevuto nell'SMS sul telefono cellulare e fare clic su **Avanti**.

7. Nella pagina **Proteggi l'account** assicurarsi che la verifica sia riuscita e fare clic su **Avanti**.

8. Nella pagina **Proteggi l'account** fare clic su **Fine**.

9. Quando richiesto, modificare la password. Assicurarsi di registrare la nuova password.

10. Verificare di aver eseguito correttamente l'accesso al portale di Azure.

11. Disconnettersi come **aaduser2** e chiudere la finestra del browser InPrivate.

    >**Nota**:è stato verificato che il criterio di accesso condizionale appena creato impone MFA quando l'utente aaduser2 accede al portale di Azure.

12. Nella finestra del browser che visualizza il portale di Azure tornare nel pannello del tenant di Azure Active Directory **AdatumLab500-04**.

13. Nel pannello **AdatumLab500-04** fare clic su**Sicurezza** nella sezione **Gestisci**.

14. Nella sezione **Sicurezza \| Attività iniziali** fare clic su **Accesso condizionale** nella sezione **Proteggi**.

15. Nel pannello **Accesso condizionale \| Criteri** fare clic sui puntini di sospensione accanto a **AZ500Policy1**, fare clic su **Elimina** e, quando richiesto, fare clic su **Sì** per confermare.

    >**Nota**: in questo esercizio si implementa un criterio di accesso condizionale per richiedere l'autenticazione MFA quando un utente accede al portale di Azure. 

>Risultato: è stato configurato e testato l'accesso condizionale di Azure AD.

### <a name="exercise-4-implement-azure-ad-identity-protection"></a>Esercizio 4: Implementare Azure AD Identity Protection

### <a name="estimated-timing-30-minutes"></a>Tempo stimato: 30 minuti

In questo esercizio verranno eseguite le attività seguenti 

- Attività 1: Visualizzare le opzioni di Azure AD Identity Protection nel portale di Azure
- Attività 2: Configurare un criterio di rischio utente
- Attività 3: Configurare un criterio di rischio di accesso
- Attività 4: Simulare eventi di rischio rispetto ai criteri di Azure AD Identity Protection 
- Attività 5: Esaminare i report di Azure AD Identity Protection

#### <a name="task-1-enable-azure-ad-identity-protection"></a>Attività1: Abilitare Azure AD Identity Protection

In questa attività si visualizzeranno le opzioni di Azure AD Identity Protection nel portale di Azure. 

1. Se necessario, accedere al portale di Azure **`https://portal.azure.com/`** .

    >**Nota**: assicurarsi di aver eseguito l'accesso al tenant di Azure AD **AdatumLab500-04**. È possibile usare il filtro **Directory e sottoscrizione** per passare da un tenant di Azure AD a un altro. Assicurarsi di aver eseguito l'accesso come utente con il ruolo di amministratore globale nel tenant di Azure AD.

2. Nel pannello **AdatumLab500-04** fare clic su**Sicurezza** nella sezione **Gestisci**.

3. Nella sezione **Sicurezza \| Attività iniziali** fare clic su **Identity Protection** nella sezione **Proteggi**.

4. Nel pannello **Identity Protection \| Panoramica** esaminare i grafici **Nuovi utenti a rischio rilevati** e **Nuovi accessi a rischio rilevati** e le altre informazioni sugli utenti a rischio. 

#### <a name="task-2-configure-a-user-risk-policy"></a>Attività 2: Configurare un criterio di rischio utente

In questa attività si creerà un criterio di rischio utente. 

1. Nel pannello **Identity Protection \| Panoramica** fare clic su **Criteri di rischio utente** nella sezione **Proteggi**

2. Configurare **Criteri di correzione del rischio utente** con le impostazioni seguenti: 

   - Fare clic su **Utenti**. Nella scheda **Includi** del pannello **Utenti** assicurarsi che sia selezionata l'opzione **Tutti gli utenti**.

   - Nel pannello **Utenti** passare alla scheda **Escludi**, fare clic su **Selezionare gli utenti esclusi**, selezionare il proprio account utente e quindi fare clic su **Seleziona**. 

   - Fare clic su **Rischio utente**. Nel pannello **Rischio utente** selezionare **Basso e superiore** e quindi fare clic su **Fine**. 

   - Fare clic su **Accesso**. Nel pannello **Accesso** assicurarsi che siano selezionate l'opzione **Consenti l'accesso** e la casella di controllo **Richiedi modifica password**, quindi fare clic su **Fine**.

   - Impostare **Imponi criteri** su **Sì** e fare clic su **Salva**.

#### <a name="task-3-configure-sign-in-risk-policy"></a>Attività 3: Configurare un criterio di rischio di accesso

In questa attività si configurerà un criterio di rischio di accesso. 

1. Nel pannello **Identity Protection \| Criteri di rischio utente** fare clic su **Criteri di rischio di accesso** nella sezione **Proteggi**

2. Configurare **Criteri di correzione del rischio di accesso** con le impostazioni seguenti: 

   - Fare clic su **Utenti**. Nella scheda **Includi** del pannello **Utenti** assicurarsi che sia selezionata l'opzione **Tutti gli utenti**.

   - Fare clic su **Rischio di accesso**. Nel pannello **Rischio di accesso** selezionare **Medio e superiore**, quindi fare clic su **Fine**. 

   - Fare clic su **Accesso**. Nel pannello **Accesso** assicurarsi che siano selezionate l'opzione **Consenti l'accesso** e la casella di controllo **Richiedi autenticazione a più fattori**, quindi fare clic su **Fine**.

   - Impostare **Imponi criteri** su **Sì** e fare clic su **Salva**.

#### <a name="task-4-simulate-risk-events-against-the-azure-ad-identity-protection-policies"></a>Attività 4: Simulare eventi di rischio rispetto ai criteri di Azure AD Identity Protection 

> Prima di iniziare questa attività, assicurarsi che la distribuzione del modello avviata nell'Esercizio 1 sia stata completata. La distribuzione include una macchina virtuale di Azure denominata **az500-04-vm1**. 

1. Nel portale di Azure impostare il filtro **Directory e sottoscrizione** sul tenant di Azure AD associato alla sottoscrizione di Azure in cui è stata distribuita la macchina virtuale di Azure **az500-04-vm1**.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Macchine virtuali** e premere **INVIO**.

3. Nel pannello **Macchine virtuali** fare clic sulla voce **az500-04-vm1**. 

4. Nel pannello **az500-04-vm1** fare clic su **Connetti** e quindi su **RDP** nel menu a discesa. 

5. Fare clic su **Scarica file RDP** e usare il file per connettersi alla macchina virtuale di Azure **az500-04-vm1** tramite Desktop remoto. Quando viene chiesto di eseguire l'autenticazione, specificare le credenziali seguenti:

   |Impostazione|Valore|
   |---|---|
   |Nome utente|**Studente**|
   |Password|**Usare la password personale creata in Lab 04 > Esercizio 1 > Attività 1 > Passaggio 9.**|

    >**Nota**: attendere l'apertura della sessione Desktop remoto e il caricamento di **Server Manager**.  

    >**Nota**: i passaggi seguenti vengono eseguiti nella sessione Desktop remoto nella macchina virtuale di Azure **az500-04-vm1**. 

6. In **Server Manager** fare clic su **Server locale** e quindi su **Configurazione sicurezza avanzata IE**.

7. Nella finestra di dialogo **Protezione avanzata di Internet Explorer** impostare entrambe le opzioni su **No** e fare clic su **OK**.

8. Avviare **Internet Explorer**, fare clic sull'icona a forma di ingranaggio sulla barra degli strumenti, scegliere **Sicurezza** dal menu a discesa e quindi fare clic su **InPrivate Browsing**.

9. Nella finestra InPrivate di Internet Explorer passare a ToR Browser Project all'indirizzo  **https://www.torproject.org/projects/torbrowser.html.en** .

10. Scaricare e installare la versione del browser ToR per Windows con le impostazioni predefinite. 

11. Al termine dell'installazione, avviare il browser ToR, usare l'opzione **Connect** nella pagina iniziale e passare ad Application Access Panel all'indirizzo  **https://myapps.microsoft.com** .

12. Quando richiesto, provare ad accedere con l'account **aaduser3**. 

    >**Nota**: verrà visualizzato il messaggio **L'accesso è stato bloccato**. Questo comportamento è previsto, perché questo account non è configurato con l'autenticazione a più fattori, che è richiesta a causa dell'aumento del rischio di accesso associato all'uso del browser ToR.

13. Usare l'opzione**Disconnetti e accedi con un altro account** o selezionare la freccia indietro per accedere con l'account **aaduser1** creato e configurato per l'autenticazione a più fattori in precedenza in questo lab.

    >**Nota**: questa volta verrà visualizzato il messaggio **È stata rilevata un'attività sospetta**. Anche in questo caso, questo comportamento è previsto, perché questo account è configurato con l'autenticazione a più fattori. Considerando l'aumento del rischio di accesso associato all'uso del browser ToR, sarà necessario usare l'autenticazione a più fattori.

14. Usare l'opzione **Verifica** e specificare se si vuole verificare la propria identità tramite SMS o con una chiamata.

15. Completare la verifica e assicurarsi di aver eseguito correttamente l'accesso ad Application Access Panel.

16. Chiudere la sessione RDP. 

    >**Nota**: a questo punto sono stati effettuati due diversi tentativi di accesso. Verranno quindi esaminati i report di Azure Identity Protection.

#### <a name="task-5-review-the-azure-ad-identity-protection-reports"></a>Attività 5: Esaminare i report di Azure AD Identity Protection

In questa attività si esamineranno i report di Azure AD Identity Protection generati dagli accessi al browser ToR.

1. Tornare nel portale di Azure e usare il filtro **Directory e sottoscrizione** per passare al tenant di Azure Active Directory **AdatumLab500-04**.

2. Nel pannello **AdatumLab500-04** fare clic su**Sicurezza** nella sezione **Gestisci**.

3. Nel pannello **Sicurezza \| Attività iniziali** fare clic su **Utenti a rischio** nella sezione **Report**. 

4. Esaminare il report e identificare eventuali voci che fanno riferimento all'account utente **aaduser3**.

5. Nel pannello **Sicurezza \| Attività iniziali** fare clic su **Accessi a rischio** nella sezione **Report**. 

6. Esaminare il report e identificare eventuali voci che corrispondono all'accesso con l'account utente **aaduser3**.

7. In **Report** fare clic su **Rilevamenti dei rischi**.

8. Esaminare il report e identificare eventuali voci che rappresentano l'accesso da un indirizzo IP anonimo generato dal browser ToR. 

 >**Nota**:possono essere necessari 10-15 minuti per visualizzare i rischi nei report.

> **Risultato**: è stata abilitata la funzionalità Azure AD Identity Protection, sono stati configurati i criteri di rischio utente e i criteri di rischio di accesso, quindi è stata convalidata la configurazione di Azure AD Identity Protection simulando eventi di rischio.

**Pulire le risorse**

> È necessario rimuovere le risorse di Identity Protection che non vengono più usate. 

Usare la procedura seguente per disabilitare i criteri di Identity Protection nel tenant di Azure AD **AdatumLab500-04**.

1. Nel portale di Azure tornare nel pannello del tenant di Azure Active Directory **AdatumLab500-04**.

2. Nel pannello **AdatumLab500-04** fare clic su**Sicurezza** nella sezione **Gestisci**.

3. Nella sezione **Sicurezza \| Attività iniziali** fare clic su **Identity Protection** nella sezione **Proteggi**.

4. Nel pannello **Identity Protection \| Panoramica** fare clic su **Criteri di rischio utente**.

5. Nel pannello **Identity Protection \| Criteri di rischio utente** impostare **Applica criteri** su **No** e quindi fare clic su **Salva**.

6. Nel pannello **Identity Protection \| Criteri di rischio utente** fare clic su **Criteri di rischio di accesso**

7. Nel pannello **Identity Protection \| Criteri di rischio di accesso** impostare **Applica criteri** su **No** e quindi fare clic su **Salva**.

Usare la procedura seguente per arrestare la macchina virtuale di Azure di cui è stato effettuato il provisioning in precedenza nel lab.

1. Nel portale di Azure impostare il filtro **Directory e sottoscrizione** sul tenant di Azure AD associato alla sottoscrizione di Azure in cui è stata distribuita la macchina virtuale di Azure **az500-04-vm1**.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Macchine virtuali** e premere **INVIO**.

3. Nel pannello **Macchine virtuali** fare clic sulla voce **az500-04-vm1**. 
 
4. Nel pannello **az500-04-vm1** fare clic su **Arresta** e, quando richiesto, fare clic su **OK** per confermare 

>  Non rimuovere le risorse di cui è stato effettuato il provisioning in questo lab, perché il lab PIM ha una dipendenza da tali risorse.
