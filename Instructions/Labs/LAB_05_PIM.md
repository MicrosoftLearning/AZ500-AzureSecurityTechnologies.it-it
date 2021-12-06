---
lab:
  title: 05 - Azure AD Privileged Identity Management
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: 9e0b57125345a57e9f050667c7df18c6b8585ef9
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/16/2021
ms.locfileid: "132625680"
---
# <a name="lab-05-azure-ad-privileged-identity-management"></a>Lab 05: Azure AD Privileged Identity Management
# <a name="student-lab-manual"></a>Manuale del lab per gli studenti

## <a name="lab-scenario"></a>Scenario del lab

È stato chiesto di creare un modello di verifica che usa Azure Privileged Identity Management (PIM) per abilitare l'amministrazione just-in-time e controllare il numero di utenti che possono eseguire operazioni con privilegi. I requisiti specifici sono:

- Creare un'assegnazione permanente dell'utente di Azure AD aaduser2 al ruolo di amministratore della sicurezza. 
- Configurare l'utente di Azure AD aaduser2 in modo che sia idoneo per i ruoli Amministratore fatturazione e Ruolo con autorizzazioni di lettura globali.
- Configurare l'attivazione del Ruolo con autorizzazioni di lettura globali in modo che richieda un'approvazione dell'utente di Azure AD aaduser3
- Configurare una verifica di accesso del Ruolo con autorizzazioni di lettura globali ed esaminare le funzionalità di controllo.

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

> Prima di procedere, assicurarsi di aver completato il Lab 04: MFA, accesso condizionale e AAD Identity Protection. Saranno necessari il tenant di Azure AD AdatumLab500-04 e gli account utente aaduser1, aaduser2 e aaduser3.

## <a name="lab-objectives"></a>Obiettivi del lab

In questo lab verranno completati gli esercizi seguenti:

- Esercizio 1: Configurare utenti e ruoli di PIM.
- Esercizio 2: Attivare i ruoli di PIM con e senza approvazione.
- Esercizio 3: Creare una verifica di accesso ed esaminare le funzionalità di controllo di PIM.

### <a name="exercise-1---configure-pim-users-and-roles"></a>Esercizio 1: Configurare utenti e ruoli di PIM

#### <a name="estimated-timing-15-minutes"></a>Tempo stimato: 15 minuti

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Rendere un utente idoneo per un ruolo.
- Attività 2: Configurare un ruolo in modo che richieda l'approvazione per attivare e aggiungere un membro idoneo.
- Attività 3: Assegnare a un utente un'assegnazione permanente a un ruolo. 

#### <a name="task-1-make-a-user-eligible-for-a-role"></a>Attività 1: Rendere un utente idoneo per un ruolo

In questa attività si renderà un utente idoneo per un ruolo di Azure AD directory.

1. Accedere al portale di Azure **`https://portal.azure.com/`** .

    >**Nota**: assicurarsi di aver eseguito l'accesso al tenant di Azure AD **AdatumLab500-04**. È possibile usare il filtro **Directory e sottoscrizione** per passare da un tenant di Azure AD a un altro. Assicurarsi di aver eseguito l'accesso come utente con il ruolo di amministratore globale.
    
    >**Nota**: se la voce AdatumLab500-04 non è ancora visualizzata, fare clic sul collegamento Cambia directory, selezionare la riga AdatumLab500-04 e fare clic sul pulsante Cambia.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Azure AD Privileged Identity Management** e premere **INVIO**.

1. Nel pannello **Azure AD Privileged Identity Management** fare clic su **Ruoli di Azure AD** nella sezione **Gestisci**.

1. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Ruoli** nella sezione **Gestisci**.

1. Nel pannello **AdatumLab500-04 \| Ruoli** fare clic su **+ Aggiungi assegnazioni**.

1. Nel pannello **Aggiungi assegnazioni** fare clic su **Amministratore fatturazione** nell'elenco a discesa **Seleziona ruolo**.

1. Fare clic sul collegamento **Nessun membro selezionato**, fare clic su **aaduser2** nel pannello **Selezionare un membro** e quindi fare clic su **Seleziona**.

1. Nel pannello **Aggiungi assegnazioni** fare clic su **Avanti**. 

