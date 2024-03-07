---
lab:
  title: 02 - Autenticazione a più fattori e accesso condizionale
  module: Module 01 - Manage Identity and Access
---

# Lab 02: autenticazione a più fattori e accesso condizionale
# Manuale del lab per gli studenti

## Scenario laboratorio

È stato chiesto di creare un modello di verifica delle funzionalità che migliorano l'autenticazione dell'ID di Microsoft Entra. In particolare, sarà necessario valutare quanto segue:

- Autenticazione a più fattori di Microsoft Entra ID
- Accesso condizionale di Microsoft Entra ID
- Criteri basati sui rischi per l'accesso condizionale di Microsoft Entra ID

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verranno completati gli esercizi seguenti:

- Esercizio 1: Distribuire una macchina virtuale di Azure usando un modello di Azure Resource Manager
- Esercizio 2: Implementare Azure MFA
- Esercizio 3: Implementare i criteri di accesso condizionale di Microsoft Entra ID 
- Esercizio 4: Implementare Microsoft Entra ID Identity Protection

## Diagramma di MFA - Accesso condizionale - Identity Protection

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/246a3798-6f50-4a41-99c2-71e9ab6a4c8f)

## Istruzioni

## File del lab:

- **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.json**
- **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.parameters.json** 

### Esercizio 1: Distribuire una macchina virtuale di Azure usando un modello di Azure Resource Manager

### Tempo stimato: 10 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Distribuire una macchina virtuale di Azure usando un modello di Azure Resource Manager.

#### Attività 1: Distribuire una macchina virtuale di Azure usando un modello di Azure Resource Manager

In questa attività si creerà una macchina virtuale usando un modello di ARM. La macchina virtuale verrà usata nell'ultimo esercizio per questo lab. 

1. Accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: accedere al portale di Azure usando un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure in uso per questo lab e il ruolo Global Amministrazione istrator nel tenant di Microsoft Entra ID associato a tale sottoscrizione.

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
   |Abbonamento|Nome della sottoscrizione di Azure che verrà usata nel lab|
   |Gruppo di risorse|Fare clic su **Crea nuovo** e digitare il nome **AZ500LAB04**|
   |Ubicazione|**Stati Uniti orientali**|
   |Dimensioni macchina virtuale|**Standard_D2s_v3**|
   |Nome VM|**az500-04-vm1**|
   |Nome utente amministratore|**Studente**|
   |Password amministratore|**Creare una password personalizzata e registrarla per riferimento futuro. Verrà richiesta la password per l'accesso al lab richiesto.**|
   |Nome rete virtuale|**az500-04-vnet1**|

   >**Nota**: per identificare le aree di Azure in cui è possibile effettuare il provisioning di macchine virtuali di Azure, vedere [**https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/)

10. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: non attendere il completamento della distribuzione, ma procedere con l'esercizio successivo. La macchina virtuale inclusa in questa distribuzione verrà usata nell'ultimo esercizio di questo lab.

> Risultato: è stata avviata la distribuzione di un modello di una VM di Azure **az500-04-vm1** che verrà usata nell'ultimo esercizio di questo lab.


### Esercizio 2: Implementare Azure MFA

### Tempo stimato: 30 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Creare un nuovo tenant di Microsoft Entra ID.
- Attività 2: Attivare la versione di valutazione di Microsoft Entra ID P2.
- Attività 3: Creare utenti e gruppi di Microsoft Entra ID.
- Attività 4: Assegnare licenze microsoft Entra ID P2 agli utenti di Microsoft Entra ID.
- Attività 5: Configurare le impostazioni di Azure MFA.
- Attività 6: Convalidare la configurazione di MFA

#### Attività 1: Creare un nuovo tenant di Microsoft Entra ID

In questa attività verrà creato un nuovo tenant di Microsoft Entra ID. 

1. Nella casella di testo Cerca risorse, servizi e documenti della portale di Azure **nella parte superiore della pagina portale di Azure digitare **Microsoft Entra ID** e premere **INVIO**.**

2. Nel pannello che **visualizza Panoramica** del tenant corrente di Microsoft Entra ID fare clic su Gestisci tenant** e quindi nella schermata successiva fare clic **su **+ Crea**.

3. **Nella scheda Informazioni di base** del **pannello Crea un tenant** verificare che l'opzione **Microsoft Entra ID** sia selezionata e fare clic su **Avanti: Configurazione >**.

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


#### Attività 2: Attivare la versione di valutazione di Microsoft Entra ID P2

In questa attività si eseguirà l'iscrizione per la versione di valutazione gratuita di Microsoft Entra ID P2. 

