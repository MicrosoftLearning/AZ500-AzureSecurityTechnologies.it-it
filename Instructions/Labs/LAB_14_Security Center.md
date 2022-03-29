---
lab:
  title: 14 - Microsoft Defender for Cloud
  module: Module 04 - Microsoft Defender for Cloud
ms.openlocfilehash: 6ec274b75692321577c8966e07349211209eaa02
ms.sourcegitcommit: a8470295248a6363987bd5ea47154fe39f8535c3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/09/2022
ms.locfileid: "139703588"
---
# <a name="lab-14-microsoft-defender-for-cloud"></a>Lab 14: Microsoft Defender for Cloud
# <a name="student-lab-manual"></a>Manuale del lab per studenti

## <a name="lab-scenario"></a>Scenario del lab

È stata ricevuta la richiesta di creare un modello di verifica di un ambiente basato su Microsoft Defender for Cloud. In particolare, sarà necessario:

- Configurare Microsoft Defender for Cloud per monitorare una macchina virtuale.
- Esaminare le raccomandazioni di Microsoft Defender for Cloud per la macchina virtuale.
- Implementare le raccomandazioni per la configurazione guest e l'accesso Just-In-Time alle macchine virtuali. 
- Capire come poter usare il punteggio di sicurezza per determinare lo stato di avanzamento del processo di creazione di un'infrastruttura più sicura.

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## <a name="lab-objectives"></a>Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Implementare Microsoft Defender for Cloud

## <a name="microsoft-defender-for-cloud-diagram"></a>Diagramma di Microsoft Defender for Cloud