1. Verificare che l'opzione **Tipo di assegnazione** sia impostata su **Idoneo** e fare clic su **Assegna**.
 
1. Nel pannello **AdatumLab500-04 \| Ruoli** fare clic su **Assegnazioni** nella sezione **Gestisci**.

1. Nel pannello **AdatumLab500-04 \| Assegnazioni** notare le schede **Assegnazioni idonee**, **Assegnazioni attive** e **Assegnazioni scadute**.

1. Nella scheda **Assegnazioni idonee** verificare che **aaduser2** sia visualizzato come **Amministratore fatturazione**. 

    >**Nota**: durante l'accesso, aaduser2 sarà idoneo per l'uso del ruolo Amministratore fatturazione. 

#### <a name="task-2-configure-a-role-to-require-approval-to-activate-and-add-an-eligible-member"></a>Attività 2: Configurare un ruolo in modo che richieda l'approvazione per attivare e aggiungere un membro idoneo

1. Nel portale di Azure tornare nel pannello **Azure AD Privileged Identity Management** e fare clic su **Ruoli di Azure AD**.

1. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Ruoli** nella sezione **Gestisci**.

1. Nel pannello **AdatumLab500-04 \| Ruoli** fare clic sulla voce **Ruolo con autorizzazioni di lettura globali**. 

1. Sulla barra degli strumenti del pannello **Ruolo con autorizzazioni di lettura globali \| Assegnazioni** fare clic su **Impostazioni** ed esaminare le impostazioni di configurazione del ruolo, inclusi i requisiti di Azure Multi-Factor Authentication.

1. Fare clic su **Modifica**.

1. Nella scheda **Attivazione** abilitare la casella di controllo **Richiedi l'approvazione per l'attivazione**.

1. Fare clic su **Selezionare uno o più responsabili approvazione**, fare clic su **aaduser3** nel pannello **Selezionare un membro** e quindi fare clic su **Seleziona**.

1. Fare clic su **Avanti: Assegnazione**.

1. Deselezionare la casella di controllo **Consenti le assegnazioni idonee permanenti**, lasciando i valori predefiniti per tutte le altre impostazioni.

1. Fare clic su **Avanti: Notifica**.

1. Nel pannello **Modifica l'assegnazione di ruolo - Ruolo con autorizzazioni di lettura globali** esaminare le impostazioni e fare clic su **Aggiorna**.

    >**Nota**: tutti gli utenti che tentano di usare il Ruolo con autorizzazioni di lettura globali avranno ora bisogno dell'approvazione di aaduser3. 

1. Nel pannello **Ruolo con autorizzazioni di lettura globali \| Assegnazioni** fare clic su **+ Aggiungi assegnazioni**.

1. Nel pannello **Aggiungi assegnazioni** fare clic su **Nessun membro selezionato**, fare clic su **aaduser2** nel pannello **Selezionare un membro** e quindi fare clic su **Seleziona**.

1. Fare clic su **Avanti**. 

1. Assicurarsi che l'opzione **Tipo di assegnazione** sia impostata su **Idoneo** ed esaminare le impostazioni di durata dell'idoneità.

1. Fare clic su **Assegna**.

    >**Nota**: l'utente aaduser2 è idoneo per il Ruolo con autorizzazioni di lettura globali. 
 
#### <a name="task-3-give-a-user-permanent-assignment-to-a-role"></a>Attività 3: Assegnare a un utente un'assegnazione permanente a un ruolo.

1. Nel portale di Azure tornare nel pannello **Azure AD Privileged Identity Management** e fare clic su **Ruoli di Azure AD**.

1. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Ruoli** nella sezione **Gestisci**.

1. Nel pannello **AdatumLab500-04 \| Ruoli** fare clic su **+ Aggiungi assegnazioni**.

1. Nel pannello **Aggiungi assegnazioni** selezionare **Amministratore della sicurezza** nell'elenco a discesa **Seleziona ruolo**.

1. Nel pannello **Aggiungi assegnazioni** fare clic su **Nessun membro selezionato**, fare clic su **aaduser2** nel pannello **Selezionare un membro** e quindi fare clic su **Seleziona**.

1. Fare clic su **Avanti**. 