1. Sulla barra degli strumenti del portale di Azure fare clic sull'icona **Directory e sottoscrizione** a destra dell'icona Cloud Shell. 

2. Nel pannello **Directory e sottoscrizione** fare clic sul tenant appena creato **AdatumLab500-04** e quindi fare clic sul pulsante **Cambia** per impostarlo come directory corrente.

    >**Nota**: se la voce **AdatumLab500-04** non viene visualizzata nell'elenco di filtro **Directory e sottoscrizione**, può essere necessario aggiornare la finestra del browser.

3. Nella casella di testo Cerca risorse, servizi e documenti della portale di Azure **nella parte superiore della pagina portale di Azure digitare **Microsoft Entra ID** e premere **INVIO**.** Nel pannello **AdatumLab500-04** fare clic su**Licenze** nella sezione **Gestisci**.

4. Nel pannello Panoramica licenze\|, in **Attività** rapide, fare clic su **Ottieni una versione di valutazione** gratuita.** **

5. Espandere MICROSFT ENTRA ID P2, quindi fare clic su **Attiva.**


#### Attività 3: Creare utenti e gruppi di Microsoft Entra ID.

In questa attività si creeranno tre utenti: aaduser1 (amministratore globale), aaduser2 (utente) e aaduser3 (utente). Per le attività successive sarà necessario il nome e la password dell'entità utente di ogni utente. 

1. Tornare al **pannello AdatumLab500-04** Microsoft Entra ID e, nella **sezione Gestisci** , fare clic su **Utenti**.

2. Nel pannello **Utenti tutti gli \| utenti** fare clic su **+ Nuovo utente** e quindi su **Crea nuovo utente**. 

3. **Nel pannello Nuovo utente verificare che l'opzione **Crea utente**** sia selezionata e specificare le impostazioni seguenti nella scheda Informazioni di base (lasciare tutti gli altri con i valori predefiniti) e fare clic su **Avanti: Proprietà >**:

   |Impostazione|valore|
   |---|---|
   |Nome dell'entità utente|**aaduser1**|
   |Name|**aaduser1**|
   |Password|assicurarsi che l'opzione **Genera automaticamente la password** sia selezionata|
   
      >**Nota**: registrare il nome completo dell'entità utente. È possibile copiarne il valore facendo clic sul **pulsante Copia negli Appunti** sul lato destro dell'elenco a discesa che visualizza il nome di dominio. Sarà necessaria più avanti in questo lab.
   
      >**Nota**: registrare la password dell'utente. È possibile copiarne il valore facendo clic sul **pulsante Copia negli Appunti** sul lato destro della casella di testo. Sarà necessaria più avanti in questo lab. 
   
4. Nella **scheda Proprietà** scorrere fino alla fine e specificare La posizione di utilizzo: **Stati Uniti** (lasciare tutti gli altri con i valori predefiniti) e fare clic su **Avanti: Assegnazioni >**.

5. Nella **scheda Assegnazioni** fare clic su **+ Aggiungi ruolo** e cercare e selezionare **Global Amministrazione istrator**. Fare clic su **Seleziona, quindi **rivedi e crea** e quindi su **Crea****.

6. Nel pannello **Utenti \| Tutti gli utenti** fare clic su **+ Nuovo utente**. 

7. Nel pannello **Nuovo utente verificare che l'opzione **Crea utente**** sia selezionata e specificare le impostazioni seguenti (lasciare tutti gli altri con i valori predefiniti) e fare clic su **Avanti: Proprietà >**:

   |Impostazione|valore|
   |---|---|
   |Nome dell'entità utente|**aaduser2**|
   |Name|**aaduser2**|
   |Password|assicurarsi che l'opzione **Genera automaticamente la password** sia selezionata| 

    >**Nota**: registrare il nome completo dell'entità utente e la password.

8. Nella **scheda Proprietà** scorrere fino alla fine e specificare la posizione di utilizzo: **Stati Uniti** (lasciare tutti gli altri con i valori predefiniti) e fare clic su **Rivedi e crea** e quindi su **Crea**.

9. Nel pannello **Utenti \| Tutti gli utenti** fare clic su **+ Nuovo utente**. 

10. Nel pannello **Nuovo utente verificare che l'opzione **Crea utente**** sia selezionata e specificare le impostazioni seguenti (lasciare tutti gli altri con i valori predefiniti) e fare clic su **Avanti: Proprietà >**:

    |Impostazione|valore|
    |---|---|
    |Nome dell'entità utente|**aaduser3**|
    |Name|**aaduser3**|
    |Password|assicurarsi che l'opzione **Genera automaticamente la password** sia selezionata|

    >**Nota**: registrare il nome completo dell'entità utente e la password.

