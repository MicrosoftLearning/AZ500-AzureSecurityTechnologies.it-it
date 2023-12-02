---
lab:
  title: 03 - Microsoft Entra Privileged Identity Management
  module: Module 01 - Manage Identity and Access
---

# Lab 03: Microsoft Entra Privileged Identity Management
# Manuale del lab per gli studenti

## Scenario laboratorio

È stato chiesto di creare un modello di verifica che usi Microsoft Entra Privileged Identity Management (PIM) per abilitare l'amministrazione JIT e controllare il numero di utenti che possono eseguire operazioni con privilegi. I requisiti specifici sono:

- Creare un'assegnazione permanente dell'utente aaduser2 Microsoft Entra ID al ruolo Security Amministrazione istrator. 
- Configurare l'utente aaduser2 Microsoft Entra ID in modo che sia idoneo per i ruoli Fatturazione Amministrazione istrator e Lettore globale.
- Configurare l'attivazione del ruolo Lettore globale per richiedere l'approvazione dell'utente aaduser3 Microsoft Entra ID
- Configurare una verifica di accesso del Ruolo con autorizzazioni di lettura globali ed esaminare le funzionalità di controllo.

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

> Prima di procedere, assicurarsi di aver completato Lab 02: MFA, accesso condizionale e Microsoft Entra Identity Protection. Saranno necessari il tenant di Microsoft Entra, AdatumLab500-04 e gli account utente aaduser1, aaduser2 e aaduser3.

## Obiettivi del lab

In questo lab verranno completati gli esercizi seguenti:

- Esercizio 1: Configurare utenti e ruoli di PIM.
- Esercizio 2: Attivare i ruoli di PIM con e senza approvazione.
- Esercizio 3: Creare una verifica di accesso ed esaminare le funzionalità di controllo di PIM.

## Diagramma di Microsoft Entra Privileged Identity Management

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/aef34a22-8ebd-4015-a04a-7ac3c357b862)

## Istruzioni

### Esercizio 1: Configurare utenti e ruoli di PIM

#### Tempo stimato: 15 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Rendere un utente idoneo per un ruolo.
- Attività 2: Configurare un ruolo in modo che richieda l'approvazione per attivare e aggiungere un membro idoneo.
- Attività 3: Assegnare a un utente un'assegnazione permanente a un ruolo. 

#### Attività 1: Rendere un utente idoneo per un ruolo

In questa attività si renderà un utente idoneo per un ruolo MICROSOFT Entra ID.

1. Accedere al portale di Azure all'indirizzo **`https://portal.azure.com/`**.

    >**Nota**: assicurarsi di aver eseguito l'accesso **al tenant di Microsoft Entra ad AdatumLab500-04** . È possibile usare il **filtro directory e sottoscrizione** per passare dai tenant di Microsoft Entra. Assicurarsi di aver eseguito l'accesso come utente con il ruolo di amministratore globale.
    
    >**Nota**: se la voce AdatumLab500-04 non è ancora visualizzata, fare clic sul collegamento Cambia directory, selezionare la riga AdatumLab500-04 e fare clic sul pulsante Cambia.

2. Nella casella di testo Cerca risorse, servizi e documenti nella **parte superiore della pagina **portale di Azure portale di Azure digitare Microsoft Entra Privileged Identity Management** e premere **INVIO****.

3. Nel pannello **Privileged Identity Management** fare clic su **Ruoli** ID Di Microsoft Entra nella **sezione Gestione**.

4. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Ruoli** nella sezione **Gestisci**.

5. Nel pannello **AdatumLab500-04 \| Ruoli** fare clic su **+ Aggiungi assegnazioni**.

6. Nel pannello **Aggiungi assegnazioni** fare clic su **Amministratore fatturazione** nell'elenco a discesa **Seleziona ruolo**.

7. Fare clic sul collegamento **Nessun membro selezionato**, fare clic su **aaduser2** nel pannello **Selezionare un membro** e quindi fare clic su **Seleziona**.

