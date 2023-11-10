---
lab:
  title: 03 - Configurare il tenant per ID verificato di Microsoft Entra
  module: Module 01 - Manage Identity and Access
---
# Lab 03: Configurare il tenant per ID verificato di Microsoft Entra
# Manuale del lab per gli studenti

## Scenario laboratorio

# Configurare il tenant per ID verificato di Microsoft Entra

>[!Note] 
> Le credenziali verificabili di Azure Active Directory sono ora ID verificato di Microsoft Entra e fanno parte della famiglia di prodotti Microsoft Entra. Altre informazioni sulla [famiglia](https://aka.ms/EntraAnnouncement) di soluzioni di identità Microsoft Entra e introduzione all'interfaccia [](https://entra.microsoft.com)di amministrazione unificata di Microsoft Entra.

ID verificato di Microsoft Entra è una soluzione di identità decentralizzata che consente di proteggere l'organizzazione. Il servizio consente di rilasciare e verificare le credenziali. Le autorità emittenti possono usare il servizio ID verificato per rilasciare le proprie credenziali verificabili personalizzate. I verificatori possono usare l'API REST gratuita del servizio per richiedere e accettare facilmente le credenziali verificabili nelle app e nei servizi. In entrambi i casi, il tenant di Azure AD deve essere configurato per rilasciare le proprie credenziali verificabili o verificare la presentazione delle credenziali verificabili di un utente rilasciate da terze parti. Nel caso in cui si sia un emittente e un verificatore, è possibile usare un singolo tenant di Azure AD per rilasciare le proprie credenziali verificabili e verificare quelle di altri utenti.

Questa esercitazione descrive come configurare il tenant di Azure AD per usare il servizio credenziali verificabili.

In particolare, si apprenderà come:

> - Creare un'istanza di Azure Key Vault.
> - Configurare il servizio ID verificato.
> - Registrare un'applicazione in Azure AD.
> - Verificare la proprietà del dominio per l'identificatore decentralizzato (DID)

Il diagramma seguente illustra l'architettura di ID verificato e il componente configurato.

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/8d4d01c2-3110-421a-91a8-7b052bc8d793)


## Prerequisiti

- Assicurarsi di avere l'amministratore globale o l'autorizzazione di amministratore dei criteri di autenticazione per la directory che si vuole configurare. Se non si è l'amministratore globale, è necessaria l'autorizzazione dell'amministratore dell'applicazione per completare la registrazione dell'app, inclusa la concessione del consenso amministratore.
- Assicurarsi di avere il ruolo di collaboratore per la sottoscrizione di Azure o il gruppo di risorse in cui si distribuisce Azure Key Vault.

## Creare un insieme di credenziali delle chiavi

Azure Key Vault è un servizio cloud che consente l'archiviazione sicura e l'accesso di segreti e chiavi. Il servizio ID verificato archivia le chiavi pubbliche e private in Azure Key Vault. Queste chiavi vengono usate per firmare e verificare le credenziali.

### Usare il portale di Azure per creare un insieme di credenziali delle chiavi di Azure.

