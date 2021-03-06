{:new_window: target="_blank"}

# Inizia ad utilizzare {{site.data.keyword.objectstorageshort}}  {: #using-object-storage} 

*Ultimo aggiornamento: 24 giugno 2016*
{: .last-updated}


## Utilizzo dell'interfaccia utente {{site.data.keyword.objectstorageshort}} {: #using-object-storage-ui}

### Elementi dell'interfaccia utente e navigazione
Quando viene effettuato il provisioning del tuo {{site.data.keyword.objectstorageshort}}, puoi visualizzare le informazioni della tua istanza nel dashboard dell'istanza del servizio {{site.data.keyword.objectstorageshort}} per {{site.data.keyword.Bluemix_notm}}. Dal dashboard, seleziona la tua istanza {{site.data.keyword.objectstorageshort}} per visualizzare il pannello con informazioni più dettagliate.  
#### Dati di utilizzo
Nella home page della tua applicazione, vedrai le informazioni sull'utilizzo dell'archiviazione per la tua istanza. Viene visualizzato anche il numero corrente di contenitori di archiviazione (**Storage Container**) e il numero totale di oggetti (**Objects**) in tutti i tuoi contenitori. Elenca l'utilizzo della memoria in megabyte. Il valore di archiviazione consumata (**Storage Consumed**) fa riferimento alla quantità corrente di spazio utilizzata. 
#### Azioni
Per recuperare i dati di utilizzo più recenti, fai clic sul pulsante di aggiornamento (**Refresh**).   
####Browser degli oggetti 
Utilizza il browser degli oggetti per gestire gli oggetti e i contenitori di archiviazione oggetti. Tra le varie azioni che puoi eseguire, puoi creare contenitori, caricare file, eliminare contenitori ed eliminare file.


## Utilizzo di {{site.data.keyword.objectstorageshort}} da un'applicazione {{site.data.keyword.Bluemix_notm}} {: #using-object-storage-from-bluemix-app} 

### Come eseguire il bind del tuo servizio {{site.data.keyword.objectstorageshort}} a un'applicazione dopo la creazione {: #bind-object-storage-to-application} 
1.	Nel dashboard {{site.data.keyword.Bluemix_notm}}, seleziona l'applicazione di cui si desideri eseguire il bind.
2.	Nella panoramica dell'applicazione, fai clic su **Esegui il bind di un servizio o di una API**.
3.	Seleziona la tua istanza {{site.data.keyword.objectstorageshort}} dall'elenco di servizi e fai clic su **Aggiungi**.
4.	Fai clic su **Riprepara** quando ti viene richiesto. La tua applicazione deve essere ripreparata per utilizzare il nuovo servizio.

### Contesto associato mediante bind

Se vuoi utilizzare {{site.data.keyword.objectstorageshort}} in un contesto associato, le credenziali cloud vengono fornite indirettamente tramite il processo di esecuzione del bind di applicazioni. Dopo che hai eseguito correttamente il bind di un'istanza del servizio alla tua applicazione, una configurazione simile alla seguente viene aggiunta alla tua variabile di
ambiente `VCAP_SERVICES`.

    {
    "Object-Storage": [
    {
      "name": "Object-Storage - YP",
      "label": "Object-Storage",
      "plan": "Free",
      "credentials": {
         "auth_url": "https://identity.open.softlayer.com",
         "project": "object_storage_d049255b",
         "projectId": "0f47b41b06d047f9aae3b33f1db061ed",
         "region": "dallas",
         "userId": "ad78b2a3f843466988afd077731c61fc",
         "username": "user_202db1f8a7aa3f3ac51ec68f10dbe7dc29070bc7",
         "password": "K/jyIi2jR=1?D.TP",
         "domainId": "2df6373c549e49f8973fb6d22ab18c1a",
         "domainName": "639347"
        }
       }
      ]
    }


## Utilizzo della CLI Swift per accedere a {{site.data.keyword.objectstorageshort}} {: #using-swift-cli}