1. Esaminare le impostazioni di **Tipo di assegnazione** e fare clic su **Assegna**.

    >**Nota**: l'utente aaduser2 è ora idoneo in modo permanente per il ruolo Amministratore della sicurezza.
    
### <a name="exercise-2---activate-pim-roles-with-and-without-approval"></a>Esercizio 2: Attivare i ruoli di PIM con e senza approvazione

#### <a name="estimated-timing-15-minutes"></a>Tempo stimato: 15 minuti

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Attivare un ruolo che non richiede l'approvazione. 
- Attività 2: Attivare un ruolo che richiede l'approvazione. 

#### <a name="task-1-activate-a-role-that-does-not-require-approval"></a>Attività 1: Attivare un ruolo che non richiede l'approvazione.

In questa attività si attiverà un ruolo che non richiede l'approvazione.

1. Aprire una finestra del browser InPrivate.

1. Aprire una finestra del browser InPrivate, passare al portale di Azure e accedere con l'account utente **aaduser2**.

    >**Nota**: per accedere è necessario specificare un nome completo dell'account utente **aaduser2**, incluso il nome di dominio DNS del tenant di Azure AD registrato in precedenza in questo lab. Questo nome utente è nel formato aaduser2`<your_tenant_name>`.onmicrosoft.com, dove `<your_tenant_name>` è il segnaposto che rappresenta il nome univoco del tenant di Azure AD. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Azure AD Privileged Identity Management** e premere **INVIO**.

1. Nel pannello **Azure AD Privileged Identity Management** fare clic su **Ruoli personali** nella sezione **Attività**.

1. Verranno visualizzate tre opzioni di **Ruoli idonei** per **aaduser2**: **Ruolo con autorizzazioni di lettura globali**, **Amministratore della sicurezza** e **Amministratore fatturazione**. 

1. Nella riga che visualizza la voce **Amministratore fatturazione** fare clic su **Attiva**.

1. Se necessario, fare clic sull'avviso **È necessaria una verifica aggiuntiva. Fare clic per continuare** e seguire le istruzioni per verificare la propria identità.

1. Nella casella di testo **Motivo** del pannello **Attiva - Amministratore fatturazione** digitare un testo che giustifichi l'attivazione e quindi fare clic su **Attiva**.

    >**Nota**: quando si attiva un ruolo in PIM, possono essere richiesti fino a 10 minuti prima che l'attivazione diventi effettiva. 
    
    >**Nota**: quando l'assegnazione di ruolo è attiva, il browser verrà aggiornato. In caso di problemi, è sufficiente disconnettersi e accedere di nuovo al portale di Azure usando l'account utente **aaduser2**.

1. Tornare nel pannello **Azure AD Privileged Identity Management** e fare clic su **Ruoli personali** nella sezione **Attività**.

1. Nel pannello **Ruoli personali \| Ruoli di Azure AD** passare alla scheda **Assegnazioni attive**. Si noti che il ruolo **Amministratore fatturazione** è **Attivato**.

    >**Nota**: dopo l'attivazione, un ruolo viene disattivato automaticamente quando viene raggiunto il limite di tempo definito in **Ora di fine** (durata dell'idoneità).

    >**Nota**: se si completano le attività di amministratore in anticipo, è possibile disattivare un ruolo manualmente.

1.  Nell'elenco **Assegnazioni attive**, fare clic sul collegamento **Disattiva** nella riga che rappresenta il ruolo Amministratore fatturazione.

1.  Nel pannello **Disattiva - Amministratore fatturazione** fare di nuovo clic su **Disattiva** per confermare.


#### <a name="task-2-activate-a-role-that-requires-approval"></a>Attività 2: Attivare un ruolo che richiede l'approvazione. 

In questa attività si attiverà un ruolo che richiede l'approvazione.

1. Nella finestra del browser InPrivate, mentre si è connessi al portale di Azure come utente **aaduser2**, tornare nel pannello **Privileged Identity Management \| Avvio rapido**. 

1. Nel pannello **Privileged Identity Management \| Avvio rapido** fare clic su **Ruoli personali** nella sezione **Gestisci**.

1. Nel pannello **Ruoli personali \| Ruoli di Azure AD** fare clic su **Attiva** nella riga che visualizza **Ruolo con autorizzazioni di lettura globali** dell'elenco **Assegnazioni idonee**. 