1. Avviare una sessione del browser e accedere al [menu portale di Azure.](https://portal.azure.com/)
   
2. Nella casella di ricerca portale di Azure immettere **Key Vault.**

3. Nell'elenco dei risultati scegliere **Key Vault.**

4. Nella sezione Insiemi di credenziali delle chiavi scegliere **Crea.**

5. Nella **scheda Informazioni di base** di Crea un insieme di **credenziali delle chiavi** immettere o selezionare queste informazioni:
   
   |Impostazione|Valore|
   |---|---|
   |**Dettagli di progetto**|
   |Subscription|Selezionare la propria sottoscrizione.|
   |Gruppo di risorse|Immettere **azure-rg-1.** seleziona **OK**.|
   |**Dettagli istanza**|
   |Nome insieme di credenziali delle chiavi|Immettere **Contoso-vault2.**|
   |Area|Selezionare **Stati Uniti orientali**.|
   |Piano tariffario|Standard predefinito **del sistema**|
   |Giorni di conservazione degli insiemi di credenziali eliminati|Impostazione predefinita **del sistema 90**|

6. Selezionare la **scheda** Rivedi e crea oppure selezionare il pulsante blu Rivedi e crea nella parte inferiore della pagina.
  
7. Seleziona **Crea**.

Prendere nota delle due proprietà seguenti:

* **Nome dell'insieme di credenziali**: nell'esempio corrisponde a **Contoso-Vault2**. Questo nome verrà usato per altri passaggi.
* **URI dell'insieme di credenziali: nell'esempio l'URI** dell'insieme di credenziali è `https://contoso-vault2.vault.azure.net/`. Le applicazioni che usano l'insieme di credenziali tramite l'API REST devono usare questo URI.

A questo punto, l'account Azure è l'unico autorizzato a eseguire operazioni su questo nuovo insieme di credenziali.

>[!NOTE]
>Per impostazione predefinita, l'account che crea un insieme di credenziali è l'unico con accesso. Il servizio ID verificato deve accedere all'insieme di credenziali delle chiavi. È necessario configurare l'insieme di credenziali delle chiavi con i criteri di accesso che consentono all'account usato durante la configurazione di creare ed eliminare chiavi. L'account usato durante la configurazione richiede anche le autorizzazioni per firmare in modo che possa creare l'associazione di dominio per ID verificato. Se si usa lo stesso account durante il test, modificare i criteri predefiniti per concedere l'autorizzazione di firma dell'account, oltre alle autorizzazioni predefinite concesse agli autori dell'insieme di credenziali.

### Impostare i criteri di accesso per l'insieme di credenziali delle chiavi

Un insieme di credenziali delle chiavi definisce se un'entità di sicurezza specificata può eseguire operazioni su chiavi e segreti di Key Vault. Impostare i criteri di accesso nell'insieme di credenziali delle chiavi sia per l'account amministratore del servizio ID verificato che per l'entità API del servizio richiesta creata.
Dopo aver creato l'insieme di credenziali delle chiavi, le credenziali verificabili generano un set di chiavi usate per fornire la sicurezza dei messaggi. Queste chiavi vengono archiviate in Key Vault. Si usa un set di chiavi per firmare, aggiornare e ripristinare le credenziali verificabili.

### Impostare i criteri di accesso per l'ID verificato Amministrazione utente

1. Accedi al [portale di Azure](https://portal.azure.com).

2. Passare all'insieme di credenziali delle chiavi usato per questa esercitazione.

3. In **Impostazioni selezionare **Criteri di accesso****.

4. In **Aggiungi criteri di accesso**, in **U edizione Standard R** selezionare l'account usato per seguire questa esercitazione.

5. Per **Autorizzazioni** chiave verificare che siano selezionate le autorizzazioni seguenti: **Get**, **Create**, **Delete** e **Sign**. Per impostazione predefinita, l'opzione **Crea** ed **Elimina** è già abilitata. **Il segno** deve essere l'unica autorizzazione chiave da aggiornare.

      ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/7c8a92ea-24f1-41e6-9656-869e8486af72)


6. Per salvare le modifiche, selezionare **Salva**.

## Configurare l'ID verificato

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/b4b857a2-24b8-4c3f-9f5c-43d23b58427f)


Per configurare l'ID verificato, seguire questa procedura:

1. Accedi al [portale di Azure](https://portal.azure.com).

2. *Cercare ID* verificato. Selezionare **quindi ID** verificato.

3. Nel menu a sinistra selezionare **Setup (Imposta).**

4. Dal menu centrale selezionare **Definisci le impostazioni dell'organizzazione**

5. Configurare l'organizzazione specificando le informazioni seguenti:

    1. **Nome** organizzazione: immettere un nome per fare riferimento all'azienda all'interno degli ID verificati. Questo nome non è visibile ai clienti.

    1. **Dominio** attendibile: immettere un dominio aggiunto a un endpoint di servizio nel documento di identità decentralizzata (DID). Il dominio è ciò che associa il DID a qualcosa di tangibile che l'utente potrebbe conoscere della propria azienda. Microsoft Authenticator e altri portafogli digitali usano queste informazioni per verificare che il DID sia collegato al dominio. Se il portafoglio può verificare il DID, visualizza un simbolo che indica che la verifica è stata eseguita. Se il portafoglio non è in grado di verificare il DID, informa l'utente che le credenziali sono state rilasciate da un'organizzazione che non è stato possibile convalidare.

        >[!IMPORTANT]
        > Il dominio non può essere un reindirizzamento. In caso contrario, l'operazione DID e il dominio non possono essere collegati. Assicurarsi di usare HTTPS per il dominio. Ad esempio: `https://did.woodgrove.com`.

    1. **Insieme di credenziali** delle chiavi: selezionare l'insieme di credenziali delle chiavi creato in precedenza.

    1. In **Avanzate** è possibile scegliere il **sistema** di attendibilità che si vuole usare per il tenant. È possibile scegliere tra **Web** o **ION**. Web significa che il tenant usa [did:web](https://w3c-ccg.github.io/did-method-web/) come metodo did e ION significa che usa [did:ion](https://identity.foundation/ion/).

        >[!IMPORTANT]
        > L'unico modo per modificare il sistema di attendibilità consiste nel rifiutare esplicitamente il servizio ID verificato e ripetere l'onboarding.

1. Seleziona **Salva**.  

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/b191676e-03e3-423f-aa28-1e3d2887ea07)


### Impostare i criteri di accesso per le entità servizio ID verificato

Quando si configura l'ID verificato nel passaggio precedente, i criteri di accesso in Azure Key Vault vengono aggiornati automaticamente per assegnare alle entità servizio l'ID verificato le autorizzazioni necessarie.  
Se è necessario reimpostare manualmente le autorizzazioni, il criterio di accesso dovrebbe essere simile al seguente.

| Entità servizio | AppId | Autorizzazioni chiave |
| -------- | -------- | -------- |
| Servizio credenziali verificabili | bb2a64ee-5d29-4b07-a491-25806dc854d3 | Get, Sign |
| Richiesta di servizio credenziali verificabili | 3db474b9-6a0c-4840-96ac-1fceb342124f | Segno |



![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/896b8ea0-b88b-43b8-a93b-4771defedfde)


## Registrare un'applicazione in Azure AD

L'applicazione deve ottenere i token di accesso quando vuole chiamare ID verificato di Microsoft Entra in modo che possa emettere o verificare le credenziali. Per ottenere i token di accesso, è necessario registrare un'applicazione e concedere l'autorizzazione API per il servizio di richiesta ID verificato. Ad esempio, usare la procedura seguente per un'applicazione Web:

1. Accedere al portale di Azure[ con l'account ](https://portal.azure.com)amministrativo.

1. Se si ha accesso a più tenant, selezionare la directory e la **sottoscrizione**. Cercare quindi e selezionare **Azure Active Directory**.

1. In **Gestisci** selezionare **Registrazioni app** > **Nuova registrazione**.  

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/03ca3d13-5564-4ce2-b42c-f8333bc97fb5)


1. Immettere un nome visualizzato per l'applicazione. Ad esempio: *verificabile-credentials-app*.

1. Per Tipi di account supportati, selezionare Account solo in questa directory organizzativa (Solo directory predefinita - Tenant singolo).For **Supported account types**, select **Accounts in this organizational directory only (Default Directory only - Single tenant)**).

