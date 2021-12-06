---
lab:
  title: 14 - Centro sicurezza di Azure
  module: Module 04 - Manage Security Operations
ms.openlocfilehash: 33134a5d1f7f5138083c1889c04f3296c3f05586
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/16/2021
ms.locfileid: "132625734"
---
# <a name="lab-14-azure-security-center"></a>Lab 14: Centro sicurezza di Azure
# <a name="student-lab-manual"></a>Manuale del lab per gli studenti

## <a name="lab-scenario"></a>Scenario del lab

È stato chiesto di creare un modello di verifica di un ambiente basato sul Centro sicurezza. In particolare, sarà necessario:

- Configurare il Centro sicurezza per monitorare una macchina virtuale.
- Consultare le raccomandazioni del Centro sicurezza per la macchina virtuale.
- Implementare le raccomandazioni per la configurazione guest e l'accesso Just-In-Time alle macchine virtuali. 
- Capire come poter usare il punteggio di sicurezza per determinare lo stato di avanzamento del processo di creazione di un'infrastruttura più sicura.

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## <a name="lab-objectives"></a>Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Implementare il Centro sicurezza

### <a name="exercise-1-implement-security-center"></a>Esercizio 1: Implementare il Centro sicurezza

In questo esercizio verranno eseguite le attività seguenti:

- Attività 1: Configurare il Centro sicurezza
- Attività 2: Esaminare le raccomandazioni del Centro sicurezza
- Attività 3: Implementare la raccomandazione del Centro sicurezza per abilitare l'accesso Just-In-Time alle macchine virtuali

#### <a name="task-1-configure-security-center"></a>Attività 1: Configurare il Centro sicurezza

In questa attività si eseguirà l'onboarding e la configurazione del Centro sicurezza.

1. Accedere al portale di Azure **`https://portal.azure.com/`** .

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Centro sicurezza** e premere **INVIO**.

1. Nel pannello **Centro sicurezza \| Attività iniziali** fare clic su **Aggiorna** e quindi su **Installa agenti**.
     
1. Nel pannello **Centro sicurezza \| Attività iniziali**, nella sezione **Gestione** disponibile nel menu verticale a sinistra, fare clic su **Prezzi e impostazioni**.

1. Nel pannello **Centro sicurezza \| Prezzi e impostazioni** fare clic sulla voce che rappresenta la sottoscrizione e nel pannello **Impostazioni \| Piani di Azure Defender** verificare che sia selezionata l'opzione **Azure Defender - On**. 

    >**Nota**: verificare tutte le funzionalità disponibili nel livello Azure Defender e assicurarsi che Azure Defender sia attivato per ogni tipo di risorsa. 

1. Se sono state apportate modifiche, fare clic su **Salva**.

1. Nel pannello **Impostazioni \| Piani di Azure Defender** selezionare **Abilita tutto** e fare clic su **Salva**.

1. Nel pannello **Impostazioni \| Piani di Azure Defender**, nella barra dei menu verticale disponibile sul lato sinistro, fare clic su **Provisioning automatico**.

1. Nel pannello **Impostazioni \| Provisioning automatico** assicurarsi che l'opzione **Provisioning automatico** sia impostata su **Sì** per il primo elemento **Agente di Log Analytics per VM di Azure**. 

1. Nel pannello **Impostazioni \| Provisioning automatico**, nel menu verticale disponibile sul lato sinistro, fare clic su **Automazione del flusso di lavoro**.

1. Nel pannello **Impostazioni \| Automazione del flusso di lavoro** fare clic su **+ Aggiungi l'automazione del flusso di lavoro**.

1. Nel pannello **Aggiungi l'automazione del flusso di lavoro** consultare le impostazioni disponibili. 

    >**Nota**: è possibile attivare avvisi di rilevamento delle minacce basati su azioni e le raccomandazioni del Centro sicurezza. È inoltre possibile configurare un'azione basata su app per la logica. 