1. Nella casella di testo **Motivo** del pannello **Attiva - Ruolo con autorizzazioni di lettura globali** digitare un testo che giustifichi l'attivazione e quindi fare clic su **Attiva**.

1. Fare clic sull'icona **Notifiche** sulla barra degli strumenti del portale di Azure e visualizzare la notifica che informa che la richiesta è in attesa di approvazione.

    >**Nota**: come amministratore ruolo con privilegi è possibile esaminare e annullare le richieste in qualsiasi momento. 

1. Nel pannello **Ruoli personali \| Ruoli di Azure AD** individuare il ruolo **Amministratore della sicurezza** e fare clic su **Attiva**. 

1. Fare clic sul messaggio di avviso **È necessaria una verifica aggiuntiva. Fare clic per continuare**. 

1. Seguire le istruzioni per verificare la propria identità.

    >**Nota**: è necessario eseguire l'autenticazione una sola volta per sessione. 

1. Tornare nell'interfaccia del portale di Azure, quindi nel pannello **Attiva - Amministratore della sicurezza** digitare un testo che giustifichi l'attivazione nella casella di testo **Motivo** e fare clic su **Attiva**.

    >**Nota**: verrà completato il processo di approvazione automatica.

1. Nel pannello **Ruoli personali \| Ruoli di Azure AD** fare clic sulla scheda **Assegnazioni attive** e notare che l'elenco di **assegnazioni attive** include il ruolo **Amministratore della sicurezza** ma non **Ruolo con autorizzazioni di lettura globale**.

    >**Nota**: ora si approverà il Ruolo con autorizzazioni di lettura globali.

1. Disconnettersi dal portale di Azure come **aaduser2**.

1. Accedere al portale di Azure come **aaduser3**.

    >**Nota**: se si verificano problemi con l'autenticazione usando uno degli account utente, è possibile accedere al tenant di Azure AD usando il proprio account utente per reimpostare le password o riconfigurare le opzioni di accesso.

1. Nel portale di Azure passare a **Azure AD Privileged Identity Management**.

1. Nel pannello **Privileged Identity Management \| Avvio rapido** fare clic su **Approva richieste** nella sezione **Attività**.

1. Nel pannello **Approva richieste \| Ruoli di Azure AD**, nella sezione **Richieste di attivazioni del ruolo**, selezionare alla casella di controllo della voce che rappresenta la richiesta di attivazione per **Ruolo con autorizzazioni di lettura globali** da parte di **aaduser2**.

1. Scegliere **Approva**. Nella casella di testo **Giustificazione** del pannello **Approva richiesta** digitare un motivo per l'attivazione, prendere nota delle ore di inizio e di fine e quindi fare clic su **Conferma**. 

    >**Nota**. è anche disponibile l'opzione per rifiutare le richieste.

1. Disconnettersi dal portale di Azure come **aaduser3**.

1. Accedere al portale di Azure come **aaduser2**

1. Nel portale di Azure passare a **Azure AD Privileged Identity Management**.

1. Nel pannello **Privileged Identity Management \| Avvio rapido** fare clic su **Ruoli personali** nella sezione **Attività**.

1. Nel pannello **Ruoli personali \| Ruoli di Azure AD** fare clic sulla scheda **Assegnazioni attive** e verificare che il Ruolo con autorizzazioni di lettura globali è ora attivo.

    >**Nota**: potrebbe essere necessario aggiornare la pagina per visualizzare l'elenco aggiornato delle assegnazioni attive.

1. Disconnettersi e chiudere la finestra del browser InPrivate.

> Risultato: ci si è esercitati con l'attivazione dei ruoli di PIM con e senza approvazione. 

### <a name="exercise-3---create-an-access-review-and-review-pim-auditing-features"></a>Esercizio 3: Creare una verifica di accesso ed esaminare le funzionalità di controllo di PIM

#### <a name="estimated-timing-10-minutes"></a>Tempo stimato: 10 minuti

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Configurare gli avvisi di sicurezza per i ruoli della directory Azure AD in PIM
- Attività 2: Esaminare avvisi di PIM, le informazioni di riepilogo e le informazioni di controllo dettagliate

