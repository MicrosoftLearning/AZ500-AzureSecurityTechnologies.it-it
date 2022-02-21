---
lab:
  title: 02 - Criteri di Azure
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: d530ee043d5cf56d6f805e597d7a35eee47b3d5e
ms.sourcegitcommit: 4a94ae2382fc99dda007add73148dd4108227ab1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2022
ms.locfileid: "137818159"
---
# <a name="lab-02-azure-policy"></a>Lab 02 - Criteri di Azure
# <a name="student-lab-manual"></a>Manuale del lab per gli studenti

## <a name="lab-scenario"></a>Scenario del lab

È stato chiesto di creare un modello di verifica che mostra come è possibile usare Criteri di Azure. In particolare, è necessario:

- Creare un criterio Località consentite che garantisce che le risorse vengano create solo in un'area specifica.
- Eseguire test per verificare che le risorse vengano create solo nella località consentita

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## <a name="lab-objectives"></a>Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Implementare Criteri di Azure. 

### <a name="exercise-1-implement-azure-policy"></a>Esercizio 1: Implementare Criteri di Azure

#### <a name="estimated-timing-20-minutes"></a>Tempo stimato: 20 minuti

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Creare un gruppo di risorse di Azure. 
- Attività 2: Creare un'assegnazione del criterio Località consentite.
- Attività 3: Verificare che l'assegnazione del criterio Località consentite funzioni. 

#### <a name="task-1-create-a-resource-group-for-the-lab"></a>Attività 1: Creare un gruppo di risorse per il lab. 

In questa attività si creerà un gruppo di risorse per il lab. 

1. Accedere al portale di Azure **`https://portal.azure.com/`** .

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

1. Aprire Cloud Shell facendo clic sulla prima icona in alto a destra nel portale di Azure. Se richiesto, selezionare **PowerShell** e **Crea risorsa di archiviazione**.

1. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

1. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per creare un gruppo di risorse (verificare con il docente il valore del parametro location):

    ```powershell
    New-AzResourceGroup -Name AZ500LAB02 -Location 'East US'
    ```

1. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per elencare i gruppi di risorse in modo da verificare che il nuovo gruppo di risorse sia stato creato:

    ```powershell
    Get-AzResourceGroup | format-table
    ```

1. Chiudere **Cloud Shell**.

#### <a name="task-2-create-an-allowed-locations-policy-assignment"></a>Attività 2: Creare un'assegnazione del criterio Località consentite.

In questa attività si creerà un'assegnazione del criterio Località consentite e si specificheranno le aree di Azure che possono essere usate dal criterio. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Criteri** e premere **INVIO**.

1. Nella sezione **Creazione** nel pannello **Criteri** selezionare **Definizioni**.

1. Dedicare un minuto a esaminare le definizioni predefinite. Usare l'elenco a discesa **Categoria** per filtrare l'elenco dei criteri.

1. Nella casella di testo **Cerca** digitare **Località consentite**. 

   >**Nota**: il criterio **Località consentite** consente di limitare le località delle risorse, non dei gruppi di risorse. Per limitare le località dei gruppi di risorse, è possibile usare il criterio **Località consentite per i gruppi di risorse**.

1. Fare clic sulla definizione del criterio **Località consentite** per visualizzarne i dettagli. 

   >**Nota**: la definizione di questo criterio accetta una gamma di località come parametri. Una regola dei criteri è un'istruzione "if-then". La clausola "if" controlla se la località della risorsa è inclusa nell'elenco di parametri e, in caso contrario, la clausola "then" nega la creazione della risorsa oppure, per le risorse esistenti, le contrassegna come non conformi.

1. Nel pannello **Località consentite** fare clic su **Assegna**.

1. Nella scheda **Informazioni di base** del pannello **Località consentite** fare clic sul pulsante con i puntini di sospensione (...) accanto alla casella di testo **Ambito** e quindi specificare le impostazioni seguenti nel pannello **Ambito**:

   |Impostazione|Valore|
   |---|---|
   |Subscription|Nome della propria sottoscrizione di Azure|
   |Resource group|**AZ500LAB02**|

1. Fare clic su **Seleziona**.

1. Nella scheda **Informazioni di base** del pannello **Località consentite** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

   |Impostazione|Valore|
   |---|---|
   |Nome dell'assegnazione|**Consenti Regno Unito meridionale per AZ500LAB02**|
   |Descrizione|**Consenti la creazione di risorse in Regno Unito meridionale solo per AZ500LAB02**|
   |Imposizione dei criteri|**Enabled**|

1. Fare clic su **Avanti**.

1. Nella scheda **Parametri** del pannello **Località consentite** selezionare **Regno Unito meridionale** come unica località consentita nell'elenco a discesa **Località consentite**. 

   >**Nota**: è possibile selezionare più di una località. Se il criterio richiede un set diverso di parametri, questa scheda fornisce le opzioni necessarie. 

1. Fare clic su **Rivedi e crea** e quindi su **Crea** per creare l'assegnazione di criteri. 

   >**Nota**: verrà visualizzata una notifica che indica che l'assegnazione è riuscita e che il completamento può richiedere circa 30 minuti.

   >**Nota**: il motivo per cui l'assegnazione di criteri di Azure può richiedere fino a 30 minuti è che deve essere replicata a livello globale. In genere questa operazione richiede solo pochi minuti.  Se l'attività successiva non riesce, attendere alcuni minuti e riprovare.

#### <a name="task-3-test-the-allowed-locations-policy-assignment"></a>Attività 3: Testare l'assegnazione del criterio Località consentite

In questa attività verrà testata l'assegnazione del criterio Località consentite. 

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Reti virtuali** e premere **INVIO**.

1. Nel pannello **Reti virtuali** fare clic su **+ Crea**.

   >**Nota**: prima di tutto, si tenterà di creare una rete virtuale negli Stati Uniti orientali. Poiché questa non è una località consentita, la richiesta verrà bloccata. 

1. Nella scheda **Informazioni di base** del pannello **Crea rete virtuale** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |---|---|
    |Resource group|**AZ500LAB02**|
    |Nome|**myVnet**|
    |Region|**(Stati Uniti) Stati Uniti orientali**|

1. Fare clic su **Rivedi e crea**. 

1. Nella scheda **Rivedi e crea** del pannello **Crea rete virtuale** notare il messaggio **Convalida non riuscita**. 

    > **Nota**: se l'avviso **Convalida non riuscita** non viene visualizzato, fare clic su **Indietro** e attendere ancora alcuni minuti.

1. Nella scheda **Informazioni di base** fare clic sul collegamento del messaggio di errore per aprire il pannello **Assegnazione criteri**. Verrà visualizzata l'assegnazione dei criteri che limita la posizione.

1. Chiudere il pannello **Assegnazione criteri**, nel pannello **Crea rete virtuale** fare clic sulla scheda **Informazioni di base** e nel menu a discesa **Area** selezionare **(Europa) Regno Unito meridionale**.

1. Fare clic su **Rivedi e crea**, verificare che la convalida sia riuscita, fare clic su **Crea** e verificare che la rete virtuale sia stata creata. 

> Risultati dell'esercizio: in questo esercizio si è appreso come applicare un criterio di Azure selezionando le definizioni di un criterio predefinito e assegnando il criterio a un gruppo di risorse.

**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti.

1. Nel portale di Azure aprire Cloud Shell facendo clic sulla prima icona nell'angolo in alto a destra. Se richiesto, fare clic su **Riconnetti**.

1. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il gruppo di risorse creato in questo lab:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB02" -Force -AsJob
    ```

1.  Chiudere il riquadro **Cloud Shell**. 