8. Nel pannello **Aggiungi assegnazioni** fare clic su **Avanti**. 

9. Verificare che l'opzione **Tipo di assegnazione** sia impostata su **Idoneo** e fare clic su **Assegna**.
 
10. Nel pannello **AdatumLab500-04 \| Ruoli** fare clic su **Assegnazioni** nella sezione **Gestisci**.

11. Nel pannello **AdatumLab500-04 \| Assegnazioni** notare le schede **Assegnazioni idonee**, **Assegnazioni attive** e **Assegnazioni scadute**.

12. Nella scheda **Assegnazioni idonee** verificare che **aaduser2** sia visualizzato come **Amministratore fatturazione**. 

    >**Nota**: durante l'accesso, aaduser2 sarà idoneo per l'uso del ruolo Amministratore fatturazione. 

#### Attività 2: Configurare un ruolo in modo che richieda l'approvazione per attivare e aggiungere un membro idoneo

1. Nel portale di Azure tornare al **pannello Privileged Identity Management** e fare clic su **Ruoli** ID di Microsoft Entra.

2. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Ruoli** nella sezione **Gestisci**.

3. Nel pannello **AdatumLab500-04 \| Ruoli** fare clic sulla voce **Ruolo con autorizzazioni di lettura globali**. 

4. **Nel pannello Assegnazioni** con autorizzazioni di lettura \| globali fare clic sull'icona **Impostazioni** ruolo sulla barra degli strumenti del pannello e rivedere le impostazioni di configurazione per il ruolo, inclusi i requisiti di Azure Multi-Factor Authentication.

5. Fare clic su **Modifica**.

6. Nella scheda **Attivazione** abilitare la casella di controllo **Richiedi l'approvazione per l'attivazione**.

7. Fare clic su **Selezionare uno o più responsabili approvazione**, fare clic su **aaduser3** nel pannello **Selezionare un membro** e quindi fare clic su **Seleziona**.

8. Fare clic su **Avanti: Assegnazione**.

9. Deselezionare la casella di controllo **Consenti le assegnazioni idonee permanenti**, lasciando i valori predefiniti per tutte le altre impostazioni.

10. Fare clic su **Avanti: Notifica**.

11. Esaminare le impostazioni in **Notifica**, lasciare l'impostazione predefinita per tutte le opzioni e fare clic su **Aggiorna**.

    >**Nota**: tutti gli utenti che tentano di usare il Ruolo con autorizzazioni di lettura globali avranno ora bisogno dell'approvazione di aaduser3. 

12. Nel pannello **Ruolo con autorizzazioni di lettura globali \| Assegnazioni** fare clic su **+ Aggiungi assegnazioni**.

13. Nel pannello **Aggiungi assegnazioni** fare clic su **Nessun membro selezionato**, fare clic su **aaduser2** nel pannello**Selezionare un membro** e quindi fare clic su **Seleziona**.

14. Fare clic su **Avanti**. 

15. Assicurarsi che l'opzione **Tipo di assegnazione** sia impostata su **Idoneo** ed esaminare le impostazioni di durata dell'idoneità.

16. Fai clic su **Assegna**.

    >**Nota**: l'utente aaduser2 è idoneo per il Ruolo con autorizzazioni di lettura globali. 
 
#### Attività 3: Assegnare a un utente un'assegnazione permanente a un ruolo.

1. Nel portale di Azure tornare al **pannello Privileged Identity Management** e fare clic su **Ruoli** ID di Microsoft Entra.

2. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Ruoli** nella sezione **Gestisci**.

3. Nel pannello **AdatumLab500-04 \| Ruoli** fare clic su **+ Aggiungi assegnazioni**.

4. Nel pannello **Aggiungi assegnazioni** selezionare **Amministratore della sicurezza** nell'elenco a discesa **Seleziona ruolo**.