#### <a name="task-1-configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Attività 1: Configurare gli avvisi di sicurezza per i ruoli della directory Azure AD in PIM

In questa attività si ridurranno i rischi associati ad assegnazioni di ruoli "obsolete". A tale scopo, si creerà una verifica di accesso di PIM per assicurarsi che i ruoli assegnati siano ancora validi. In particolare, verrà esaminato il Ruolo con autorizzazioni di lettura globali. 

1. Accedere al portale di Azure **`https://portal.azure.com/`** con il proprio account.

    >**Nota**: assicurarsi di aver eseguito l'accesso al tenant di Azure AD **AdatumLab500-04**. È possibile usare il filtro **Directory e sottoscrizione** per passare da un tenant di Azure AD a un altro. Assicurarsi di aver eseguito l'accesso come utente con il ruolo di amministratore globale.
    
    >**Nota**: se la voce AdatumLab500-04 non è ancora visualizzata, fare clic sul collegamento Cambia directory, selezionare la riga AdatumLab500-04 e fare clic sul pulsante Cambia.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Azure AD Privileged Identity Management** e premere **INVIO**.

1. Nel portale di Azure passare al pannello **Azure AD Privileged Identity Management**. 

1. Nel pannello **Privileged Identity Management \| Avvio rapido** fare clic su **Ruoli di Azure AD** nella sezione **Attività**.

1. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Verifiche di accesso** nella sezione **Gestisci**.

1. Nel pannello **AdatumLab500-04 \| Verifiche di accesso** fare clic su **Nuovo**:

1. Nel pannello **Crea una verifica di accesso** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre: 

   |Impostazione|Valore|
   |---|---|
   |Nome della verifica|**Global Reader Review**|
   |Data di inizio|La data odierna| 
   |Frequenza|**Una volta**|
   |Data di fine|La fine del mese corrente|
   |Verifica l'appartenenza ai ruoli|**Ruolo con autorizzazioni di lettura globali**|
   |Revisori|**Utenti selezionati**|
   |Selezionare i revisori|Il proprio account|

1. Nel pannello **Crea una verifica di accesso** fare clic su **Avvia**:
 
    >**Nota**: è necessario circa un minuto prima che la verifica venga distribuita e visualizzata nel pannello **AdatumLab500-04 \| Verifiche di accesso**. Potrebbe essere necessario aggiornare la pagina Web. Lo stato della verifica sarà **Attivo.** 

1. Nel pannello **AdatumLab500-04 \| Verifiche di accesso** fare clic sulla voce **Ruolo con autorizzazioni di lettura globali** in **Verifica Ruolo con autorizzazioni di lettura globali**. 

1. Nel pannello **Verifica Ruolo con autorizzazioni di lettura globali** esaminare la pagina **Panoramica** e notare che i grafici **Stato** mostrano un singolo utente nella categoria **Verifica non effettuata**. 

1. Nella sezione **Gestisci** del pannello **Verifica Ruolo con autorizzazioni di lettura globali** fare clic su **Risultati**. Si noti che è indicato che aaduser2 ha accesso a questo ruolo.

1. Fare clic su **Visualizza** nella riga **aaduser2** per visualizzare un log di controllo dettagliato con voci che rappresentano le attività di PIM che coinvolgono tale utente.

1. Tornare nel pannello **AdatumLab500-04 \| Verifiche di accesso**.

1. Nel pannello **AdatumLab500-04 \| Verifiche di accesso** fare clic su **Verifica l'accesso** nella sezione **Attività** e quindi fare clic sulla voce **Global Reader Review**. 

1. Nel pannello **Verifica Ruolo con autorizzazioni di lettura globali** fare clic sulla voce **aaduser2**. 

1. Nella casella di testo **Motivo** digitare una giustificazione per l'approvazione e quindi fare clic su **Approva** per mantenere l'appartenenza al ruolo corrente oppure su **Nega** per revocarla. 

1. Tornare nel pannello **Azure AD Privileged Identity Management** e fare clic su **Ruoli di Azure AD** nella sezione **Gestisci**.

1. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Verifiche di accesso** nella sezione **Gestisci**.

1. Selezionare la voce che rappresenta la verifica di **Ruolo con autorizzazioni di lettura globali**. Si noti che il grafico **Stato** è stato aggiornato per mostrare la verifica. 