1. Selezionare **Registra** per creare l'applicazione.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/d0a15737-c8a9-4522-990d-1cf6bc12cc1e)


### Concedere le autorizzazioni per ottenere i token di accesso

In questo passaggio si concedono le autorizzazioni all'entità **servizio Richiesta servizio credenziali** verificabili.

Per aggiungere le autorizzazioni necessarie, seguire questa procedura:

1. Rimanere nella **pagina dei dettagli dell'applicazione verificabile-credentials-app** . Seleziona **Autorizzazioni API** > **Aggiungi un'autorizzazione**.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/adf97022-2081-4273-8d61-f2b53628e989)

1. Seleziona **API utilizzate dall'organizzazione**.

1. Cercare l'entità **servizio Richiesta** servizio credenziali verificabili e selezionarla.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/01691eb2-ab7f-4778-9911-342eb9350f99)

1. Scegliere **Autorizzazione** applicazionee espandere **VerificaableCredential.Create.All**.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/3c5e008e-2ba5-417a-90ff-22e8f622ad34)


1. Selezionare **Aggiungi autorizzazioni**.

1. Selezionare **Concedi consenso amministratore per \<your tenant name\>**.

È possibile scegliere di concedere separatamente le autorizzazioni di rilascio e presentazione se si preferisce separare gli ambiti in applicazioni diverse.

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/640def45-e666-423e-bf69-598e8980b125)

## Registrare l'ID decentralizzato e verificare la proprietà del dominio

Dopo l'installazione di Azure Key Vault e il servizio ha una chiave di firma, è necessario completare il passaggio 2 e 3 nella configurazione.

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/40e10177-b025-4d10-b16a-d66c06762c3f)

1. Passare al servizio ID verificato nel portale di Azure.  
1. Nel menu a sinistra selezionare **Setup (Imposta).**
1. Dal menu centrale selezionare **Verifica la proprietà** del dominio.

# Verificare la proprietà del dominio per l'identificatore decentralizzato (DID)

## Verificare la proprietà del dominio e il file did-configuration.json

Il dominio deve essere un dominio sotto il controllo e deve essere nel formato `https://www.example.com/`. 

1. Dal portale di Azure passare alla pagina VerifiedID.

1. Selezionare **Installazione, quindi **Verificare la proprietà** del dominio e scegliere **Verifica** per il dominio**

1. Copiare o scaricare il `did-configuration.json` file.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/342fa12f-2d0d-40cf-a2f9-8796a86f0824)

1. Ospitare il `did-configuration.json` file nel percorso specificato. Esempio: se è stato specificato un dominio `https://www.example.com` , il file deve essere ospitato in questo URL `https://www.example.com/.well-known/did-configuration.json`.
   Nell'URL non può essere presente alcun percorso aggiuntivo diverso dal `.well-known path` nome.

1. Quando è `did-configuration.json` disponibile pubblicamente nell'URL .well-known/did-configuration.json, verificarlo premendo il **pulsante Aggiorna stato** verifica.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/49a06251-af56-49b2-9059-0bd4ca678da6)

> Risultato: è stata creata correttamente un'istanza di Azure Key Vault, è stato configurato il servizio ID verificato, è stata registrata un'applicazione in Azure AD e la proprietà del dominio verificata per l'identificatore decentralizzato (DID).

**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti. 