5. Nel pannello **Aggiungi assegnazioni** fare clic su **Nessun membro selezionato**, fare clic su **aaduser2** nel pannello**Selezionare un membro** e quindi fare clic su **Seleziona**.

6. Fare clic su **Avanti**. 

7. Esaminare le impostazioni di **Tipo di assegnazione** e fare clic su **Assegna**.

8. Nel pannello di spostamento sinistro fare clic su **Assegnazioni**. Nella **scheda Assegnazioni idonee**, in **Sicurezza Amministrazione istrator** selezionare **Aggiorna** per l'assegnazione **aaduser2**. Selezionare **Idoneità permanente** e fare clic su **Salva**.

    >**Nota**: l'utente aaduser2 è ora idoneo in modo permanente per il ruolo Amministratore della sicurezza.
    
### Esercizio 2: Attivare i ruoli di PIM con e senza approvazione

#### Tempo stimato: 15 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Attivare un ruolo che non richiede l'approvazione. 
- Attività 2: Attivare un ruolo che richiede l'approvazione. 

#### Attività 1: Attivare un ruolo che non richiede l'approvazione.

In questa attività si attiverà un ruolo che non richiede l'approvazione.

1. Aprire una finestra del browser InPrivate.

2. Nella finestra del browser InPrivate passare al portale di Azure all'indirizzo ****`https://portal.azure.com/`e accedere usando l'account **utente aaduser2**.

    >**Nota**: per accedere è necessario specificare un nome completo dell'account **utente aaduser2** , incluso il nome di dominio DNS del tenant di Microsoft Entra registrato in precedenza in questo lab. Questo nome utente è nel formato aaduser2@`<your_tenant_name>`.onmicrosoft.com, dove `<your_tenant_name>` è il segnaposto che rappresenta il nome univoco del tenant di Microsoft Entra. 

3. Nella casella di testo Cerca risorse, servizi e documenti nella **parte superiore della pagina **portale di Azure portale di Azure digitare Microsoft Entra Privileged Identity Management** e premere **INVIO****.

4. Nel pannello **Privileged Identity Management** fare clic su **Ruoli personali** nella sezione **Attività**.

5. Verranno visualizzate tre opzioni di **Ruoli idonei** per **aaduser2**: **Ruolo con autorizzazioni di lettura globali**, **Amministratore della sicurezza** e **Amministratore fatturazione**. 

6. Nella riga che visualizza la voce **Amministratore fatturazione** fare clic su **Attiva**.

7. Se necessario, fare clic sull'avviso **È necessaria una verifica aggiuntiva. Fare clic per continuare** e seguire le istruzioni per verificare la propria identità.

8. Nella casella di testo **Motivo** del pannello **Attiva - Amministratore fatturazione** digitare un testo che giustifichi l'attivazione e quindi fare clic su **Attiva**.

    >**Nota**: quando si attiva un ruolo in PIM, possono essere richiesti fino a 10 minuti prima che l'attivazione diventi effettiva. 
    
    >**Nota**: quando l'assegnazione di ruolo è attiva, il browser verrà aggiornato. In caso di problemi, è sufficiente disconnettersi e accedere di nuovo al portale di Azure usando l'account utente **aaduser2**.

9. Tornare al pannello **Privileged Identity Management** e fare clic su **Ruoli personali** nella sezione **Attività**.

10. Nel pannello **Ruoli personali \| di** Microsoft Entra ID passare alla **scheda Assegnazioni** attive. Si noti che il **ruolo Fatturazione Amministrazione istrator** è **Attivato**.

    >**Nota**: dopo l'attivazione, un ruolo viene disattivato automaticamente quando viene raggiunto il limite di tempo definito in **Ora di fine** (durata dell'idoneità).

    >**Nota**: se si completano le attività di amministratore in anticipo, è possibile disattivare un ruolo manualmente.