Puoi accedere al servizio {{site.data.keyword.objectstorageshort}} su Internet e da applicazioni e server virtuali in IBM {{site.data.keyword.Bluemix_notm}}. Ecco alcuni casi di utilizzo comuni per il servizio {{site.data.keyword.objectstorageshort}}:

* Backup dei dati di volume dalle tue istanze
* Utilizzo di un'ubicazione intermedia in fase di trasferimento di grandi quantità di dati
* Trasferimento di dati tra ambienti che non sono connessi direttamente
* Funzione di repository centrale

Il servizio {{site.data.keyword.objectstorageshort}} è basato su OpenStack Swift ed è possibile accedervi utilizzando qualsiasi applicazione client compatibile. Questa sezione descrive come utilizzare il client Python Swift, che è la CLI (command-line interface) per l'API {{site.data.keyword.objectstorageshort}} e le sue estensioni, per gestire contenitori e file.

### Installazione del client Swift {: #install-swift-client}

Installa il seguente software prerequisito, se non è già installato. Per ulteriori informazioni, vedi la [documentazione di OpenStack](http://docs.openstack.org/user-guide/common/cli_install_openstack_command_line_clients.html#install-the-prerequisite-software){: new_window}. 
* Python 2.7 o successive
* Package setuptools
* Package pip

Installa il client Python Swift utilizzando Python pip:

	sudo pip install python-swiftclient

### Impostazione del client {: #setup-swift-client}

Il client Swift prende le seguenti informazioni di autenticazione dalle seguenti variabili di ambiente:
* ```OS_AUTH_URL``` è l'URL dell'endpoint
* ```OS_USER_ID``` è il nome utente
* ```OS_PASSWORD``` è la password

Imposta le informazioni di autenticazione nel seguente modo. 

	export OS_USER_ID=24a20b8e4e724f5fa9e7bfdc79ca7e85
	export OS_PASSWORD=aaa55AAAaaaaa]?,
	export OS_PROJECT_ID=383ec90b22ff4ba4a78636f4e989d5b1
	export OS_AUTH_URL=https://identity.open.softlayer.com/v3
	export OS_REGION_NAME=dallas
	export OS_IDENTITY_API_VERSION=3
	export OS_AUTH_VERSION=3

Puoi trovare i valori di credenziale per il tuo servizio {{site.data.keyword.objectstorageshort}} dalla pagina **Credenziali del servizio**
nell'interfaccia utente {{site.data.keyword.objectstorageshort}}. 

**Nota:** assicurati di aggiungere ```/v3``` al ```auth_url``` dalle credenziali nell'interfaccia utente {{site.data.keyword.objectstorageshort}} quando configuri le variabili di ambiente ```OS_AUTH_URL``` per il client Swift.


### Gestione dei contenitori {: #work-with-containers}

Elencazione dei contenitori:

	swift list
	
Creazione di un contenitore:

	swift post <nome_contenitore>
	
Elencazione del contenuto di un contenitore:

	swift list <nome_contenitore>

### Gestione degli oggetti {: #work-with-objects}

#### Aggiunta di una file a un contenitore

	swift upload <nome_contenitore> <nome_file>

#### Aggiunta di file più grandi di 5 GB a un contenitore

Se stai caricando un file più grande di 5 GB, devi suddividerlo in parti più piccole. Puoi indicare al client Swift di gestire tale caricamento fornendo il parametro ```-segment-size```:

	swift upload <nome_contenitore> <nome_file> --segment-size <dimensione_in_byte>
	
Ogni segmento viene caricato in parallelo in un contenitore separato denominato ```<container_name>_segments```. Dopo che tutti i segmenti sono stati caricati, Swift crea un file manifest in modo che i segmenti possano essere scaricati in un singolo file dal contenitore originale ```<container_name>``` con il nome file originale ```<file_name>```.

Ad esempio, il seguente comando carica un file denominato ```large_file``` da un contenitore denominato ```test_container``` con la dimensione del segmento ```1073741824```.

	swift upload test_container -S 1073741824 large_file

Puoi eseguire il seguente comando per scaricare il file:

	swift download test_container large_file

#### Download di un file

	swift download <nome_contenitore> <nome_file>
	
#### Aggiunta di una directory a un contenitore