11. Nella **scheda Proprietà** scorrere fino alla fine e specificare la posizione di utilizzo: **Stati Uniti** (lasciare tutti gli altri con i valori predefiniti) e fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: a questo punto, nella pagina **Utenti** dovrebbero essere elencati i tre nuovi utenti. 
    
#### Attività 4: Assegnare licenze Microsoft Entra ID Premium P2 agli utenti di Microsoft Entra ID

In questa attività si assegnerà ogni utente alla licenza Microsoft Entra ID Premium P2.

1. Nel pannello **Utenti \| Tutti gli utenti** fare clic sulla voce che rappresenta l'account utente. 

2. Nel pannello che visualizza le proprietà degli account utente fare clic su **Modifica proprietà**.  Verificare che la posizione di utilizzo sia impostata su **Stati Uniti**. In caso contrario, impostare il percorso di utilizzo e fare clic su **Salva**.

3. Tornare al **pannello AdatumLab500-04** Microsoft Entra ID e, nella **sezione Gestisci** , fare clic su **Licenze**.

4. Nel pannello Panoramica licenze \| fare clic su **Tutti i prodotti**, selezionare la **casella di controllo Microsoft Entra ID Premium P2** e fare clic su **+ Assegna**.** **

5. Nel pannello **Assegna licenza** fare clic su **+ Aggiungi utenti e gruppi**.

6. Nel pannello **Utenti** selezionare **aaduser1**, **aaduser2**, **aaduser3** e il proprio account utente e fare clic su **Seleziona**.

7. Nel pannello **Assegna licenze** fare clic su **Opzioni** di assegnazione, verificare che tutte le opzioni siano abilitate, fare clic su **Rivedi e assegna** e quindi su **Assegna**.

8. Disconnettersi dal portale di Azure accedere di nuovo usando lo stesso account. Questo passaggio è necessario per rendere effettiva l'assegnazione di licenza.

    >**Nota**: a questo punto, sono state assegnate licenze Microsoft Entra ID Premium P2 a tutti gli account utente che verranno usati in questo lab. Assicurarsi di disconnettersi e quindi accedere di nuovo. 

#### Attività 5: Configurare le impostazioni di Azure MFA.

In questa attività si configurerà MFA, che verrà abilitata per aaduser1. 

1. Nella portale di Azure tornare al **pannello tenant AdatumLab500-04** Microsoft Entra ID.

    >**Nota**: assicurarsi di usare il tenant AdatumLab500-04 Microsoft Entra ID.

2. Nel **pannello tenant AdatumLab500-04** Microsoft Entra ID fare clic su **Sicurezza** nella **sezione Gestione**.

3. Nel pannello **Sicurezza \| Attività iniziali**, nella sezione **Gestisci** fare clic su **Autenticazione a più fattori**.

4. Nel pannello **Autenticazione a più fattori \| Attività iniziali** fare clic sul collegamento **Altre impostazioni di autenticazione a più fattori basate sul cloud**. 

    >**Nota**: si aprirà una nuova scheda del browser che visualizza la pagina **Autenticazione a più fattori**.

5. Nella pagina **Autenticazione a più fattori** fare clic sulla scheda **Impostazioni servizio**. Esaminare le **opzioni di verifica**. Si noti che sono abilitate le opzioni **SMS al telefono**, **Notifica tramite l'app per dispositivi mobili** e **Codice di verifica dall'app per dispositivi mobili o dal token hardware**. Fare clic su **Salva** e quindi su **Chiudi**.

6. Passare alla **scheda Utenti** , fare clic su **aaduser1** voce, fare clic sul **collegamento Abilita** e, quando richiesto, fare clic su **Abilita autenticazione a più fattori** e quindi fare clic su **Chiudi**.

7. Si noti che la colonna **Stato di Multi-Factor Auth** per **aaduser1** indica ora **Abilitato**.

8. Fare clic su **aaduser1** e notare che a questo punto è disponibile anche l'opzione **Applica**. 

    >**Nota**: la modifica dello stato utente da Abilitato a Imponi influisce solo sulle app integrate di Microsoft Entra ID legacy che non supportano Azure MFA e, dopo che lo stato cambia in Applicato, richiedono l'uso delle password dell'app.

9. Con la voce **aaduser1** selezionata, fare clic su **Gestisci impostazioni utente** ed esaminare le opzioni disponibili: 

   - Richiedere agli utenti selezionati di fornire di nuovo i metodi di contatto.

   - Eliminare tutte le password dell'app esistenti generate dagli utenti selezionati.

   - Ripristina l'autenticazione a più fattori in tutti i dispositivi memorizzati.

