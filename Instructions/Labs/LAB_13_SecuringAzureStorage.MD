---
lab:
  title: ' 13 - Endpoint di servizio e protezione Archiviazione'
  module: Module 03 - Secure data and applications
---

# Lab 13: Endpoint di servizio e protezione Archiviazione
# Manuale del lab per gli studenti

## Scenario laboratorio

È stato chiesto di creare un modello di verifica per illustrare la protezione delle condivisioni file di Azure. In particolare, sarà necessario:
    
- Creare un endpoint di archiviazione in modo che il traffico destinato ad Archiviazione di Azure rimanga sempre all'interno della rete backbone di Azure.
- Configurare l'endpoint di archiviazione in modo che solo le risorse di una subnet specifica possano accedere all'archiviazione.
- Verificare che le risorse esterne alla subnet specifica non possano accedere all'archiviazione. 

> Per tutte le risorse di questo lab, viene usata l'area **Stati Uniti orientali**. Verificare con il docente che questa sia l'area da usare per il corso. 

## Obiettivi del lab

In questo lab verrà completato l'esercizio seguente:

- Esercizio 1: Endpoint servizio e archiviazione di sicurezza

## Diagramma di endpoint servizio e archiviazione di sicurezza

![image](https://user-images.githubusercontent.com/91347931/157534883-29664a05-85d1-4c70-99a7-f16d2360755d.png)

## Istruzioni

### Esercizio 1: Endpoint servizio e archiviazione di sicurezza

### Tempo stimato: 45 minuti

In questo esercizio si completeranno le seguenti attività:

- Attività 1: Creare una rete virtuale
- Attività 2: Aggiungere una subnet alla rete virtuale e configurare un endpoint di archiviazione
- Attività 3: Configurare un gruppo di sicurezza di rete per limitare l'accesso alla subnet
- Attività 4: Configurare un gruppo di sicurezza di rete per consentire RDP nella subnet pubblica
- Attività 5: Creare un account di archiviazione con una condivisione file
- Attività 6: Distribuire macchine virtuali nelle subnet designate
- Attività 7: Testare la connessione di archiviazione dalla subnet privata per verificare che l'accesso sia consentito
- Attività 8: Testare la connessione di archiviazione dalla subnet pubblica per verificare che l'accesso sia negato 

#### Attività 1: Creare una rete virtuale

In questa attività verrà creata una rete virtuale.

1. Accedere al portale di Azure **`https://portal.azure.com/`**.

    >**Nota**: accedere al portale di Azure con un account con il ruolo Proprietario o Collaboratore nella sottoscrizione di Azure usata per il lab.

2. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Reti virtuali** e premere **INVIO**.

3. Nel pannello **Reti virtuali** fare clic su  **+ Crea**.

4. Nella scheda **Informazioni di base** del pannello **Crea rete virtuale** specificare le impostazioni seguenti lasciando i valori predefiniti per le altre, quindi fare clic su **Avanti: Indirizzi IP**:

    |Impostazione|Valore|
    |---|---|
    |Abbonamento|Nome della sottoscrizione di Azure usata in questo lab|
    |Gruppo di risorse|Fare clic su **Crea nuovo** e digitare il nome **AZ500LAB12**|
    |Name|**myVirtualNetwork**|
    |Region|**(Stati Uniti) Stati Uniti orientali**|

5. Nella scheda **Indirizzi IP** del pannello **Crea rete virtuale** impostare lo **Spazio indirizzi IPv4** su **10.0.0.0/16**, nella colonna **Nome della subnet** fare clic su **default** e nel pannello **Modifica subnet** specificare le impostazioni seguenti e fare clic su **Salva**:

    |Impostazione|Valore|
    |---|---|
    |Nome subnet|**Pubblica**|
    |Intervallo di indirizzi subnet|**10.0.0.0/24**|

6. Nella scheda **Indirizzi IP** del pannello **Crea rete virtuale** fare clic su **Rivedi e crea**.

7. Nella scheda **Rivedi e crea** del pannello **Crea rete virtuale** fare clic su **Crea**.

#### Attività 2: Aggiungere una subnet alla rete virtuale e configurare un endpoint di archiviazione

In questa attività verrà creata un'altra subnet in cui verrà abilitato un endpoint di servizio. Gli endpoint di servizio sono abilitati per servizio e per subnet. 

1. Nel portale di Azure tornare al pannello **Reti virtuali**.

2. Nel pannello **Reti virtuali** fare clic sulla voce **myVirtualNetwork**.

3. Nella sezione **Impostazioni** del pannello **myVirtualNetwork** fare clic su **Subnet**.

4. Nel pannello **myVirtualNetwork \| Subnet** fare clic su **+ Subnet**. 

5. Nel pannello **Aggiungi subnet** specificare le impostazioni, seguenti lasciando i valori predefiniti per le altre:

    |Impostazione|Valore|
    |---|---|
    |Nome subnet|**Privata**|
    |Intervallo di indirizzi subnet|**10.0.1.0/24**|
    |Endpoint di servizio|**Microsoft.Storage**|

6. Nel pannello **Aggiungi subnet** fare clic su **Salva** per aggiungere la nuova subnet.

    >**Nota**: la rete virtuale ha ora due subnet, Pubblica e Privata. 
    
#### Attività 3: Configurare un gruppo di sicurezza di rete per limitare l'accesso alla subnet

In questa attività verrà creato un gruppo di sicurezza di rete con due regole di sicurezza in uscita (Archiviazione e Internet) e una regola di sicurezza in ingresso (RDP). Il gruppo di sicurezza di rete verrà inoltre associato alla subnet Privata. In questo modo verrà limitato il traffico in uscita dalle macchine virtuali di Azure connesse a tale subnet.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Gruppi di sicurezza di rete** e premere **INVIO**.

2. Nel pannello **Gruppi di sicurezza di rete** fare clic su **+ Crea**.

3. Nella scheda **Informazioni di base** del pannello **Crea gruppo di sicurezza di rete** specificare le impostazioni seguenti: 

    |Impostazione|Valore|
    |---|---|
    |Abbonamento|Nome della sottoscrizione di Azure usata in questo lab|
    |Gruppo di risorse|**AZ500LAB12**|
    |Name|**myNsgPrivate**|
    |Region|**Stati Uniti orientali**|

4. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: nei passaggi successivi verrà creata una regola di sicurezza in uscita che consente la comunicazione con il servizio Archiviazione di Azure. 

5. Nel portale di Azure tornare al pannello **Gruppi di sicurezza di rete** e fare clic sulla voce **myNsgPrivate**.

6. Nel pannello **myNsgPrivate**, nella sezione **Impostazioni** fare clic su **Regole di sicurezza in uscita**.

7. Nel pannello **myNsgPrivate \| Regole di sicurezza in uscita** fare clic su **+ Aggiungi**.

8. Nel pannello **Aggiungi regola di sicurezza in uscita** specificare le impostazioni seguenti per consentire in modo esplicito il traffico in uscita verso Archiviazione di Azure (usare le impostazioni predefinite per tutti gli altri valori): 

    |Impostazione|Valore|
    |---|---|
    |Origine|**Tag del servizio**|
    |Tag del servizio di origine|**VirtualNetwork**|
    |Intervalli porte di origine|**\***|
    |Destinazione|**Tag del servizio**|
    |Tag del servizio di destinazione|**Storage**|
    |Intervalli porte di destinazione|**\***|
    |Protocollo|**Any**|
    |Azione|**Consenti**|
    |Priorità|**1000**|
    |Name|**Allow-Archiviazione-All**|

9. Nel pannello **Aggiungi regola di sicurezza in uscita** fare clic su **Aggiungi** per creare la nuova regola in uscita. 

10. Nel pannello **myNsgPrivate**, nella sezione **Impostazioni** fare clic su **Regole di sicurezza in uscita**, quindi fare clic su **+ Aggiungi**.

11. Nel pannello **Aggiungi regola di sicurezza in uscita** specificare le impostazioni seguenti per negare in modo esplicito il traffico in uscita verso Internet (usare le impostazioni predefinite per tutti gli altri valori): 

    |Impostazione|Valore|
    |---|---|
    |Origine|**Tag del servizio**|
    |Tag del servizio di origine|**VirtualNetwork**|
    |Intervalli porte di origine|**\***|
    |Destinazione|**Tag del servizio**|
    |Tag del servizio di destinazione|**Internet**|
    |Intervalli porte di destinazione|**\***|
    |Protocollo|**Any**|
    |Azione|**Nega**|
    |Priorità|**1100**|
    |Name|**Deny-Internet-All**|

    >**Nota**: questa regola sostituisce una regola predefinita in tutti i gruppi di sicurezza di rete che consente la comunicazione Internet in uscita. 

    >**Nota**: nei passaggi successivi verrà creata una regola di sicurezza in ingresso che consente il traffico RDP (Remote Desktop Protocol) alla subnet. La regola esegue l'override di una regola di sicurezza predefinita che rifiuta tutto il traffico in ingresso da Internet. Sono consentite le connessioni Desktop remoto alla subnet per poter testare la connettività in un passaggio successivo.

12. Nel pannello **myNsgPrivate**, nella sezione **Impostazioni** fare clic su **Regole di sicurezza in ingresso** e quindi fare clic su **+ Aggiungi**.

13. Nel pannello **Aggiungi regola di sicurezza in ingresso** specificare le impostazioni seguenti, lasciando i valori predefiniti per le altre voci: 

    |Impostazione|Valore|
    |---|---|
    |Origine|**Any**|
    |Intervalli porte di origine|**\***|
    |Destinazione|**Tag del servizio**|
    |Tag del servizio di destinazione|**VirtualNetwork**|
    |Intervalli porte di destinazione|**3389**|
    |Protocollo|**TCP**|
    |Azione|**Consenti**|
    |Priorità|**1200**|                                                    
    |Name|**Allow-RDP-All**|

14. Nel pannello **Aggiungi regola di sicurezza in ingresso** fare clic su **Aggiungi** per creare la nuova regola in ingresso. 

    >**Nota**: il gruppo di sicurezza di rete verrà ora associato alla subnet Privata.

15. Nel pannello **Subnet** selezionare **+ Associa** e specificare le impostazioni seguenti nella sezione **Associa subnet**, quindi fare clic su **OK**:

    |Impostazione|Valore|
    |---|---|
    |Rete virtuale|**myVirtualNetwork**|
    |Subnet|**Privata**|

#### Attività 4: Configurare un gruppo di sicurezza di rete per consentire RDP nella subnet pubblica

In questa attività si creerà un gruppo di sicurezza di rete con una regola di sicurezza in ingresso (RDP). Il gruppo di sicurezza di rete verrà inoltre associato alla subnet Pubblica. In questo modo verrà consentito l'accesso RDP alla macchina virtuale pubblica.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Gruppi di sicurezza di rete** e premere **INVIO**.

2. Nel pannello **Gruppi di sicurezza di rete** fare clic su **+ Crea**.

3. Nella scheda **Informazioni di base** del pannello **Crea gruppo di sicurezza di rete** specificare le impostazioni seguenti: 

    |Impostazione|Valore|
    |---|---|
    |Abbonamento|Nome della sottoscrizione di Azure usata in questo lab|
    |Gruppo di risorse|**AZ500LAB12**|
    |Name|**myNsgPublic**|
    |Region|**Stati Uniti orientali**|

4. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: nei passaggi successivi verrà creata una regola di sicurezza in uscita che consente la comunicazione con il servizio Archiviazione di Azure. 

5. Nel portale di Azure tornare nel pannello **Gruppi di sicurezza di rete** e fare clic sulla voce **myNsgPublic**.

6. Nella sezione **Impostazioni** del pannello **myNsgPublic** fare clic su **Regole di sicurezza in ingresso** e quindi fare clic su **+Aggiungi**.

7. Nel pannello **Aggiungi regola di sicurezza in ingresso** specificare le impostazioni seguenti, lasciando i valori predefiniti per le altre voci: 

    |Impostazione|Valore|
    |---|---|
    |Origine|**Any**|
    |Intervalli porte di origine|**\***|
    |Destinazione|**Tag del servizio**|
    |Tag del servizio di destinazione|**VirtualNetwork**|
    |Intervalli porte di destinazione|**3389**|
    |Protocollo|**TCP**|
    |Azione|**Consenti**|
    |Priorità|**1200**|                                                    
    |Name|**Allow-RDP-All**|

8. Nel pannello **Aggiungi regola di sicurezza in ingresso** fare clic su **Aggiungi** per creare la nuova regola in ingresso. 

    >**Nota**: il gruppo di sicurezza di rete verrà ora associato alla subnet Pubblica.

9. Nel pannello **Subnet** selezionare **+ Associa** e specificare le impostazioni seguenti nella sezione **Associa subnet**, quindi fare clic su **OK**:

    |Impostazione|Valore|
    |---|---|
    |Rete virtuale|**myVirtualNetwork**|
    |Subnet|**Pubblica**|
    
#### Attività 5: Creare un account di archiviazione con una condivisione file

In questa attività verrà creato un account di archiviazione con una condivisione file e si otterrà la chiave dell'account di archiviazione.  

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Account di archiviazione** e premere **INVIO**.

2. Nel pannello **Account di archiviazione** fare clic su  **+ Crea**.

3. Nella scheda **Informazioni di base** del pannello **Crea account di archiviazione** specificare le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    |Impostazione|Valore|
    |---|---|
    |Abbonamento|Nome della sottoscrizione di Azure usata in questo lab|
    |Gruppo di risorse|**AZ500LAB12**|
    |Nome account di archiviazione|Qualsiasi nome univoco globale composto da 3-24 lettere e numeri|
    |Ubicazione|**(Stati Uniti) Stati Uniti orientali**|
    |Prestazioni|**Standard (account v2 per utilizzo generico)**|
    |Ridondanza|**Archiviazione con ridondanza locale**|

4. Nella scheda **Generale** del pannello **Crea account di archiviazione** fare clic su **Rivedi e crea**, attendere il completamento del processo di convalida e quindi fare clic su **Crea**.

    >**Nota**: attendere che l'account di archiviazione venga creato. L'operazione richiede circa 2 minuti.

5. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Gruppi di risorse** e premere **INVIO**.

6. Nel pannello **Gruppi di risorse** fare clic sulla voce **AZ500LAB12** nell'elenco dei gruppi di risorse.

7. Nel pannello del gruppo di risorse **AZ500LAB12**, nell'elenco di risorse selezionare la voce che rappresenta l'account di archiviazione appena creato.

8. Nel pannello **Panoramica** dell'account di archiviazione fare clic su **Condivisioni file** nella scheda **Archiviazione dati** e quindi su **+ Condivisione file**.

9. Nel pannello **Nuova condivisione** file rimuovere l'opzione **Abilita backup** nella scheda Backup. 

10. Nel pannello **Nuova condivisione file** specificare le impostazioni seguenti:

    |Impostazione|valore|
    |---|---|
    |Name|**my-file-share**|

11. Nel pannello **Nuova condivisione file** fare clic su **Crea**.

    >**Nota**: recuperare e registrare lo script di PowerShell che crea un mapping dell'unità alla condivisione file di Azure. 

12. Nel pannello dell'account di archiviazione, nell'elenco delle condivisioni file fare clic su **my-file-share**.

13. Nel pannello **my-file-share** fare clic su **Connetti**.

14. Nel pannello **Connetti**, nella scheda **Windows** copiare lo script di PowerShell che crea un mapping dell'unità Z alla condivisione file. 

    >**Nota**: registrare questo script. Sarà necessario in un secondo momento in questo lab per eseguire il mapping della condivisione file dalla macchina virtuale di Azure nella subnet **Privata**.

15. Tornare al pannello dell'account di archiviazione, quindi nella sezione **Sicurezza e rete** fare clic su **Rete**.

16. Nel pannello **Firewall e reti virtuali** selezionare l'opzione **Abilitato da reti virtuali e indirizzi IP selezionati** e fare clic sul collegamento **+ Aggiungi rete virtuale esistente**. 

17. Nel pannello **Aggiungi reti** specificare le impostazioni seguenti:

    |Impostazione|Valore|
    |---|---|
    |Abbonamento|Nome della sottoscrizione di Azure usata in questo lab|
    |Reti virtuali|**myVirtualNetwork**|
    |Subnet|**Privata**|

18. Nel pannello **Aggiungi reti** fare clic su **Aggiungi**. 

19. Tornare al pannello dell'account di archiviazione e fare clic su **Salva**.

    >**Nota**: a questo punto del lab sono stati configurati una rete virtuale, un gruppo di sicurezza di rete e un account di archiviazione con una condivisione file. 

#### Attività 6: Distribuire macchine virtuali nelle subnet designate

In questa attività verranno create due macchine virtuali, una nella subnet Privata e una in quella Pubblica. 

>**Nota**: la prima macchina virtuale verrà connessa alla subnet Privata.

1. Nella casella di testo **Cerca risorse, servizi e documentazione** nella parte superiore della pagina del portale di Azure digitare **Macchine virtuali** e premere **INVIO**.

2. Nel pannello **Macchine virtuali** fare clic su **+ Crea** e nell'elenco a discesa fare clic su **+ Macchina virtuale di Azure**.

3. Nella scheda **Informazioni di base** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |---|---|
    |Abbonamento|Nome della sottoscrizione di Azure che verrà usata nel lab|
    |Gruppo di risorse|**AZ500LAB12**|
    |Virtual machine name|**myVmPrivate**|
    |Region|**(Stati Uniti) Stati Uniti orientali**|
    |Immagine|**Windows Server 2022 Datacenter: Azure Edition - Gen 2**|
    |Username|**localadmin**|
    |Password|**Usare la password personale creata in Lab 04 > Esercizio 1 > Attività 1 > Passaggio 9.**|
    |Porte in ingresso pubbliche|**Nessuna**|
    |Si ha già una licenza di Windows Server|**Non selezionato**|

    >**Nota**: per le porte in ingresso pubbliche, verrà usato il gruppo di sicurezza di rete creato in precedenza. 

4. Fare clic su **Avanti: Dischi >** e nella scheda **Dischi** del pannello **Crea macchina virtuale** impostare **Tipo di disco del sistema operativo** su **HDD Standard**, quindi fare clic su **Avanti: Rete >**.

5. Fare clic su **Avanti: Rete >** e quindi di nuovo nella scheda **Rete** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |---|---|
    |Rete virtuale|**myVirtualNetwork**|
    |Subnet|**Privata (10.0.1.0/24)**|
    |IP pubblico|**(new)myVmPrivate-ip**|
    |Gruppo di sicurezza di rete della scheda di interfaccia di rete|**Nessuna**|

6. Fare clic su **Avanti: Gestione >** e, nella scheda **Gestione** del pannello **Crea macchina virtuale** accettare le impostazioni predefinite e fare clic su **Rivedi e crea**.

7. Nel pannello **Rivedi e crea** assicurarsi che la convalida sia riuscita e fare clic su **Crea**.

    >**Nota**: la seconda macchina virtuale verrà connessa alla subnet Pubblica.

8. Nel pannello **Macchine virtuali** fare clic su **+ Aggiungi** e nell'elenco a discesa fare clic su **+ Macchina virtuale di Azure**.

9. Nella scheda **Informazioni di base** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |---|---|
    |Abbonamento|Nome della sottoscrizione di Azure che verrà usata nel lab|
    |Gruppo di risorse|**AZ500LAB12**|
    |Virtual machine name|**myVmPublic**|
    |Region|**(Stati Uniti) Stati Uniti orientali**|
    |Immagine|**Windows Server 2022 Datacenter: Azure Edition - Gen 2**|
    |Username|**localadmin**|
    |Password|**Usare la password personale creata in Lab 04 > Esercizio 1 > Attività 1 > Passaggio 9.**|
    |Porte in ingresso pubbliche|**Nessuna**|
    |Si ha già una licenza di Windows Server|**Non selezionato**|

    >**Nota**: per le porte in ingresso pubbliche, verrà usato il gruppo di sicurezza di rete creato in precedenza. 

10. Fare clic su **Avanti: Dischi >** e nella scheda **Dischi** del pannello **Crea macchina virtuale** impostare **Tipo di disco del sistema operativo** su **HDD Standard**, quindi fare clic su **Avanti: Rete >**.

11. Fare clic su **Avanti: Rete >** e quindi di nuovo nella scheda **Rete** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    |Impostazione|Valore|
    |---|---|
    |Rete virtuale|**myVirtualNetwork**|
    |Subnet|**Pubblica (10.0.0.0/24)**|
    |IP pubblico|**(new)myVmPublic-ip**|
    |Gruppo di sicurezza di rete della scheda di interfaccia di rete|**Nessuna**|

12. Fare clic su **Avanti: Gestione >** e, nella scheda **Gestione** del pannello **Crea macchina virtuale** accettare le impostazioni predefinite e fare clic su **Rivedi e crea**.

13. Nel pannello **Rivedi e crea** assicurarsi che la convalida sia riuscita e fare clic su **Crea**.

    >**Nota**: è possibile passare all'attività successiva una volta completata la distribuzione della macchina virtuale di Azure **myVMPrivate**.

#### Attività 7: Testare la connessione di archiviazione dalla subnet privata per verificare che l'accesso sia consentito

In questa attività si eseguirà la connessione alla macchina virtuale myVMPrivate tramite Desktop remoto, quindi si eseguirà il mapping di un'unità alla condivisione file. 

1. Passare al pannello **Macchine virtuali**. 

2. Nel pannello **Macchine virtuali** fare clic su **myVMPrivate**.

3. Nel pannello **myVMPrivate** fare clic su **Connetti** e nel menu a discesa fare clic su **RDP**. 

4. Fare clic su **Scarica file RDP** e usare il file per connettersi alla macchina virtuale di Azure **myVMPrivate** tramite Desktop remoto. Quando viene chiesto di eseguire l'autenticazione, specificare le credenziali seguenti:

    |Impostazione|Valore|
    |---|---|
    |Nome utente|**localadmin**|
    |Password|**Usare la password personale creata in Lab 04 > Esercizio 1 > Attività 1 > Passaggio 9.**|

    >**Nota**: attendere l'apertura della sessione Desktop remoto e il caricamento di Server Manager.

    >**Nota**: verrà eseguito il mapping dell'unità Z a una condivisione file di Azure all'interno della sessione Desktop remoto in un computer Windows Server 2022

5. Nella sessione Desktop remoto per **myVMPrivate** fare clic su **Start** e quindi su **Windows PowerShell ISE**.

6. Nella finestra **Windows PowerShell ISE** aprire il riquadro **Script** quindi incollare ed eseguire lo script di PowerShell registrato in precedenza in questo lab. Il formato dello script è il seguente:

    ```powershell
    $connectTestResult = Test-NetConnection -ComputerName <storage_account_name>.file.core.windows.net -Port 445
    if ($connectTestResult.TcpTestSucceeded) {
       # Save the password so the drive will persist on reboot
       cmd.exe /C "cmdkey /add:`"<storage_account_name>.file.core.windows.net`" /user:`"localhost\<storage_account_name>`"  /pass:`"<storage_account_key>`""
       # Mount the drive
       New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage_account_name>.file.core.windows.net\my-file-share" -Persist
    } else {
       Write-Error -Message "Unable to reach the Azure storage account via port 445. Check to make sure your organization or ISP is not blocking port 445, or use Azure P2S VPN, Azure S2S VPN, or Express Route to tunnel SMB traffic over a different port."
    }
    ```
    >**Nota**: il segnaposto `<storage_account_name>` rappresenta il nome dell'account di archiviazione che ospita la condivisione file `<storage_account_key>` e la relativa chiave primaria

7. Avviare Esplora file e verificare che il mapping dell'unità Z: sia stato creato correttamente.

8. Successivamente, dal riquadro della console **Windows PowerShell ISE** eseguire questo comando per verificare che la macchina virtuale non abbia connettività in uscita a Internet:

    ```powershell
    Test-NetConnection -ComputerName www.bing.com -Port 80
    ```

    >**Nota**: il test ha esito negativo perché il gruppo di sicurezza di rete associato alla subnet Privata non consente l'accesso in uscita in Internet.

9. Terminare la sessione Desktop remoto alla macchina virtuale di Azure **myVMPrivate**.

    >**Nota**: a questo punto è stato confermato che la macchina virtuale nella subnet Privata può accedere all'account di archiviazione. 

####  Attività 8: Testare la connessione di archiviazione dalla subnet pubblica per verificare che l'accesso sia negato 

1. Passare al pannello **Macchine virtuali**. 

2. Nel pannello **Macchine virtuali** fare clic su **myVMPublic**.

3. Nel pannello **myVMPublic** fare clic su **Connetti** e nel menu a discesa fare clic su **RDP**. 

4. Fare clic su **Scarica file RDP** e usare il file per connettersi alla macchina virtuale di Azure **myVMPublic** tramite Desktop remoto. Quando viene chiesto di eseguire l'autenticazione, specificare le credenziali seguenti:

    |Impostazione|Valore|
    |---|---|
    |Nome utente|**localadmin**|
    |Password|**Usare la password personale creata in Lab 04 > Esercizio 1 > Attività 1 > Passaggio 9.**|

    >**Nota**: attendere l'apertura della sessione Desktop remoto e il caricamento di Server Manager.

    >**Nota**: verrà eseguito il mapping dell'unità Z a una condivisione file di Azure all'interno della sessione Desktop remoto in un computer Windows Server 2022

5. Nella sessione Desktop remoto per **myVMPublic** fare clic su **Start** e quindi su **Windows PowerShell ISE**.

6. Nella finestra **Windows PowerShell ISE** aprire il riquadro **Script**, quindi incollare ed eseguire lo stesso script di PowerShell eseguito all'interno della sessione Desktop remoto nella macchina virtuale di Azure **myVMPrivate**.

    >**Nota**: questa volta verrà visualizzato il messaggio di errore **New-PSDrive : Access is denied** (Accesso negato). 

    >**Nota**: l'accesso viene negato perché la macchina virtuale *myVmPublic* viene distribuita nella subnet Pubblica. La subnet Pubblica non ha un endpoint servizio abilitato per Archiviazione di Azure. L'account di archiviazione consente l'accesso alla rete unicamente dalla subnet Privata.

7. Successivamente, dal riquadro della console **Windows PowerShell ISE** eseguire questo comando per verificare che la macchina virtuale abbia connettività in uscita a Internet: 

    ```powershell
    Test-NetConnection -ComputerName www.bing.com -Port 80
    ```

    >**Nota**: il test avrà esito positivo perché non esiste alcuna regola di sicurezza in uscita per negare Internet nella subnet Pubblica.

8. Terminare la sessione Desktop remoto alla macchina virtuale di Azure **myVMPublic**.

    >**Nota**: a questo punto è stato confermato che la macchina virtuale nella subnet Pubblica non può accedere all'account di archiviazione, ma ha accesso a Internet.


**Pulire le risorse**

> Ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate evita l'addebito di costi imprevisti. 

1. Aprire Cloud Shell facendo clic sulla prima icona in alto a destra nel portale di Azure. Se richiesto, selezionare **PowerShell** e **Crea risorsa di archiviazione**.

2. Assicurarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia selezionato **PowerShell**.

3. Nella sessione di PowerShell all'interno del riquadro Cloud Shell eseguire il comando seguente per rimuovere il gruppo di risorse creato in questo lab:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB12" -Force -AsJob
    ```

4.  Chiudere il riquadro **Cloud Shell**. 