![image](https://user-images.githubusercontent.com/91347931/157537800-94a64b6e-026c-41b2-970e-f8554ce1e0ab.png)

## <a name="instructions"></a>Istruzioni

### <a name="exercise-1-implement-microsoft-defender-for-cloud"></a>Esercizio 1: Implementare Microsoft Defender for Cloud

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Configurare Microsoft Defender for Cloud
- Attività 2: Esaminare le raccomandazioni di Microsoft Defender for Cloud
- Attività 3: Implementare la raccomandazione di Microsoft Defender for Cloud per abilitare l'accesso JIT (Just-In-Time) alla macchina virtuale

#### <a name="task-1-configure-microsoft-defender-for-cloud"></a>Attività 1: Configurare Microsoft Defender for Cloud

In questa attività si eseguiranno l'onboarding e la configurazione di Microsoft Defender for Cloud.

1. Accedere al portale di Azure **`https://portal.azure.com/`** .

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Microsoft Defender for Cloud** e premere **INVIO**.

3. Se l'operazione non è stata completata in precedenza, nel pannello **Microsoft Defender for Cloud \| Introduzione** fare clic su **Aggiorna**.
     
4. Se l'operazione non è stata completata in precedenza, nel pannello **Microsoft Defender for Cloud \| Introduzione** scorrere verso il basso nella scheda **Installa agenti** e fare clic su **Installa agenti**.

5. Nel pannello **Microsoft Defender for Cloud \| Introduzione**, nella scheda **Aggiorna** >> nella sezione **Select workspaces with enhanced security features** (Seleziona aree di lavoro con funzionalità di sicurezza avanzate) >> attivare il **piano di Microsoft Defender** selezionando l'area di lavoro Log Analytics. 

    >**Nota**: esaminare tutte le funzionalità disponibili nell'ambito dei piani di Microsoft Defender. 

6. Nel pannello **Piani di Defender** selezionare **Abilitare tutti i piani di Microsoft Defender for Cloud** e fare clic su **Salva**.

7. Passare a **Microsoft Defender for Cloud**  e fare clic su **Impostazioni ambiente** nelle impostazioni di gestione nella barra dei menu verticale a sinistra.

8. Nel pannello **Microsoft Defender for Cloud | Impostazioni ambiente** fare clic sulla sottoscrizione pertinente. 

9. Nel pannello **Impostazioni | Piani di Defender**, nel menu verticale a sinistra fare clic su **Provisioning automatico**.

10. Nel pannello **Impostazioni | Provisioning automatico** verificare che l'opzione Provisioning automatico sia impostata su **Sì** per la prima voce, **Agente di Log Analytics per VM di Azure**.

11. Nel pannello **Impostazioni \| Automazione del flusso di lavoro** fare clic su **+ Aggiungi l'automazione del flusso di lavoro**.

12. Nel pannello **Aggiungi l'automazione del flusso di lavoro** consultare le impostazioni disponibili. 

    >**Nota**: è possibile attivare gli avvisi di rilevamento delle minacce basati sulle azioni e le raccomandazioni di Microsoft Defender for Cloud. È inoltre possibile configurare un'azione basata su app per la logica. 

13. Nel pannello **Aggiungi l'automazione del flusso di lavoro** fare clic su **Annulla**.

    >**Nota**: Microsoft Defender for Cloud offre molte informazioni dettagliate sulle macchine virtuali, tra cui lo stato di aggiornamento del sistema, le configurazioni di sicurezza del sistema operativo e la protezione degli endpoint.

14. Tornare al pannello **Microsoft Defender for Cloud \| Impostazioni ambiente**, espandere la sottoscrizione e fare clic sulla voce che rappresenta l'area di lavoro Log Analytics creata nel lab precedente.

15. Nel pannello **Impostazioni \| Piani di Defender** verificare che l'opzione **Abilitare tutti i piani di Microsoft Defender for Cloud** sia selezionata e fare clic su **Salva**.

16. Selezionare **Raccolta dati** nel riquadro **Microsoft Defender for Cloud \| Impostazioni**. Selezionare **Tutti gli eventi** e **Salva**.


#### <a name="task-2-review-the-microsoft-defender-for-cloud-recommendation"></a>Attività 2: Esaminare la raccomandazione di Microsoft Defender for Cloud

In questa attività si esamineranno le raccomandazioni di Microsoft Defender for Cloud. 

1. Nel portale di Azure tornare al pannello **Microsoft Defender for Cloud \| Panoramica**. 

2. Nel pannello **Microsoft Defender for Cloud \| Panoramica** esaminare il riquadro **Punteggio di sicurezza**.

    >**Nota**: registrare il punteggio corrente, se disponibile.

3. Tornare al pannello **Microsoft Defender for Cloud \| Panoramica** e selezionare **Risorse valutate**.

4. Nel pannello **Inventario** selezionare la voce **myVM**.

    >**Nota**: potrebbe essere necessario attendere alcuni minuti e aggiornare la pagina del browser per poter visualizzare la voce.
    
5. Nel pannello **Integrità delle risorse**, nella scheda **Raccomandazioni**, esaminare l'elenco delle raccomandazioni per **myVM**.


#### <a name="task-3-implement-the-microsoft-defender-for-cloud-recommendation-to-enable-just-in-time-vm-access"></a>Attività 3: Implementare la raccomandazione di Microsoft Defender for Cloud per abilitare l'accesso JIT (Just-In-Time) alla macchina virtuale

In questa attività si implementerà la raccomandazione di Microsoft Defender for Cloud per abilitare l'accesso JIT (Just-In-Time) alla macchina virtuale. 

1. Nel portale di Azure tornare al pannello **Microsoft Defender for Cloud \| Panoramica** e selezionare **Protezioni carico di lavoro** nel riquadro **Sicurezza cloud**.

2. Nel pannello **Protezioni carico di lavoro**, nella sezione **Protezione avanzata** fare clic sul riquadro **Accesso Just-In-Time alla VM** e nel pannello **Accesso Just-In-Time alla VM** fare clic su **Try Just in time VM access** (Prova accesso Just-In-Time alla VM).

    >**Nota**: se le macchine virtuali non sono elencate, passare al pannello **Macchina virtuale**, fare clic su **Configurazione** e selezionare l'opzione **Enable the Just-in-time VMs** (Abilita VM Just-In-Time) in **Accesso Just-In-Time alla VM**. Ripetere il passaggio precedente per tornare a **Microsoft Defender for Cloud** e aggiornare la pagina. Verrà visualizzata la macchina virtuale.

3. Nel pannello **Accesso Just-In-Time alla VM** selezionare **Non configurato** e quindi fare clic sulla voce **myVM**.

    >**Nota**: potrebbe essere necessario attendere alcuni minuti prima che la voce **myVM** diventi disponibile.

4. Selezionare **Abilita JIT in 1 VM**.

5. Nel pannello **Configurazione dell'accesso JIT alla VM**, all'estrema destra della riga che fa riferimento alla porta **22**, fare clic sul pulsante con i puntini di sospensione e quindi su **Elimina**.

6. Nel pannello **Configurazione dell'accesso JIT alla VM** fare clic su **Salva**.

    >**Nota**: monitorare lo stato di avanzamento della configurazione facendo clic sull'icona **Notifiche** sulla barra degli strumenti e visualizzando il pannello **Notifiche**. 

    >**Nota**: la propagazione nel punteggio di sicurezza delle raccomandazioni implementate in questo lab può richiedere del tempo. Controllare periodicamente il punteggio di sicurezza per verificare che sia stata presa in considerazione l'implementazione di queste funzionalità. 

> Risultati: è stato eseguito l'onboarding di Microsoft Defender for Cloud e sono state implementate le raccomandazioni per la macchina virtuale. 

    >**Note**: Do not remove the resources from this lab as they are needed for the Azure Sentinel lab.