10. Fare clic su **Annulla** e tornare alla scheda del browser che visualizza il **pannello Attività iniziali** di Multi-Factor Authentication \| nel portale di Azure.

11. Nella sezione **Impostazioni** fare clic su **Avviso di illecito**.

12. Nel pannello **Multi-Factor Authentication \| Avviso di illecito** configurare le impostazioni seguenti:

    |Impostazione|valore|
    |---|---|
    |Consenti agli utenti di inviare avvisi di illeciti|**On**|
    |Blocca automaticamente gli utenti per cui è stato segnalato un illecito|**On**|
    |Codice per segnalare illeciti durante il messaggio introduttivo iniziale|**0**|

13. Fare clic su **Salva**

    >**Nota**: a questo punto è stata abilitata la funzionalità MFA per aaduser1 e sono state configurate le impostazioni di avviso per gli illeciti. 

14. Tornare al **pannello tenant AdatumLab500-04** Microsoft Entra ID, nella **sezione Gestione** fare clic su **Proprietà**, quindi fare clic sul **collegamento Gestisci impostazioni predefinite** di sicurezza nella parte inferiore del pannello, nel pannello **Abilita impostazioni predefinite** di sicurezza fare clic su **Disabilitato**. Selezionare **My Organization is using Conditional Access (L'organizzazione usa l'accesso *** condizionale) come motivo per la disabilitazione*, fare clic su **Salva**, leggere l'avviso e quindi fare clic su **Disabilita**.

    >**Nota**: assicurarsi di aver eseguito l'accesso al **tenant di AdatumLab500-04** Microsoft Entra ID. È possibile usare il **filtro directory e sottoscrizione** per passare dai tenant di Microsoft Entra ID. Assicurarsi di aver eseguito l'accesso come utente con il ruolo Global Amministrazione istrator nel tenant di Microsoft Entra ID.

#### Attività 6: Convalidare la configurazione di MFA

In questa attività si convaliderà la configurazione di MFA testando l'accesso dell'account utente aaduser1. 

1. Aprire una finestra del browser InPrivate.

2. Passare alla portale di Azure, **`https://portal.azure.com/`** e accedere usando l'account **utente aaduser1.** 

    >**Nota**: per accedere è necessario specificare un nome completo dell'account **utente aaduser1** , incluso il nome di dominio DNS del tenant di Microsoft Entra ID registrato in precedenza in questo lab. Questo nome utente è nel formato aaduser1@`<your_tenant_name>`.onmicrosoft.com, dove `<your_tenant_name>` è il segnaposto che rappresenta il nome univoco del tenant dell'ID Microsoft Entra. 

3. Quando richiesto, nella finestra di dialogo **Sono necessarie altre informazioni** selezionare **Avanti**.

    >**Nota**: la sessione del browser verrà reindirizzata alla pagina **Verifica di sicurezza aggiuntiva**.

4. Nella pagina **Proteggi l'account** selezionare il collegamento **Si vuole configurare un metodo diverso**, selezionare **Telefono** nell'elenco a discesa **Specificare il metodo da usare**, quindi selezionare **Conferma**.

5. Nella pagina **Proteggi l'account** selezionare il paese o l'area geografica, digitare il proprio numero di cellulare nell'area **Immettere il numero di telefono**, assicurarsi che sia selezionata l'opzione **Invia un SMS** e fare clic su **Avanti**.
 
6. Nella pagina **Proteggi l'account** digitare il codice ricevuto nell'SMS sul telefono cellulare e fare clic su **Avanti**.

7. Nella pagina **Proteggi l'account** assicurarsi che la verifica sia riuscita e fare clic su **Avanti**.

8. Se viene richiesto un metodo di autenticazione aggiuntivo, fare clic su **Desidero usare un metodo** diverso, selezionare **Posta elettronica** dall'elenco a discesa, fare clic su **Conferma**, specificare l'indirizzo di posta elettronica che si intende usare e fare clic su **Avanti**. Identificare il codice nel corpo del messaggio di posta elettronica corrispondente ricevuto e quindi fare clic su **Fine**.

9. Quando richiesto, modificare la password. Assicurarsi di registrare la nuova password.

10. Verificare di aver eseguito correttamente l'accesso al portale di Azure.

11. Disconnettersi come **aaduser1** e chiudere la finestra del browser InPrivate.

> Risultato: è stato creato un nuovo tenant di AD, sono stati configurati gli utenti di AD, è stata configurata la funzionalità MFA ed è stata testata l'esperienza di MFA per un utente. 


### Esercizio 3: Implementare i criteri di accesso condizionale di Microsoft Entra ID 

### Tempo stimato: 15 minuti