11.  Nell'elenco **Assegnazioni attive**, fare clic sul collegamento **Disattiva** nella riga che rappresenta il ruolo Amministratore fatturazione.

12.  Nel pannello **Disattiva - Amministratore fatturazione** fare di nuovo clic su **Disattiva** per confermare.


#### Attività 2: Attivare un ruolo che richiede l'approvazione. 

In questa attività si attiverà un ruolo che richiede l'approvazione.

1. Nella finestra del browser InPrivate, mentre si è connessi al portale di Azure come utente **aaduser2**, tornare nel pannello **Privileged Identity Management \| Avvio rapido**. 

2. Nel pannello **Privileged Identity Management \| Avvio rapido** fare clic su **Ruoli personali** nella sezione **Attività**.

3. Nel pannello **Ruoli personali \| di** Microsoft Entra ID, nell'elenco delle **assegnazioni idonee**, nella riga in cui è visualizzato il **ruolo Lettore** globale fare clic su **Attiva**. 

4. Nella casella di testo **Motivo** del pannello **Attiva - Ruolo con autorizzazioni di lettura globali** digitare un testo che giustifichi l'attivazione e quindi fare clic su **Attiva**.

5. Fare clic sull'icona **Notifiche** sulla barra degli strumenti del portale di Azure e visualizzare la notifica che informa che la richiesta è in attesa di approvazione.

    >**Nota**: come amministratore ruolo con privilegi è possibile esaminare e annullare le richieste in qualsiasi momento. 

6. Nel pannello **Ruoli personali \| di** Microsoft Entra ID individuare il **ruolo Sicurezza Amministrazione istrator** e fare clic su **Attiva**. 

7. Se necessario, fare clic sull'avviso **È necessaria una verifica aggiuntiva. Fare clic per continuare** e seguire le istruzioni per verificare la propria identità.

    >**Nota**: è necessario eseguire l'autenticazione una sola volta per sessione. 

8. Tornare nell'interfaccia del portale di Azure, quindi nel pannello **Attiva - Amministratore della sicurezza** digitare un testo che giustifichi l'attivazione nella casella di testo **Motivo** e fare clic su **Attiva**.

    >**Nota**: verrà completato il processo di approvazione automatica.

9. Tornare al **pannello Ruoli personali ruoli** Microsoft \| Entra ID fare clic sulla **scheda Assegnazioni** attive e notare che l'elenco delle **assegnazioni** attive include **Security Amministrazione istrator** ma non il **ruolo Lettore** globale.

    >**Nota**: ora si approverà il Ruolo con autorizzazioni di lettura globali.

10. Disconnettersi dal portale di Azure come **aaduser2**.

11. Nel browser InPrivate accedere al portale di Azure all'indirizzo aaduser3****.**`https://portal.azure.com/`**

    >**Nota**: se si verificano problemi con l'autenticazione usando uno degli account utente, è possibile accedere al tenant di Microsoft Entra usando l'account utente per reimpostare le password o riconfigurare le opzioni di accesso.

12. Nella portale di Azure passare a **Microsoft Entra Privileged Identity Management** (Nella casella di testo Cerca risorse, servizi e documenti nella parte superiore della pagina portale di Azure digitare Microsoft Entra Privileged Identity Management e premere INVIO).

13. Nel pannello **Privileged Identity Management \| Avvio rapido** fare clic su **Approva richieste** nella sezione **Attività**.

14. Nel pannello Approva richieste \| di ruoli** Microsoft Entra ID selezionare **** la casella di controllo relativa alla voce che rappresenta la richiesta di attivazione del ruolo al **ruolo Lettore** globale da **aaduser2**.**

15. Fai clic su **Approva**. Nella casella di testo **Giustificazione** del pannello **Approva richiesta** digitare un motivo per l'attivazione, prendere nota delle ore di inizio e di fine e quindi fare clic su **Conferma**. 

    >**Nota**. è anche disponibile l'opzione per rifiutare le richieste.

