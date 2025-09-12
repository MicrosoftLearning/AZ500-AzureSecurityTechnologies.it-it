---
lab:
  title: 10 - Abilitare l'accesso JUST-In-Time nelle macchine virtuali
  module: Module 03 - Configure and manage threat protection by using Microsoft Defender for Cloud
---

# Lab 10: Abilitare l'accesso JIT nelle macchine virtuali

# Manuale del lab per gli studenti

## Scenario laboratorio

In qualità di tecnico della sicurezza di Azure presso una società di servizi finanziari, si è responsabili della protezione delle risorse di Azure, incluse le macchine virtuali (VM) che ospitano applicazioni critiche. Il team di sicurezza ha rilevato che l'accesso continuo alle macchine virtuali aumenta il rischio di attacchi di forza bruta e accesso non autorizzato. Per attenuare questo problema, il Chief Information Security Officer (CISO) ha richiesto di abilitare l'accesso JIT (Just-in-Time) alle macchine virtuali in una macchina virtuale di Azure specifica usata per l'elaborazione delle transazioni finanziarie.

## Obiettivi del lab

In questo lab verranno completati gli esercizi seguenti:

- Esercizio 1: Abilitare JIT nelle macchine virtuali dal portale di Azure.

- Esercizio 2: Richiedere l'accesso a una macchina virtuale con JIT abilitato dal portale di Azure.

## Istruzioni per l'esercizio 

### Esercizio 1: Abilitare JIT nelle macchine virtuali da macchine virtuali di Azure

>**Nota**: è possibile abilitare JIT in una macchina virtuale dalle pagine delle macchine virtuali di Azure del portale di Azure.

1. Nella casella di ricerca nella parte superiore del portale immettere **Macchine virtuali**. Selezionare **Macchine virtuali** nei risultati della ricerca.

2. Selezionare **myVM**.
 
3. Selezionare **Configurazione** nella **sezione Impostazioni** di **myVM.**
   
4. In **Accesso just-in-time alle macchine virtuali selezionare** **Abilita JUST-in-time.**

5. In **Accesso** just-in-time alle macchine virtuali fare clic sul collegamento che legge **Apri Microsoft Defender per il cloud.**

6. Per impostazione predefinita, l'accesso just-in-time per la macchina virtuale usa queste impostazioni:

   - Computer Windows
   
     - Porta RDP: 3389
     - Accesso massimo consentito: tre ore
     - Indirizzi IP di origine consentiti: Qualsiasi

   - Computer Linux
     - SSH port: 22
     - Accesso massimo consentito: Tre ore
     - Indirizzi IP di origine consentiti: Qualsiasi
   
7. Per impostazione predefinita, l'accesso just-in-time per la macchina virtuale usa queste impostazioni:

   - Nella scheda Configurato** fare clic con il **pulsante destro del mouse sulla macchina virtuale a cui si vuole aggiungere una porta e selezionare Modifica.

   ![image](https://github.com/user-attachments/assets/aa4ded55-c5b1-4d40-b5a0-a4c33b9eb81b)
   
   - In **Configurazione dell'accesso JIT alla VM** è possibile modificare le impostazioni esistenti di una porta già protetta o aggiungere una nuova porta personalizzata.
   - Al termine della modifica delle porte, selezionare **Salva**.   

### Esercizio 2: Richiedere l'accesso a una macchina virtuale abilitata per JIT dalla pagina di connessione della macchina virtuale di Azure.

>**Nota**: quando una macchina virtuale ha un JIT abilitato, è necessario richiedere l'accesso per connettersi. È possibile richiedere l'accesso in uno dei modi supportati, indipendentemente da come si è abilitato il JIT.
   
1. Nel portale di Azure aprire le pagine delle macchine virtuali.

2. Selezionare la macchina virtuale a cui connettersi e aprire la **pagina Connetti** .

   - Azure verifica se JIT è abilitato in tale macchina virtuale.

        - Se JIT non è abilitato per la macchina virtuale, viene richiesto di abilitarlo.
    
        - Se JIT è abilitato, selezionare **Richiedi accesso** per passare una richiesta di accesso con l'indirizzo IP, l'intervallo di tempo e le porte richieste configurate per la macchina virtuale.
    
   ![image](https://github.com/user-attachments/assets/f5d0b67c-7731-4261-b0eb-a56c505dadd4)

> **Risultati**: sono stati esaminati vari metodi su come abilitare JIT nelle macchine virtuali e su come richiedere l'accesso alle macchine virtuali con JIT abilitato in Microsoft Defender per il cloud.