In questo esercizio si completeranno le seguenti attività: 

- Attività 1: Configurare un criterio di accesso condizionale.
- Attività 2: Testare il criterio di accesso condizionale.

#### Attività 1 - Configurare un criterio di accesso condizionale. 

In questa attività si esamineranno le impostazioni dei criteri di accesso condizionale e si creerà un criterio che richiede MFA per l'accesso al portale di Azure. 

1. Nella portale di Azure tornare al **pannello tenant AdatumLab500-04** Microsoft Entra ID.

2. Nel pannello **AdatumLab500-04** fare clic su**Sicurezza** nella sezione **Gestisci**.

3. Nella sezione **Sicurezza \| Attività iniziali** fare clic su **Accesso condizionale** nella sezione **Proteggi**. Nel pannello di spostamento sinistro fare clic su **Criteri**.

4. Nel pannello **Accesso condizionale \| Criteri** fare clic su **+ Nuovo criterio**. 

5. Nel pannello **Nuovo** configurare le impostazioni seguenti:

   - Nella casella di testo **Nome** digitare **AZ500Policy1**
    
   - In **Utenti** fare clic su **0 Utenti e gruppi selezionati**. Sul lato destro in Includi, Abilita **Seleziona utenti e gruppi** >> selezionare la **casella di controllo Utenti e gruppi** , nel **pannello Seleziona utenti e gruppi** selezionare la **casella di controllo aaduser2** e fare clic su **Seleziona**.
    
   - In **Risorse di** destinazione fare clic su **Nessuna risorsa di destinazione selezionata**, fare clic su **Seleziona app**, in Seleziona fare clic su **Nessuno**. Nel pannello **Seleziona** selezionare la casella di controllo relativa all'API **** gestione dei servizi di Windows Azure e fare clic su **Seleziona**. 

     >**Nota**: leggere l'avviso che indica che questo criterio influisce sull'accesso al portale di Azure.
    
   - In **Condizioni** fare clic su **0 condizioni selezionate**, in **Rischio di accesso** fare clic su **Non configurato**. **Nel pannello Rischio** di accesso esaminare i livelli di rischio, ma non apportare modifiche e chiudere il **pannello Rischio** di accesso.
    
   - In **Piattaforme** del dispositivo fare clic su **Non configurato**, esaminare le piattaforme del dispositivo che possono essere incluse, ma non apportare modifiche e fare clic su **Fine**.
    
   - In **Percorsi** fare clic su **Non configurato** ed esaminare le opzioni di posizione senza apportare modifiche.
    
   - In **Concedi** nella **sezione Controlli** di accesso fare clic su **0 controlli selezionati**. Nel pannello **Concedi** selezionare la **casella di controllo Richiedi autenticazione** a più fattori e fare clic su **Seleziona**
    
   - Impostare **Abilita criterio** su **Sì**.

6. Nel pannello **Nuovo** fare clic su **Crea**. 

    >**Nota**: a questo punto sono presenti criteri di accesso condizionale che richiedono MFA per l'accesso al portale di Azure. 

#### Attività 2: Testare il criterio di accesso condizionale.

In questa attività si accederà al portale di Azure come **aaduser2** e si verificherà che sia necessaria l'autenticazione MFA. Si eliminerà anche il criterio prima di proseguire con l'esercizio successivo. 

1. Aprire una finestra di Microsoft Edge InPrivate.

2. Nella nuova finestra del browser passare al portale di Azure, **`https://portal.azure.com/`** e accedere con l'account **utente aaduser2.**

3. Quando richiesto, nella finestra di dialogo **Sono necessarie altre informazioni** selezionare **Avanti**.

    >**Nota**: la sessione del browser verrà reindirizzata alla pagina Mantieni sicuro** l'account**.
    
4. Nella pagina **Proteggi l'account** selezionare il collegamento **Si vuole configurare un metodo diverso**, selezionare **Telefono** nell'elenco a discesa **Specificare il metodo da usare**, quindi selezionare **Conferma**.

5. Nella pagina **Proteggi l'account** selezionare il paese o l'area geografica, digitare il proprio numero di cellulare nell'area **Immettere il numero di telefono**, assicurarsi che sia selezionata l'opzione **Invia un SMS** e fare clic su **Avanti**.

6. Nella pagina **Proteggi l'account** digitare il codice ricevuto nell'SMS sul telefono cellulare e fare clic su **Avanti**.

7. Nella pagina **Proteggi l'account** assicurarsi che la verifica sia riuscita e fare clic su **Avanti**.

8. Nella pagina **Proteggi l'account** fare clic su **Fine**.

9. Quando richiesto, modificare la password. Assicurarsi di registrare la nuova password.