Swift non ha una vera struttura di directory ma utilizza la denominazione per rappresentare un layout di directory. Per aggiungere una directory a un contenitore, esegui questo comando:

	swift upload <nome_contenitore> <nome_directory>
	
Questo comando caricherà la struttura di directory completa come un percorso relativo. Ad esempio, se specifichi ```/mnt/volume1```, la struttura di directory mnt/volume1 verrà collegata a tutti i nomi file per indicare la struttura di directory.

	
#### Download di una directory

Per scaricare una struttura di directory, utilizza il parametro ```-prefix``` per indicare la directory o la struttura di directory che vuoi scaricare.

	swift download <nome_contenitore> --prefix <directory>
	
#### Eliminazione di un file

	swift delete <nome_contenitore> <nome_file>

### Utilizzo delle versioni dell'oggetto {: #work-with-object-versioning}

Puoi configurare le versioni di ogni oggetto nel tuo contenitore utilizzando l'indicatore ```X-Versions-Location```. Per farlo, crea un contenitore aggiuntivo per mantenere le vecchie versioni dei tuoi oggetti nel seguente modo. 

Se stai utilizzando il client swift, puoi configuralo in questo modo:

	swift post container_one -H "X-Versions-Location:container_two"

Se stai utilizzando curl, puoi configuralo in questo modo:

	curl -i -X PUT -H "X-Auth-Token: <token>" -H "X-Versions-Location:container_two" https://<object-storage_url>/container_one

Nell'esempio, ```container_two``` è stato configurato per contenere le vecchie versioni dei tuoi oggetti archiviati in  ```container_one```. Pertanto, ```container_one``` avrà la versione più aggiornata dei tuoi oggetti e ```container_two``` avrà la vecchie versioni dei tuoi oggetti. Assicurati che ```container_two``` esista in modo che l'utilizzo delle versioni funzioni.

Con l'utilizzo delle versioni configurato, quando carichi un oggetto in ```container_one```, se è presente una versione esistente dell'oggetto, la versione esistente viene spostata in ```container_two``` e la nuova versione viene creata in ```container_one```. Se elimini un oggetto da ```container_one```, la versione precedente dell'oggetto viene spostata da ```container_two``` a ```container_one```.

Gli oggetti in ```container_two``` saranno automaticamente denominati con il  seguente formato: ```<Length><Object_name>/<Timestamp>```

```Length``` fa riferimento alla lunghezza del nome del tuo oggetto; questo è un numero esadecimale a 3 caratteri senza zeri. ```Object_name``` è il nome del tuo oggetto. ```Timestamp``` è la data/ora in cui questa versione particolare dell'oggetto è stata caricata originalmente.

Per disabilitare l'utilizzo delle versioni, utilizza l'indicatore ```X-Remove-Versions-Location```:

	swift post container_one -H "X-Remove-Versions-Location:"

o

	curl -i -X POST -H "X-Auth-Token: <token>" -H "X-Remove-Versions-Location: anyvalue" https://<object-storage_url>/container_one

Questo è un esempio completo dell'utilizzo delle versioni:

1. Crea un contenitore:

		$ swift post container_one
		$

2. Configura l'utilizzo delle versioni per container_one:

		$ swift post container_one -H "X-Versions-Location:container_two"
		$

3. Crea container_two:

		$ swift post container_two
		$

4. Carica un oggetto per la prima volta in container_one:

		$ swift upload container_one object
		object
		$

5. Elenca gli oggetti in container_one:

		$ swift list container_one
		object
		$

6. Elenca gli oggetti in container_two:

		$ swift list container_two
		$

7. Carica una nuova versione dell'oggetto in container_one:

		$ swift upload container_one object
		object
		$

8. Elenca gli oggetti in container_one:

		$ swift list container_one
		object
		$

9. Elenca gli oggetti in container_two:

		$ swift list container_two
		006object/1457456909.27383
		$

10. Elimina l'oggetto in container_one:

		$ swift delete container_one object
		object
		$

11. Elenca entrambi i contenitori:

		$ swift list container_one
		object
		$ swift list container_two
		$