16. Disconnettersi dal portale di Azure come **aaduser3**.

17. Nel browser InPrivate accedere al portale di Azure come **`https://portal.azure.com/`** **aaduser2**

18. Nella portale di Azure passare a **Microsoft Entra Privileged Identity Management** (Nella casella di testo Cerca risorse, servizi e documenti nella parte superiore della pagina portale di Azure digitare Microsoft Entra Privileged Identity Management e premere INVIO).

19. Nel pannello **Privileged Identity Management \| Avvio rapido** fare clic su **Ruoli personali** nella sezione **Attività**.

20. Nel pannello **Ruoli personali \| di** Microsoft Entra ID fare clic sulla **scheda Assegnazioni** attive e verificare che il ruolo Lettore globale sia attivo.

    >**Nota**: potrebbe essere necessario aggiornare la pagina per visualizzare l'elenco aggiornato delle assegnazioni attive.

21. Disconnettersi e chiudere la finestra del browser InPrivate.

> Risultato: ci si è esercitati con l'attivazione dei ruoli di PIM con e senza approvazione. 

### Esercizio 3: Creare una verifica di accesso ed esaminare le funzionalità di controllo di PIM

#### Tempo stimato: 10 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Configurare gli avvisi di sicurezza per i ruoli MICROSOFT Entra ID in PIM
- Attività 2: Esaminare avvisi di PIM, le informazioni di riepilogo e le informazioni di controllo dettagliate

#### Attività 1: Configurare gli avvisi di sicurezza per i ruoli MICROSOFT Entra ID in PIM

In questa attività si ridurranno i rischi associati ad assegnazioni di ruoli "obsolete". A tale scopo, si creerà una verifica di accesso di PIM per assicurarsi che i ruoli assegnati siano ancora validi. In particolare, verrà esaminato il Ruolo con autorizzazioni di lettura globali. 

1. Accedere al portale di Azure **`https://portal.azure.com/`** con il proprio account.

    >**Nota**: assicurarsi di aver eseguito l'accesso **al tenant di Microsoft Entra ad AdatumLab500-04** . È possibile usare il **filtro directory e sottoscrizione** per passare dai tenant di Microsoft Entra. Assicurarsi di aver eseguito l'accesso come utente con il ruolo di amministratore globale.
    
    >**Nota**: se la voce AdatumLab500-04 non è ancora visualizzata, fare clic sul collegamento Cambia directory, selezionare la riga AdatumLab500-04 e fare clic sul pulsante Cambia.

2. Nella casella di testo Cerca risorse, servizi e documenti nella **parte superiore della pagina **portale di Azure portale di Azure digitare Microsoft Entra Privileged Identity Management** e premere **INVIO****.

3. Passare al pannello **Privileged Identity Management**. 

4. Nel pannello Avvio rapido di Privileged Identity Management \| fare clic su **Ruoli di** Microsoft Entra nella **sezione Gestione**.** **

5. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Verifiche di accesso** nella sezione **Gestisci**.

6. Nel pannello **AdatumLab500-04 \| Verifiche di accesso** fare clic su **Nuovo**:

7. Nel pannello **Crea una verifica di accesso** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre: 

   |Impostazione|valore|
   |---|---|
   |Nome della verifica|**Global Reader Review**|
   |Data di inizio|Data odierna| 
   |Frequenza|**Una volta**|
   |Data di fine|La fine del mese corrente|
   |Ruolo o ruoli con privilegi selezionati|**Ruolo con autorizzazioni di lettura globali**|
   |Revisori|**Utenti selezionati**|
   |Selezionare i revisori|Il proprio account|

8. Nel pannello **Crea una verifica di accesso** fare clic su **Avvia**:
 
    >**Nota**: è necessario circa un minuto prima che la verifica venga distribuita e visualizzata nel pannello **AdatumLab500-04 \| Verifiche di accesso**. Potrebbe essere necessario aggiornare la pagina Web. Lo stato della verifica sarà **Attivo.** 