10. Verificare di aver eseguito correttamente l'accesso al portale di Azure.

11. Disconnettersi come **aaduser2** e chiudere la finestra del browser InPrivate.

    >**Nota**:è stato verificato che il criterio di accesso condizionale appena creato impone MFA quando l'utente aaduser2 accede al portale di Azure.

12. Tornare alla finestra del browser che visualizza il portale di Azure, tornare al **pannello Tenant AdatumLab500-04** Microsoft Entra ID.

13. Nel pannello **AdatumLab500-04** fare clic su**Sicurezza** nella sezione **Gestisci**.

14. Nella sezione **Sicurezza \| Attività iniziali** fare clic su **Accesso condizionale** nella sezione **Proteggi**. Nel pannello di spostamento sinistro fare clic su **Criteri**.

15. Nel pannello **Accesso condizionale \| Criteri** fare clic sui puntini di sospensione accanto a **AZ500Policy1**, fare clic su **Elimina** e, quando richiesto, fare clic su **Sì** per confermare.

    >**Nota**: in questo esercizio si implementa un criterio di accesso condizionale per richiedere l'autenticazione MFA quando un utente accede al portale di Azure. 

>Risultato: è stato configurato e testato l'accesso condizionale di Microsoft Entra ID.

### Esercizio 4: Distribuire criteri basati sui rischi nell'accesso condizionale

### Tempo stimato: 30 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Visualizzare le opzioni di Microsoft Entra ID Identity Protection nel portale di Azure
- Attività 2: Configurare un criterio di rischio utente
- Attività 3: Configurare un criterio di rischio di accesso
- Attività 4: Simulare eventi di rischio rispetto ai criteri di Microsoft Entra ID Identity Protection 
- Attività 5: Esaminare i report di Microsoft Entra ID Identity Protection

#### Attività 1: Abilitare Microsoft Entra ID Identity Protection

In questa attività verranno visualizzate le opzioni di Microsoft Entra ID Identity Protection nel portale di Azure. 

1. Se necessario, accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: assicurarsi di aver eseguito l'accesso al **tenant di AdatumLab500-04** Microsoft Entra ID. È possibile usare il **filtro directory e sottoscrizione** per passare dai tenant di Microsoft Entra ID. Assicurarsi di aver eseguito l'accesso come utente con il ruolo Global Amministrazione istrator nel tenant di Microsoft Entra ID.

#### Attività 2: Configurare un criterio di rischio utente

In questa attività si creerà un criterio di rischio utente. 

1. Passare al **tenant AdatumLab500-04** Microsoft Entra ID > **Criteri** di accesso > ****condizionale per la sicurezza.** > **

2. Fare clic su **+ Nuovo criterio**.

3. Nella casella di testo **Nome** digitare il nome del criterio **AZ500Policy2**.

4. In **Assegnazioni** > **Utenti** fare clic su **0 Utenti e gruppi selezionati**.

5. In **Includi** fare clic su **Seleziona utenti e gruppi, fare clic su **Utenti e gruppi****, selezionare **aaduser2** e **aaduser3** e quindi fare clic su **Seleziona**.

6. In **Escludi** fare clic su **Utenti e gruppi**, selezionare **aaduser1** e quindi fare clic su **Seleziona**. 

7. In **Risorse di** destinazione fare clic su **Nessuna risorsa di destinazione selezionata, verificare che **l'opzione App** cloud sia selezionata** nell'elenco a discesa e, in **Includi**, selezionare **Tutte le app** cloud.

8. In **Condizioni** fare clic su **0 condizioni selezionate**, in **Rischio** utente fare clic su **Non configurato**. Nel pannello **Rischio** utente impostare Configura** su ****Sì**.

9. In **Consente di configurare i livelli di rischio utente necessari per l'applicazione dei criteri** selezionare **Alto**.

10. Fare clic su **Fatto**.

11. In **Concedi** nella **sezione Controlli** di accesso fare clic su **0 controlli selezionati**. Nel pannello Concedi** verificare che **l'opzione **Concedi accesso** sia selezionata.

12. Selezionare **Richiedi autenticazione a più fattori** e **Richiedi la modifica della password**.

13. Fare clic su **Seleziona**.

14. In **Sessione** fare clic su **0 controlli selezionati**. Selezionare **Frequenza** di accesso e quindi selezionare **Ogni volta**.

15. Fare clic su **Seleziona**.

16. Verificare che **l'opzione Abilita criterio** sia impostata **su Solo** report.

17. Fare clic su **Crea** per abilitare il criterio.

#### Attività 3: Configurare un criterio di rischio di accesso