#### <a name="task-2-review-pim-alerts-summary-information-and-detailed-audit-information"></a>Attività 2: Esaminare avvisi di PIM, le informazioni di riepilogo e le informazioni di controllo dettagliate. 

In questa attività si esamineranno gli avvisi di PIM, le informazioni di riepilogo e le informazioni di controllo dettagliate. 

1. Tornare nel pannello **Azure AD Privileged Identity Management** e fare clic su **Ruoli di Azure AD** nella sezione **Gestisci**.

1. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Avvisi**  nella sezione **Gestisci** e quindi su **Impostazione**.

1. Nel pannello **Impostazioni avviso** esaminare gli avvisi preconfigurati e i livelli di rischio. Fare clic su una delle voci per informazioni più dettagliate. 

1. Tornare nel pannello **AdatumLab500-04 \| Avvio rapido** e fare clic su **Panoramica**. 

1. Nel pannello **AdatumLab500-04 \| Panoramica** esaminare le informazioni di riepilogo sulle attivazioni dei ruoli, le attività di PIM, gli avvisi e le assegnazioni di ruoli.

1. Nel pannello **AdatumLab500-04 \| Panoramica** fare clic su **Controllo delle risorse** nella sezione **Attività**. 

    >**Nota**: la cronologia dei controlli è disponibile per tutte le assegnazioni e le attivazioni di ruoli con privilegi eseguite negli ultimi 30 giorni.

1. Si noti che è possibile recuperare informazioni dettagliate, tra cui **Ora**, **Richiedente**, **Azione**, **Nome della risorsa**, **Ambito**, **Destinazione primaria** e **Oggetto**. 

> Risultato: è stata configurata una verifica di accesso e sono state esaminate le informazioni di controllo. 

**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti. 

1. Nel portale di Azure impostare il filtro **Directory e sottoscrizione** sul tenant di Azure AD associato alla sottoscrizione di Azure in cui è stata distribuita la macchina virtuale di Azure **az500-04-vm1**.

>**Nota**: se la voce del tenant di Azure AD principale non è visualizzata, fare clic sul collegamento Cambia directory, selezionare la riga del tenant principale e fare clic sul pulsante Cambia.

1. Nel portale di Azure aprire Cloud Shell facendo clic sulla prima icona nell'angolo in alto a destra. Se richiesto, fare clic su **PowerShell** e su **Crea risorsa di archiviazione**.

1. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

1. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire quanto segue per rimuovere il gruppo di risorse creato nel lab precedente:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB04" -Force -AsJob
    ```

1.  Chiudere il riquadro **Cloud Shell**. 

1. Tornare nel portale di Azure e usare il filtro **Directory e sottoscrizione** per passare al tenant di Azure Active Directory **AdatumLab500-04**.

1. Passare al pannello **Azure Active Directory Premium P2 - Utenti con licenza**, selezionare gli account utente a cui sono state assegnate le licenze, fare clic su **Rimuovi licenza** e, quando richiesto, selezionare **Sì** per confermare.

1. Nel portale di Azure passare al pannello **Utenti - Tutti gli utenti**, fare clic sulla voce che rappresenta l'account utente **aaduser1**, quindi nel pannello **aaduser1 - Profilo** fare clic su **Elimina** e, quando richiesto, selezionare **OK** per confermare.

1. Ripetere la stessa sequenza di passaggi per eliminare gli account utente rimanenti creati.

1. Passare al pannello **AdatumLab500-04 - Proprietà** del tenant di Azure AD, fare clic su **Gestisci tenant** e quindi nella schermata successiva fare clic su **Elimina tenant**. Nel pannello **Elimina directory 'AdatumLab500-04'** fare clic sul collegamento **Ottieni l'autorizzazione per eliminare le risorse di Azure**. Nel pannello **Proprietà** del tenant di Azure Active Directory impostare **Gestione degli accessi per le risorse di Azure** su **Sì** e fare clic su **Salva**.

1. Disconnettersi dal portale di Azure e accedere di nuovo. 

1. Tornare nel pannello **Elimina directory 'AdatumLab500-04'** e fare clic su **Elimina**.

> Per altre informazioni su questa attività, vedere [https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto)