9. Nel pannello **AdatumLab500-04 \| Verifiche di accesso** fare clic sulla voce **Ruolo con autorizzazioni di lettura globali** in **Verifica Ruolo con autorizzazioni di lettura globali**. 

10. Nel pannello **Verifica Ruolo con autorizzazioni di lettura globali** esaminare la pagina **Panoramica** e notare che i grafici **Stato** mostrano un singolo utente nella categoria **Verifica non effettuata**. 

11. Nella sezione**Gestisci** del pannello **Verifica Ruolo con autorizzazioni di lettura globali** fare clic su **Risultati**. Si noti che è indicato che aaduser2 ha accesso a questo ruolo.

12. Fare clic su **Visualizza** nella riga **aaduser2** per visualizzare un log di controllo dettagliato con voci che rappresentano le attività di PIM che coinvolgono tale utente.

13. Tornare nel pannello **AdatumLab500-04 \| Verifiche di accesso**.

14. Nel pannello **AdatumLab500-04 \| Verifiche di accesso** fare clic su **Verifica l'accesso** nella sezione **Attività** e quindi fare clic sulla voce **Global Reader Review**. 

15. Nel pannello **Verifica Ruolo con autorizzazioni di lettura globali** fare clic sulla voce **aaduser2**. 

16. Nella casella di testo **Motivo** digitare una giustificazione per l'approvazione e quindi fare clic su **Approva** per mantenere l'appartenenza al ruolo corrente oppure su **Nega** per revocarla. 

17. Tornare al **pannello Privileged Identity Management** e, nella **sezione Gestione** , fare clic su **Ruoli** ID Di Microsoft Entra.

18. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Verifiche di accesso** nella sezione **Gestisci**.

19. Selezionare la voce che rappresenta la verifica di **Ruolo con autorizzazioni di lettura globali**. Si noti che il grafico **Stato** è stato aggiornato per mostrare la verifica. 

#### Attività 2: Esaminare avvisi di PIM, le informazioni di riepilogo e le informazioni di controllo dettagliate. 

In questa attività si esamineranno gli avvisi di PIM, le informazioni di riepilogo e le informazioni di controllo dettagliate. 

1. Tornare al **pannello Privileged Identity Management** e, nella **sezione Gestione** , fare clic su **Ruoli** ID Di Microsoft Entra.

2. Nel pannello **AdatumLab500-04 \| Avvio rapido** fare clic su **Avvisi ** nella sezione **Gestisci** e quindi su **Impostazione**.

3. Nel pannello **Impostazioni avviso** esaminare gli avvisi preconfigurati e i livelli di rischio. Fare clic su una delle voci per informazioni più dettagliate. 

4. Tornare nel pannello**AdatumLab500-04 \| Avvio rapido** e fare clic su **Panoramica**. 

5. Nel pannello **AdatumLab500-04 \| Panoramica** esaminare le informazioni di riepilogo sulle attivazioni dei ruoli, le attività di PIM, gli avvisi e le assegnazioni di ruoli.

6. Nel pannello **AdatumLab500-04 \| Panoramica** fare clic su **Controllo delle risorse** nella sezione **Attività**. 

    >**Nota**: la cronologia dei controlli è disponibile per tutte le assegnazioni e le attivazioni di ruoli con privilegi eseguite negli ultimi 30 giorni.

7. Si noti che è possibile recuperare informazioni dettagliate, tra cui **Ora**, **Richiedente**, **Azione**, **Nome della risorsa**, **Ambito**, **Destinazione primaria** e **Oggetto**. 

> Risultato: è stata configurata una verifica di accesso e sono state esaminate le informazioni di controllo. 

**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti. 