1. Passare al **tenant AdatumLab500-04** Microsoft Entra ID > **Criteri** di accesso> ****condizionale per la sicurezza.** > **

2. Selezionare **+ Nuovi criteri**.

3. Nella casella di testo **Nome** digitare il nome del criterio **AZ500Policy3**.

4. In **Assegnazioni** > **Utenti** fare clic su **0 Utenti e gruppi selezionati**.

5. In **Includi** fare clic su **Seleziona utenti e gruppi**, fare clic su **Utenti e gruppi**, selezionare **aaduser2** e aaduser3** e **fare clic su **Seleziona**.

6. In **Escludi** fare clic su **Utenti e gruppi** selezionare **aaduser1** e fare clic su **Seleziona**. 

7. In **Risorse di** destinazione fare clic su **Nessuna risorsa di destinazione selezionata, verificare che **l'opzione App** cloud sia selezionata** nell'elenco a discesa e, in **Includi**, selezionare **Tutte le app** cloud.

8. In **Condizioni** fare clic su **0 condizioni selezionate**, in **Rischio di accesso** fare clic su **Non configurato**. Nel pannello **Rischio di accesso** impostare Configura** su ****Sì**.

9. In **Selezionare il livello di rischio di accesso a cui verranno applicati questi criteri** selezionare **Alto** e **Medio**.

10. Fare clic su **Fatto**.

11. In **Concedi** nella **sezione Controlli** di accesso fare clic su **0 controlli selezionati**. Nel pannello Concedi** verificare che **l'opzione **Concedi accesso** sia selezionata.   

12. Selezionare **Richiedi autenticazione** a più fattori.

13. Fare clic su **Seleziona**.

14. In **Sessione** fare clic su **0 controlli selezionati**. Selezionare **Frequenza** di accesso e quindi selezionare **Ogni volta**.

15. Fare clic su **Seleziona**.

16. Confermare le impostazioni e impostare **Abilita criterio** su **Attivato**.

17. Fare clic su **Crea** per abilitare il criterio.

#### Attività 4: Simulare eventi di rischio rispetto ai criteri di Microsoft Entra ID Identity Protection 

> Prima di iniziare questa attività, assicurarsi che la distribuzione del modello avviata nell'Esercizio 1 sia stata completata. La distribuzione include una macchina virtuale di Azure denominata **az500-04-vm1**. 

1. Nella portale di Azure impostare il **filtro Directory + sottoscrizione** sul tenant di Microsoft Entra ID associato alla sottoscrizione di Azure in cui è stata distribuita la **macchina virtuale di Azure az500-04-vm1**.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Macchine virtuali** e premere **INVIO**.

3. Nel pannello **Macchine virtuali** fare clic sulla voce **az500-04-vm1**. 

4. Nel pannello **az500-04-vm1** fare clic su **Connessione**. Verificare di essere nella **scheda RDP** .

5. Fare clic su **Scarica file RDP** e usare il file per connettersi alla macchina virtuale di Azure **az500-04-vm1** tramite Desktop remoto. Quando viene chiesto di eseguire l'autenticazione, specificare le credenziali seguenti:

    |Impostazione|Valore|
    |---|---|
    |Nome utente|**Studente**|
    |Password|**Usare la password personale creata in Lab 02 > Esercizio 1 > Attività 1 > Passaggio 9.**|

    >**Nota**: attendere l'apertura della sessione Desktop remoto e il caricamento di **Server Manager**.  

    >**Nota**: i passaggi seguenti vengono eseguiti nella sessione Desktop remoto nella macchina virtuale di Azure **az500-04-vm1**. 

6. In **Server Manager** fare clic su **Server locale** e quindi su **Configurazione sicurezza avanzata IE**.

7. Nella finestra di dialogo **Protezione avanzata di Internet Explorer** impostare entrambe le opzioni su **No** e fare clic su **OK**.

8. Avviare **Internet Explorer**, fare clic sull'icona a forma di ingranaggio sulla barra degli strumenti, scegliere **Sicurezza** dal menu a discesa e quindi fare clic su **InPrivate Browsing**.

9. Nella finestra InPrivate di Internet Explorer passare a ToR Browser Project all'indirizzo **https://www.torproject.org/projects/torbrowser.html.en**.

10. Scaricare e installare la versione di Windows del browser ToR con le impostazioni predefinite. 

11. Al termine dell'installazione, avviare il browser ToR, usare l'opzione **Connessione** nella pagina iniziale e passare al Pannello di accesso dell'applicazione all'indirizzo **https://myapps.microsoft.com**.