### Pianificazione dell'eliminazione dell'oggetto {: #schedule-object-deletion}

Puoi specificare i tuoi oggetti in modo che scadano dopo una quantità di tempo prestabilita. In altre parole, puoi pianificare l'eliminazione dei tuoi oggetti. Puoi farlo utilizzando le intestazioni ```X-Delete-At``` o ```X-Delete-After```. L'intestazione ```X-Delete-At``` è un numero intero che rappresenta il momento nel quale eliminare l'oggetto. L'intestazione ```X-Delete_After``` è un numero intero che rappresenta il numero di secondi dopo cui l'oggetto sarà eliminato.

Se stai utilizzando il client swift fai un post dell'oggetto nel tuo contenitore, consulta i seguenti esempi.

* Per impostare che l'oggetto venga eliminato il "2016/04/01 08:00:00", utilizza il seguente comando:

		swift post -H "X-Delete-At:1459515600" container object

* Per impostare che l'oggetto venga eliminato tra un'ora, utilizza il seguente comando:

		swift post -H "X-Delete-After:3600" container object

  Dopo di ciò, il comando ```swift stat container object``` visualizzerà l'intestazione ```X-Delete-At``` con la data/ora di scadenza appropriata.

* Per rimuovere la data/ora di scadenza dal tuo oggetto, utilizza il seguente comando:

		swift post -H "X-Remove-Delete-After:" container object

Se stai utilizzando curl, i comandi sono i seguenti. 

* Per impostare che l'oggetto venga eliminato il "2016/04/01 08:00:00", utilizza il seguente comando:

		curl -X POST -H "X-Auth-Token: <token>" -H "X-Delete-At:1459515600" https://<object-storage_url>/container/object

* Per impostare che l'oggetto venga eliminato tra un'ora, utilizza il seguente comando:

		curl -X POST -H "X-Auth-Token: <token>" -H "X-Delete-After:3600" https://<object-storage_url>/container/object

* Per controllare se l'oggetto ha un'intestazione, utilizza il seguente comando:

		curl -I -H "X-Auth-Token: <token>" https://<object-storage_url>/container/object

* Per rimuovere la data/ora di scadenza, utilizza il seguente comando:

		curl -X POST -H "X-Auth-Token: <token>" -H "X-Remove-Delete-At:" https://<object-storage_url>/container/object

**Nota:** l'eliminazione effettiva di un oggetto potrebbe non verificarsi nell'esatto momento indicato. Tuttavia, l'oggetto scadrà nel momento specificato, il che significa che non sarà più raggiungibile. L'eliminazione effettiva avrà luogo la volta successiva in cui viene eseguito il daemon swift-object-expirer configurato nel tuo cluster swift.





### Creazione di un URL temporaneo {: #create-temporary-url}

Un URL temporaneo è un URL lungo e difficile da indovinare che può essere utilizzato per uno specifico periodo per scaricare oggetti senza che sia richiesta ulteriore autenticazione. Genera un URL temporaneo con la seguente procedura:

1. Identifica il tuo account di autenticazione.
2. Imposta una chiave segreta.
3. Crea l'URL temporaneo.

#### Identificazione del tuo account di identificazione

Il comando ```stat``` stampa le informazioni sul tuo account:

	swift stat

Individua il campo Account e annota la stringa completa dietro *Account*: compreso ```AUTH_```.

#### Impostazione di una chiave segreta

Questa chiave può essere qualsiasi cosa tu selezioni ma è buona prassi selezionare una stringa lunga, casuale e difficile da indovinare.

	swift post -m "Temp-URL-Key:<chiave>"
	
Esegui il comando ```stat``` per verificare che ```Temp-URL-Key``` sia configurato correttamente

	swift stat


#### Creazione dell'URL temporaneo

Il comando Swift ```tempurl``` rende questi argomenti posizionali:

* [metodo] GET per consentire lo scaricamento. PUT per consentire il caricamento.
* [secondi] Tempo in secondi per cui l'URL temporaneo sarà disponibile.
* [percorso] Il percorso completo dell'oggetto espresso come ```/v1/<auth_account>/<container_name>/<object_name>```. Per ulteriori informazioni, consulta l'URL [{{site.data.keyword.objectstorageshort}}](#access-points). 
* [chiave] La chiave da te impostata al passo 2.

```
swift tempurl GET <secondi> <percorso> <chiave>
```

Questo comando restituirà un URL che puoi accodare al tuo nome cluster per ottenere un URL completo. Utilizza l'URL completo per scaricare l'oggetto con qualsiasi client HTTP compatibile come curl, wget o Firefox.

## Utilizzo dell'API REST Swift per accedere a {{site.data.keyword.objectstorageshort}} {: #using-swift-restapi}

Puoi utilizzare l'API REST Swift con un'interfaccia client di riga di comando, come cURL, oppure richiamare l'API dalla tua applicazione.  

### URL {{site.data.keyword.objectstorageshort}} {: #access-points}

Per interagire con l'API {{site.data.keyword.objectstorageshort}}, crea l'URL {{site.data.keyword.objectstorageshort}} nel seguente modo:

	https://<punto di accesso>/<versione API>/AUTH_<ID progetto>/<spazio dei nomi contenitore>/<object namespace>



L'URL consiste di cinque parti. La ```<versione API>``` è v1. Puoi trovare l'```<ID progetto>```, ```<spazio dei nomi del contenitore>```, e ```<object namespace>``` del tuo {{site.data.keyword.objectstorageshort}} dall'interfaccia utente {{site.data.keyword.objectstorageshort}}.  Per il ```<punto di accesso>```, consulta la seguente tabella: 


| **Regione**  |   **Punto di accesso pubblico**                     |
|-------------|-----------------------------------------------|
| Dallas      | https://dal.objectstorage.open.softlayer.com/ | 
| Londra      | https://lon.objectstorage.open.softlayer.com/ |


*Tabella 1. Punto di accesso {{site.data.keyword.objectstorageshort}}*


### API {{site.data.keyword.objectstorageshort}}

Consulta la [guida di riferimento completa per l'API Swift OpenStack](http://developer.openstack.org/api-ref-objectstorage-v1.html){: new_window} per un elenco completo delle opzioni dell'API REST {{site.data.keyword.objectstorageshort}} e dei relativi esempi.

## Utilizzo di {{site.data.keyword.objectstorageshort}} su più regioni {: #multi-regions}  

Il servizio IBM {{site.data.keyword.objectstorageshort}} per {{site.data.keyword.Bluemix_notm}} supporta le regioni di archiviazione di Dallas e Londra. Queste regioni di archiviazione sono indipendenti dalla regione {{site.data.keyword.Bluemix_notm}}, come Stati Uniti Sud e Regno Unito, in cui viene creata l'istanza
del servizio {{site.data.keyword.objectstorageshort}}.  Ad esempio, se crei un'istanza {{site.data.keyword.objectstorageshort}} nella regione {{site.data.keyword.Bluemix_notm}} Stati Uniti Sud,
puoi leggere e scrivere dati nella regione di archiviazione di Dallas o in quella di Londra.  

Per la regione {{site.data.keyword.Bluemix_notm}} Stati Uniti Sud, la regione di archiviazione di Dallas è quella predefinita. Per la regione {{site.data.keyword.Bluemix_notm}} del Regno Unito, la regione di archiviazione di Londra è quella predefinita.  L'interfaccia utente {{site.data.keyword.objectstorageshort}} viene sempre avviata alla regione di archiviazione predefinita della regione {{site.data.keyword.Bluemix_notm}}. Per spostarti da una regione all'altra, fai clic sull'elenco a discesa delle regioni di {{site.data.keyword.objectstorageshort}} e scegline un'altra.

**Nota:** il servizio {{site.data.keyword.objectstorageshort}} NON supporta la replica tra regioni di archiviazione.

### Accesso a più regioni

Per utilizzare il servizio {{site.data.keyword.objectstorageshort}}, devi [eseguire l'autenticazione presso OpenStack Keystone](#keystone-authentication). Dopo che hai eseguito l'autenticazione correttamente, un ```X-Subject-Token``` e gli endpoint {{site.data.keyword.objectstorageshort}} saranno disponibili nella risposta.

Ad esempio, per creare un contenitore denominato ```my_container``` nella regione di archiviazione di Dallas, specifica un punto di accesso Dallas nel comando curl nel seguente modo:

	# curl -i https://dal.objectstorage.open.softlayer.com/v1/AUTH_3c9c89a2edbb458da74a9e81e215da9e/my_container -X PUT -H "Content-Length: 0" -H "X-Auth-Token: gAAAAABWlw5mwttbb_6G3LnTiGusyoOSEHXMG7oTnDYWN1vBZB6XAxUEhz4ehGkdw6Qm_I9ZFFXr8fwcc2KaEbpWbQoglhAvrYTXbrkn8MvErLdnbcT0XK2t5N7lEZyyKQlsgmQWcrch8VOO_OiSKKToORYR7luI-2TrR_JIVZm-8AAS6hLhk9"

	HTTP/1.1 201 Created
	Content-Length: 0
	Content-Type: text/html; charset=UTF-8
	X-Trans-Id: tx4a640ca81c7240ea8f812-00569712fc
	Date: Thu, 14 Jan 2016 03:16:13 GMT


Per creare un contenitore denominato ```my_container``` nella regione di archiviazione di Londra, specifica un punto di accesso Londra nel comando curl nel seguente modo:

	# curl -i https://lon.objectstorage.open.softlayer.com/v1/AUTH_3c9c89a2edbb458da74a9e81e215da9e/my_container -X PUT -H "Content-Length: 0" -H "X-Auth-Token: gAAAAABWlw5mwttbb_6G3LnTiGusyoOSEHXMG7oTnDYWN1vBZB6XAxUEhz4ehGkdw6Qm_I9ZFFXr8fwcc2KaEbpWbQoglhAvrYTXbrkn8MvErLdnbcT0XK2t5N7lEZyyKQlsgmQWcrch8VOO_OiSKKToORYR7luI-2TrR_JIVZm-8AAS6hLhk9"

	HTTP/1.1 201 Created
	Content-Length: 0
	Content-Type: text/html; charset=UTF-8
	X-Trans-Id: tx4a640ca81c7240ea8f812-00569712fc
	Date: Thu, 14 Jan 2016 03:16:13 GMT

i**Nota:** il ```X-Subject-Token``` a te acquisito da Keystone funziona tra le regioni di archiviazione. 

Per ulteriori informazioni sui punti di accesso per regioni differenti, consulta la tabella dei [punti di accesso di Object Storage](#access-points).


## Capire autenticazione e credenziali {: #understanding-authentication-credentials}

### Generazione di credenziali {{site.data.keyword.objectstorageshort}} senza l'esecuzione del bind di un'applicazione

Per generare le credenziali cloud {{site.data.keyword.objectstorageshort}} per l'utilizzo esternamente a un'applicazione {{site.data.keyword.Bluemix_notm}}, devi generare una chiave di servizio per la tua istanza {{site.data.keyword.objectstorageshort}}. Puoi generare una nuova chiave selezionando **Credenziali del servizio** dalla barra laterale dell'interfaccia utente oppure utilizzando la CLI Cloud Foundry (versione 6.11.3 o successive). Dopo che hai generato e richiamato una chiave di servizio per la tua istanza {{site.data.keyword.objectstorageshort}}, puoi utilizzare
le informazioni di integrazione cloud per richiedere un token Keystone utilizzando un SDK OpenStack oppure la API OpenStack Identity e iniziare a utilizzare l'account Swift per gestire gli oggetti.
   
Per creare la chiave utilizzando la CLI Cloud Foundry, accedi ed esegui il seguente comando:
 
    cf create-service-key <nome_istanza_archiviazione_oggetti> <nome_univoco_per_questa_chiave>

Per richiamare le credenziali del servizio dalla CLI Cloud Foundry, esegui questo comando:

	cf service-key <nome_istanza_archiviazione_oggetti> <nome_univoco_per_questa_chiave>


### Progetti cloud e utenti
Il provisioning di una nuova istanza {{site.data.keyword.objectstorageshort}} crea un progetto Keystone isolato in IBM Public Cloud. Quando esegui il bind di una nuova applicazione all'istanza {{site.data.keyword.objectstorageshort}}, viene creato un nuovo utente Keystone con l'accesso al progetto. Quando effettui il deprovisioning dell'istanza, il progetto e l'utente vengono eliminati.

### OpenStack Identity (Keystone) v3 {: #keystone-authentication}
La struttura delle credenziali contiene una serie completa di attributi per consentirti di scegliere il metodo di richiesta di token OpenStack o l'SDK OpenStack che meglio si adatta alla tua applicazione. 
 
La richiesta di token v3 consigliata è una richiesta POST a https://identity.open.softlayer.com/v3/auth/tokens come mostrato nel seguente comando curl:

	curl -i \
	  -H "Content-Type: application/json" \
	  -d '
	{
		"auth": {
			"identity": {
				"methods": [
					"password"
				],
				"password": {
					"user": {
						"id": "ad78b2a3f843466988afd077731c61fc",
						"password": "K/jyIi2jR=1?D.TP"
					}
				}
			},
			"scope": {
				"project": {
					"id": "0f47b41b06d047f9aae3b33f1db061ed"
				}
			}
		}
	}' \
	  https://identity.open.softlayer.com/v3/auth/tokens ; echo

Utilizza il valore del campo ```X-Subject-Token``` dall'intestazione della risposta come campo ```X-Auth-Token``` quando effettui richieste al servizio {{site.data.keyword.objectstorageshort}}.

Ecco una risposta di esempio. La risposta è snellita per visualizzare solo le informazioni rilevanti di {{site.data.keyword.objectstorageshort}}.

	HTTP/1.1 201 Created
	Date: Mon, 29 Feb 2016 21:03:41 GMT
	Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips mod_wsgi/3.4 Python/2.7.5
	X-Subject-Token: gAAAAABW1LIubUgqKl-eInzhZUHWEnXijp7t6_5inl4DTRLxDhNbJ25ly2X7bASNvH7ocxinaJu_kdhSfnHNRwPAeYY77Ii2Cwp02-bvxUA1S9lV_knT6EyCOW2mSBl_HuuDD2cEgdiKmyZTVt-RvDxhPKYD-rHkJz-dHO4Folg8TVXotilb1uw
	Vary: X-Auth-Token
	x-openstack-request-id: req-01e096c8-5393-4f98-8ff6-029c55e42524
	Content-Length: 12051
	Content-Type: application/json

	{
	  "token" : {
	    "roles" : [
	      {
	        "id" : "f61f06a84f6443e880210fa986bd8691",
	        "name" : "ObjectStorageOperator"
	      }
	    ],
	    "catalog" : [
	      {
	        "endpoints": [
			{
	            "id" : "20cbfa6ff22b4a67a1484d30235bfc80",
	            "region" : "london",
	            "region_id" : "london",
	            "url" : "https:\/\/lon.objectstorage.service.open.networklayer.com\/v1\/AUTH_3ecf7d7bac2c4eda89c03dd3afa7a0a3",
	            "interface" : "admin"
	          },
	          {
	            "id" : "38b8c081b11a452bb951698c334a406d",
	            "region" : "london",
	            "region_id" : "london",
	            "url" : "https:\/\/lon.objectstorage.service.open.networklayer.com\/v1\/AUTH_3ecf7d7bac2c4eda89c03dd3afa7a0a3",
	            "interface" : "internal"
	          },
	          {
	            "id" : "4207049680fa4effbecd044c7448a8cb",
	            "region" : "dallas",
	            "region_id" : "dallas",
	            "url" : "https:\/\/dal.objectstorage.open.softlayer.com\/v1\/AUTH_3ecf7d7bac2c4eda89c03dd3afa7a0a3",
	            "interface" : "public"
	          },
	          {
	            "id" : "8a65a0cf38ac4211ad6a3c9c0eb337ff",
	            "region" : "london",
	            "region_id" : "london",
	            "url" : "https:\/\/lon.objectstorage.open.softlayer.com\/v1\/AUTH_3ecf7d7bac2c4eda89c03dd3afa7a0a3",
	            "interface" : "public"
	          },
	          {
	            "id" : "a60cf32be624491d89170ef8264de5e8",
	            "region" : "dallas",
	            "region_id" : "dallas",
	            "url" : "https:\/\/dal.objectstorage.service.open.networklayer.com\/v1\/AUTH_3ecf7d7bac2c4eda89c03dd3afa7a0a3",
	            "interface" : "admin"
	          },
	          {
	            "id" : "c769862200124a308d6748e418c971ba",
	            "region" : "dallas",
	            "region_id" : "dallas",
	            "url" : "https:\/\/dal.objectstorage.service.open.networklayer.com\/v1\/AUTH_3ecf7d7bac2c4eda89c03dd3afa7a0a3",
	            "interface" : "internal"
	          }
	        ],
	        "id" : "896e4064cbe742afbf9a543c15f27ac0",
	        "type" : "object-store",
	        "name" : "swift"
	      },
	    ],
	    "extras" : {

	    },
	    "user" : {
	      "id" : "0b8aebd924ef4cc7aa9232f07e47e874",
	      "name" : "user_87c094ce47a9feae3a137ffcbbfa098a888c12a8",
	      "domain" : {
	        "id" : "8753ff40ac1a4f4a9f162ad8026b6ce0",
	        "name" : "757955"
	      }
	    },
	    "expires_at" : "2016-02-29T22:03:42.061343Z",
	    "audit_ids" : [
	      "cbA-iL2dSheyB72PHd7q8Q"
	    ],
	    "issued_at" : "2016-02-29T21:03:42.000000Z",
	    "project" : {
	      "id" : "3ecf7d7bac2c4eda89c03dd3afa7a0a3",
	      "name" : "object_storage_c1d8b3a1",
	      "domain" : {
	        "id" : "8753ff40ac1a4f4a9f162ad8026b6ce0",
	        "name" : "757955"
	      }
	    },
	    "methods" : [
	      "password"
	    ]
	  }
	}


L'URL {{site.data.keyword.objectstorageshort}} si trova nel catalogo servizi Il catalogo servizi è contenuto nel corpo della risposta della richiesta di token. La risposta è un catalogo completo dei servizi OpenStack disponibili. Seleziona l'endpoint dal catalogo dei servizi immettendo ```object-store``` e la regione che corrisponde al campo regione nelle credenziali.

**Nota:** utilizza l'interfaccia pubblica (`publicURL`). L'interfaccia interna (`internalURL`) non è accessibile da {{site.data.keyword.Bluemix_notm}}.


## Annullamento del bind o effettuazione del deprovisioning di {{site.data.keyword.objectstorageshort}} {: #deprovisioning-object-storage}

### Come effettuare il deprovisioning del tuo servizio {{site.data.keyword.objectstorageshort}}
1.	Seleziona il tuo servizio dal dashboard {{site.data.keyword.Bluemix_notm}}.  
2.	Fai clic sull'icona di ingranaggio e seleziona **Elimina servizio**.
	
**Avvertenza:** se effettui il deprovisioning di un'istanza del servizio IBM {{site.data.keyword.objectstorageshort}} per {{site.data.keyword.Bluemix_notm}}, il progetto cloud e l'account Swift vengono eliminati. Tutti i contenitori e gli oggetti nell'istanza di cui viene effettuato il deprovisioning vengono eliminati da Swift e non è possibile ripristinarli.

### Annullamento del bind di un'applicazione o eliminazione di una chiave di servizio

Se annulli il bind di un'applicazione dall'istanza {{site.data.keyword.objectstorageshort}} oppure elimini la chiave di servizio, le credenziali vengono eliminate. L'account {{site.data.keyword.objectstorageshort}} viene eliminato solo dopo che è stato effettuato il deprovisioning dell'istanza {{site.data.keyword.objectstorageshort}}. Puoi generare delle nuove credenziali cloud [eseguendo nuovamente il bind di, oppure creando, una nuova chiave di servizio](#bind-object-storage-to-application).