1. Nella portale di Azure impostare il **filtro Directory e sottoscrizione** sul tenant di Microsoft Entra associato alla sottoscrizione di Azure in cui è stata distribuita la **macchina virtuale di Azure az500-04-vm1**.

    >**Nota**: se non viene visualizzata la voce principale del tenant di Microsoft Entra, fare clic sul collegamento Cambia diretory, selezionare la riga principale del tenant e fare clic sul pulsante Cambia.

2. Nel portale di Azure aprire Cloud Shell facendo clic sulla prima icona nell'angolo in alto a destra. Se richiesto, fare clic su **PowerShell** e su **Crea risorsa di archiviazione**.

3. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

4. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire quanto segue per rimuovere il gruppo di risorse creato nel lab precedente:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB04" -Force -AsJob
    ```

5. Chiudere il riquadro **Cloud Shell**. 

6. Tornare alla portale di Azure, usare il **filtro Directory e sottoscrizione** per passare al **tenant AdatumLab500-04** AMicrosoft Entra.

7. Passare al **pannello AdatumLab500-04 Microsoft Entra** e, nella **sezione Gestisci** , fare clic su **Licenze**.

8. **Nelle licenze** | Pannello Panoramica, fare clic su **Tutti i prodotti**, selezionare la casella di controllo Microsoft **Entra ID P2** e fare clic su di essa per aprire.

    >**Nota**: in Lab 2 - Esercizio 2 - Attività 4 **Assegnare licenze microsoft Entra ID P2 agli utenti** di Microsoft Entra era di assegnare le licenze Premium agli utenti **aaduser1, aaduser2 e aaduser3**, assicurarsi di rimuovere tali licenze dagli utenti assegnati

9. Nel pannello **Microsoft Entra ID P2 - Utenti con** licenza selezionare le caselle di controllo degli account utente a cui sono state assegnate **licenze microsoft Entra ID P2** . Fare clic su **Rimuovi licenza** nel riquadro superiore e, quando richiesto, selezionare **Sì** per confermare.

10. Nel portale di Azure passare al pannello **Utenti - Tutti gli utenti**, fare clic sulla voce che rappresenta l'account utente **aaduser1**, quindi nel pannello **aaduser1 - Profilo** fare clic su **Elimina** e, quando richiesto, selezionare **OK** per confermare.

11. Ripetere la stessa sequenza di passaggi per eliminare gli account utente rimanenti creati.

12. Passare al **pannello AdatumLab500-04 - Panoramica** del tenant di Microsoft Entra, selezionare **Gestisci tenant** e quindi nella schermata successiva selezionare la casella di controllo accanto ad **AdatumLab500-04** e selezionare **Elimina**. **Nel pannello Elimina tenant 'AdatumLab500-04'** selezionare **Ottieni l'autorizzazione per eliminare il **collegamento Risorse di Azure** nel pannello Proprietà** di AMicrosoft Entra ID, impostare **Gestione degli accessi per le risorse** di Azure su **Sì** e selezionare **Salva**.

13. Disconnettersi dal portale di Azure e accedere di nuovo. 

14. Tornare nel pannello **Elimina directory 'AdatumLab500-04'** e fare clic su **Elimina**.

    >**Nota**: se ancora non è possibile eliminare il tenant e viene generato un errore **Elimina tutte le sottoscrizioni basate su licenza**, il problema potrebbe essere dovuto a sottoscrizioni collegate al tenant. In questo caso **la licenza** P2 gratuita potrebbe generare l'errore di convalida. L'eliminazione della sottoscrizione di valutazione della licenza P2 usando l'ID amministratore globale dall'amministratore di M365>> **I tuoi prodotti** e dal portale di **Business Store** risolve questo problema. Si noti anche che l'eliminazione del tenant richiede più tempo. Controllare la data di fine dell'abbonamento, una volta dopo la fine del periodo di valutazione, rivedere l'ID Microsoft Entra e quindi provare a eliminare il tenant.    

> Per altre informazioni su questa attività, vedere [https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto)