12. Quando richiesto, provare ad accedere con l'account **aaduser3**. 

    >**Nota**: verrà visualizzato il messaggio **L'accesso è stato bloccato**. Questo è previsto, poiché questo account non è configurato con l'autenticazione a più fattori, che è necessario a causa di un maggiore rischio di accesso associato all'uso del browser ToR.

13. Nel browser ToR selezionare la freccia indietro per accedere come **account aaduser1** creato e configurato per l'autenticazione a più fattori in precedenza in questo lab.

    >**Nota**: questa volta verrà visualizzato il messaggio **È stata rilevata un'attività sospetta**. Anche in questo caso, questo account è configurato con l'autenticazione a più fattori. Considerando l'aumento del rischio di accesso associato all'uso del browser ToR, sarà necessario usare l'autenticazione a più fattori.

14. Usare l'opzione **Verifica** e specificare se si vuole verificare la propria identità tramite SMS o con una chiamata.

15. Completare la verifica e assicurarsi di aver eseguito correttamente l'accesso ad Application Access Panel.

16. Chiudere la sessione RDP. 

    >**Nota**: a questo punto si è tentato di eseguire due accessi diversi. Verranno quindi esaminati i report di Azure Identity Protection.

#### Attività 5: Esaminare i report di Microsoft Entra ID Identity Protection

In questa attività verranno esaminati i report di Microsoft Entra ID Identity Protection generati dagli account di accesso del browser ToR.

1. Tornare alla portale di Azure, usare il **filtro Directory e sottoscrizione** per passare al **tenant AdatumLab500-04** Microsoft Entra ID.

2. Nel pannello **AdatumLab500-04** fare clic su**Sicurezza** nella sezione **Gestisci**.

3. Nel pannello **Sicurezza \| Attività iniziali** fare clic su **Utenti a rischio** nella sezione **Report**. 

4. Esaminare il report e identificare eventuali voci che fanno riferimento all'account utente **aaduser3**.

5. Nel pannello **Sicurezza \| Attività iniziali** fare clic su **Accessi a rischio** nella sezione **Report**. 

6. Esaminare il report e identificare eventuali voci che corrispondono all'accesso con l'account utente **aaduser3**.

7. In **Report** fare clic su **Rilevamenti dei rischi**.

8. Esaminare il report e identificare eventuali voci che rappresentano l'accesso da un indirizzo IP anonimo generato dal browser ToR. 

    >**Nota**: potrebbero essere necessari 10-15 minuti prima che i rischi vengano visualizzati nei report.

> **Risultato**: è stato abilitato Microsoft Entra ID Identity Protection, sono stati configurati i criteri di rischio utente e i criteri di rischio di accesso, nonché la configurazione convalidata di Microsoft Entra ID Identity Protection simulando gli eventi di rischio.

**Pulire le risorse**

> È necessario rimuovere le risorse di Identity Protection che non vengono più usate. 

Seguire questa procedura per disabilitare i criteri di protezione delle identità nel **tenant AdatumLab500-04** Microsoft Entra ID.

1. Nella portale di Azure tornare al **pannello tenant AdatumLab500-04** Microsoft Entra ID.

2. Nel pannello **AdatumLab500-04** fare clic su**Sicurezza** nella sezione **Gestisci**.

3. Nella sezione **Sicurezza \| Attività iniziali** fare clic su **Identity Protection** nella sezione **Proteggi**.

4. Nel pannello **Identity Protection \| Panoramica** fare clic su **Criteri di rischio utente**.

5. Nel pannello **Criteri di rischio** utente di Identity Protection \| impostare **Imposizione criteri** su **Disabilita** e quindi fare clic su **Salva**.

6. Nel pannello **Criteri di rischio utente di Identity Protection \| fare clic su **Criteri di rischio**** di accesso.

7. Nel pannello **Criteri di rischio** di accesso di Identity Protection \| impostare **Imposizione criteri** su **Disabilita** e quindi fare clic su **Salva**.

Usare la procedura seguente per arrestare la macchina virtuale di Azure di cui è stato effettuato il provisioning in precedenza nel lab.

1. Nella portale di Azure impostare il **filtro Directory + sottoscrizione** sul tenant di Microsoft Entra ID associato alla sottoscrizione di Azure in cui è stata distribuita la **macchina virtuale di Azure az500-04-vm1**.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Macchine virtuali** e premere **INVIO**.

3. Nel pannello **Macchine virtuali** fare clic sulla voce **az500-04-vm1**. 
 
4. Nel pannello **az500-04-vm1** fare clic su **Arresta** e, quando richiesto, fare clic su **OK** per confermare 

>  Non rimuovere le risorse di cui è stato effettuato il provisioning in questo lab, perché il lab PIM ha una dipendenza da tali risorse.