1. Nel pannello **Aggiungi l'automazione del flusso di lavoro** fare clic su **Annulla**.

    >**Nota**: il Centro sicurezza offre molte informazioni dettagliate sulle macchine virtuali, tra cui lo stato della procedura di aggiornamento del sistema, le configurazioni di sicurezza del sistema operativo e la protezione degli endpoint.

1. Tornare al pannello **Centro sicurezza \| Prezzi e impostazioni** e fare clic sulla voce che rappresenta l'area di lavoro Log Analytics creata nel lab precedente.

1. Nel pannello **Impostazioni \| Piani di Azure Defender** assicurarsi che sia selezionata l'opzione **Azure Defender - On** e fare clic su **Salva**.


#### <a name="task-2-review-the-security-center-recommendation"></a>Attività 2: Esaminare le raccomandazioni del Centro sicurezza

In questa attività si esamineranno le raccomandazioni del Centro sicurezza. 

1. Nel portale di Azure tornare al pannello **Centro sicurezza \| Panoramica**. 

1. Nel pannello **Centro sicurezza \| Panoramica** osservare il riquadro **Punteggio di sicurezza**.

    >**Nota**: registrare il punteggio corrente, se disponibile.

1. Tornare al pannello **Centro sicurezza \| Panoramica**  e selezionare **Risorse valutate**.

1. Nel pannello **Inventario** selezionare la voce **myVM**.

    >**Nota**: potrebbe essere necessario attendere alcuni minuti e aggiornare la pagina del browser per poter visualizzare la voce.
    
1. Nel pannello **Integrità delle risorse**, nella scheda **Raccomandazioni**, esaminare l'elenco delle raccomandazioni per **myVM**.


#### <a name="task-3-implement-the-security-center-recommendation-to-enable-just-in-time-vm-access"></a>Attività 3: Implementare la raccomandazione del Centro sicurezza per abilitare l'accesso Just-In-Time alle macchine virtuali

In questa attività verrà implementata la raccomandazione del Centro sicurezza per abilitare l'accesso Just-In-Time alla macchina virtuale. 

1. Nel portale di Azure tornare al pannello **Centro sicurezza \| Panoramica** e selezionare il riquadro **Azure Defender**.

1. Nel pannello **Azure Defender**, nella sezione **Protezione avanzata**, fare clic sul riquadro **Accesso Just-In-Time alla VM** e nel pannello **Accesso Just-In-Time alla VM** fare clic su **Prova accesso Just-In-Time alla VM**.

1. Nel pannello **Accesso Just-In-Time alla VM** selezionare **Non configurato** e quindi fare clic sulla voce **myVM**.

    >**Nota**: potrebbe essere necessario attendere alcuni minuti prima che la voce **myVM** diventi disponibile.

1. Selezionare **Abilita JIT in 1 VM**.

1. Nel pannello **Configurazione dell'accesso JIT alla VM**, all'estrema destra della riga che fa riferimento alla porta **22**, fare clic sul pulsante con i puntini di sospensione e quindi su **Elimina**.

1. Nel pannello **Configurazione dell'accesso JIT alla VM** fare clic su **Salva**.

    >**Nota**: monitorare lo stato di avanzamento della configurazione facendo clic sull'icona **Notifiche** sulla barra degli strumenti e visualizzando il pannello **Notifiche**. 

    >**Nota**: la propagazione nel punteggio di sicurezza delle raccomandazioni implementate in questo lab può richiedere del tempo. Controllare periodicamente il punteggio di sicurezza per verificare che sia stata presa in considerazione l'implementazione di queste funzionalità. 

> Risultati: è stato eseguito l'onboarding del Centro sicurezza e sono state implementate le raccomandazioni relative alle macchine virtuali. 


>**Nota**: non rimuovere le risorse da questo lab poiché saranno necessarie per il lab su Azure Sentinel.
