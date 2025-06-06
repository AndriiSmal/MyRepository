# Concessione dei permessi alle cartelle di “Exchange admin center” via PowerShell
  
## **Fase di preparazione (Una volta):**
  
1. Attivare l’esecuzione dei script di PowerShell.

    * Assicurati di avere i ruoli necessari di utente amministratore sulla macchina locale.

2. Win+S -› Cerca PowerShell -› Tasto destro -› eseguire come amministratore.

3. Immettere il script per sbloccare lo scaricamento/esecuzione del script da internet:

	   Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned

* Proseguire inserendo la “s”

> Permette di eseguire solo i script locali firmati e blocca quelli scaricati da internet non firmati.

4. Installa il modulo “ExchangeOnlineManagement” mettendo il script:

	   Install-Module -Name ExchangeOnlineManagement -Scope CurrentUser -Force

	* Proseguire immettendo l’ “s”

5. Verifica installazione modulo:

	   Get-Module -ListAvailable ExchangeOnlineManagement

	* Se non è presente provare a installare manualmente con commando:

		  Import-Module ExchangeOnlineManagement

	* In caso di alcuni errori di compatibilità / mancanza delle funzionalità provare ad aggiornare il modulo:

	Update-Module -Name ExchangeOnlineManagement

## **Fase di esecuzione:**

1. Eseguire il commando su PowerShell:

	   Connect-ExchangeOnline -UserPrincipalName admin@dominio.com

   > A posto di admin@dominio.com inserire i credenziali di account di Microsoft 365 admin center 

2. Assicurare di avere i privilegi necessari (es. Organization Management o Public Folder Management )

3. Assegnazione del permesso:

   ##### **A una cartella**

	   Add-PublicFolderClientPermission -Identity "\NomeCartellaPubblica" -User utente@dominio.com -AccessRights Ruolo

> "\NomeCartellaPubblica" riferisce alla cartella pubblica del dominio; “utente@dominio.com” che identifica la persona a cui vuoi dare il permesso; Ruolo vuole dire le privilegi che verranno assegnati.

- In caso se c’è bisogno di assegnare il permesso al gruppo di persone

   ##### **A una cartella con sottocartelle**

	  Get-PublicFolder -identity "\NomeCartellaPubblica" -Recurse | Add-PublicFolderClientPermission -user utente@domain.com -AccessRights Ruolo

   Per inserire un gruppo di persone usare:
          
      -user "nomegruppo@domain.com"
   Per inserire un elenco di persone concrete, quando non vuoi dare accesso a tutti i partecipanti del gruppo è possibile di fare in seguente modo:
   
      $utenti = @(
      "utente1@dominio.com",
      "utente2@dominio.com",
      "utente3@dominio.com",
      "utenteN@dominio.com" 
      )

      foreach ($utente in $utenti) {
      Get-PublicFolder -identity "\NomeCartellaPubblica" -Recurse | Add-PublicFolderClientPermission -User $utente -AccessRights Ruolo
      }

