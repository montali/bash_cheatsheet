# Bash CheatSheet
Cheatsheet per bash in italiano. Realizzato per l'esame di gestione sistemi IT Cloud @ UniPR 2019-20

# Generali
## Informazioni
### Help function of a command
* Parentesi quadre [] accerchiano elementi opzionali
* Qualsiasi cosa seguita da ... rappresenta una lista di lung. arbitraria
* I | indicano una scelta multipla
* Gli angle brackets <> rappresentano dati variabili

### Killare una shell ssh freezata
`Enter` + `~` + `.`

### Shortcut per spostarsi
* `ctrl+a` inizio della command line
* `ctrl+e`fine della command line
* `ctrl+u`	cancella fino all'inizio
* `ctrl+k`    cancella fino alla fine
* `ctrl+sx`	vai alla parola precedente
* `ctrl+dx`	vai alla parola seguente
* `ctrl+r`	cerca nella history

### Directory importanti
* `usr` software installati, librerie.
	* `bin` comandi utente
	* `sbin` comandi admin
	* `local`	software localmente customizzato
		
* `etc` file di configurazione specifici al sistema
* `var` Dati variabili al sistema che dovrebbero rimanere tra boots, come DB, cache...
* `run` Dati di runtime per i processi avviati since boot, come ID, lock...
* `home` Dirs per il salvataggio dei dati per gli utenti
* `root` Home per root
* `tmp` Spazio temporaneo dalla durata di 10 gg (per 30gg, /var/tmp)
* `boot` File necessari per il boot
* `dev` Contiene file speciali per accedere ai device

### Path assoluti
Un path assoluto comincia alla / root

### Path relativi
Un path che non comincia per / specifica il path partendo dalla dir in cui siamo.

### File Globbing
Processo per il pattern matching/wildcards.
* `*` ogni stringa di 0+ caratteri
* `?` ogni carattere singolo
* `~` home dello user corrente
* `~`user home dell'utente user
* `~+` directory corrente
* `~-`  directory precedente
* `[abc]` un carattere tra abc
* `[!abc]` un carattere esclusi abc
* `[^abc]` un carattere esclusi abc
* `[[:alpha:]]` ogni carattere alfabetico
* `[[:lower:]]` ogni carattere lower-case
* `[[:upper:]]` ogni carattere upper-case
* `[[:alnum:]]` un carattere alfanumerico
* `[[:punct:]]` ogni carattere di punteggiatura
* `[[:digit:]]` ogni numero
* `[[:space:]]` ogni carattere di whitespace (tabs, newline, cr)

### Brace Expansion
* Utilizzata per generare stringhe arbitrarie di caratteri
* contengono una lista comma-separated o una sequenza.
* es. `{Sunday,Monday,Thursday} {1..10}`	

### Command substitution
* Sostituiamo l'espressione $(comando) con l'output del comando.
* Si possono usare anche i backticks `comando`, ma ha due svantaggi: fa confusione, e non è nestable

### Evitare l'expansion
* `$` escapa un singolo carattere
* `''` escapa un'espressione sempre
* `""` escapa espressioni ma permette command e variable substitution

### Redirectare gli output a file
* Tramite `>` possiamo redirectare un output a file.
* `>` sovrascrive
* `>>` appende
* `2`  specifica stderr
* `&` specifica entrambi
* es. `>file 2>&1`  = `&>file`

### Pipeline
Una pipeline `|` permette di passare l'output di un comando all'input di un altro

## Comandi
* `date`    mostra i correnti data/ora. Puoi specificare con + il formato desiderato
* `passwd` passwd modifica la password dell'utente corrente. root può modificarle tutte.
* `file` su linux le estensioni sono inutili. file analizza l'header di un file per vedere l'estensione
* `head` Prende le prime 10 linee di un file. Con argomento -n specifichi il numero.
* `tail` vedi head, ma alla fine del file.
* `wc` conta linee, parole, caratteri. Prende parametro -l, -l, -c per mostrarne solo uno dei tre.
* `history` mostra una lista dei comandi già eseguiti
* `pwd` mostra il full path name corrente
* `ls` mostra i contenuti della directory corrente
	* `-a` mostra tutti i file
	* `-R` è ricorsivo
* `cd` cambia directory, `..` indica la directory padre
* `touch` updata il timestamp di un file al tempo corrente. Possiamo usarlo per creare files.
* `cp` copia un file. Per file multipli, l'ultima deve essere la directory.
* `mv` sposta o rinomina un file. 
* `rm` elimina un file.
	* `-r`	elimina ricorsivamente directories
	* `-f`	forza l'operazione suddetta.
* `rmdir` elimina una directory vuota
* `mkdir` crea una directory
	* `-p`	crea le subdirectories necessarie
* `man` mostra la pagina di manuale per il comando specificato, è diviso in 9 categorie
* `stdin` buffer che legge dalla tastiera
* `stdout` buffer che invia output al terminare
* `stderr` invia messaggi di errore al terminale
* `tee` permette di creare una pipeline mantenendo l'output a terminale o a un file specificato come argomento
* `yum` permette di installare pacchetti da una repo. La configurazione è in `/etc/yum.conf` o `/etc/yum.repos.d`
 * Comandi:
  * repolist elenca le repo, pacchetti, gruppi
  * help aiuto
  * list pacchetti installati/disponibili
  * search <WORD> cerca pacchetti per nome/summary
  * search all <W> cerca anche nella description
  * info <PKGNAME> restituisce informazioni su un pacchetto
  * provides <PATH> pacchetti che matchano il pathname
  * install <PKG> installa il pacchetto
  * update updata pacchetti

# Utenti
## Informazioni

### `/etc/passwd`
Mantiene le informazioni sugli utenti.
`nome:x(era la pwd un tempo, ora /etc/shadow):UID:GID:Nome:homepath:shell`

### Gruppi
* Hanno un nome e un GID, ogni user ha un gruppo principale.
* Il gruppo primario possiede i file creati dall'utente
* Solitamente il primary group è un gruppo con il nome dell'utente
* L'utente può essere in altri gruppi salvati in `/etc/group`

### Root
L'utente root ha potere sul sistema, con privilegi massimi. La maggior parte dei device devono essere controllati da root, tranne i removibili.

### Ranges di UID
* 0 sempre assegnato a root
* 1-200 assegnato ai system users (processi)
* 201-999 system users senza file
* 1000+ utenti regolari

### Password
	Le password sono salvate in questo formato:
	$hash_algorithm_index$random_encription_salt/hash/

### `/etc/shadow`
Contiene le password:
```login_name:password:data_di_modifica_since1970:giorni_minimi_cambio:giorni_massimi_cambio:giorni_warning:giorni_eccezione:scadenza_account_since1970:blank```

### $PATH
Contiene le location contenenti eseguibili

### $HOME
Contiene la home dir

## Comandi
* `su` permette di cambiare utente
	* `-` setta l'environment a quello dell'utente nuovo, senza rimane uguale
* `sudo` permette di eseguire un comando come root
* `useradd` crea un utente con i campi di default (specificati in /etc/login) se non specificato diversamente
* `usermod` modifica un utente
    * `-c` Aggiunge un campo al GECOS field, es. nome completo
    * `-g` Specifica il gruppo primario
    * `-G` Specifica i gruppi aggiuntivi
    * `-a` Usato con `-G`, appende i gruppi senza rimuoverlo da altri
    * `-d` Specifica l'home dir
    * `-m` Sposta la home a una nuova location, va usato con `-d`
    * `-s` Specifica una nuova login shell
    * `-L` Blocca l'utente
    * `-U` Sblocca l'utente
* `userdel`	elimina l'utente da /etc/passwd, ma lascia la home dir
	* `-r` elimina anche la home
* `groupadd` crea un gruppo	
	* `-g` 	specifica il GID
	* `-r`	crea un system group con un GID disponibile
* `groupmod` modifica un GID o un nome
	* `-n` specifica il nuovo nome
	* `-g` specifica il nuovo GID
* `groupdel` elimina un gruppo

# Permessi
Possiamo settare impostazioni diverse per il creatore, il gruppo creatore, e gli altri.
Abbiamo tre permessi: read 4, write 2, execute 1
Un file appena creato è di proprietà del creatore, con gruppo primario del creatore.

## Permessi di default
I permessi default per i file sono settati dai processi che li creano. Ma per file/dir creati da shell?
Essi vengono filtrati dalla umask della shell, che rimuove i campi specificati

## Comandi
* `chmod` modifica i permessi di di un file (-R per directory ricorsivamente)
WhoWhatWhich
  * Who	u(ser), g(roup), o(ther), a(ll)
  * What	+, -, =
  * Which	r, w, x

* `chown` modifica il possessore del file (il possessore solo root, il gruppo anche user che vi appartengono)
	:	specifica il gruppo

* `setuid/setgid` permesso di execution che esegue il comando come se fosse l'utente possessore, s. S se l'owner non ha x
 * In visualizzazione l, saranno t/T.
 * Se è specificato su una dir, i file creati all'interno avranno il creatore uguale alla dir e non l'effettivo
 * Specifichiamo setuid con un 4 prima del chmod, setgid con un 2, o con u+s e g+s

* `umask` modifica la umask della shell. 

# Processi
## Stati di un processo
* Running
  * `R` TASK_RUNNING: sta eseguendo sulla CPU o attendendo di eseguire
* Sleeping
  * `S` TASK_INTERRUPTIBLE: sta aspettando qualche condizione hw/sw
  * `D` TASK_UNINTERRUPTIBLE: non risponde ai segnali. condizioni eccezionali.
  * `K` TASK_KILLABLE: identico a D, ma risponde ai SIGKILL
* Stopped
  * `T` TASK_STOPPED: il processo è stato stoppato e può essere ripristinato
  * `T` TASK_TRACED: appare durante le pause di debug
* Zombie
  * `Z` EXIT_ZOMBIE: segnala al parent mentre esce. Tutte le risorse eccesso il PID rilasciate
  * `X` EXIT_DEAD: una volta che il parent ha pulito, il processo è rimosso del tutto. Transitorio.

## Job
* Associato ad ogni pipeline, tutti i processi della pipeline sono parte del job. 
* Un terminale supporta un job alla volta.
* Un processo in background di un terminale è membro di un altro job di quel terminale.
* I processi background non possono ricevere input ma possono scrivere.
* Un job bg può essere interrotto, se prova a leggere l'input avviene in automatico.
* Ciascun terminale può avere un foreground e più background.

## Segnali di kill
* `HUP 1` Hangup: reporta la terminazione del processo controllante di un terminale
* `INT 2` Keyboard: causa il termine con ctrl+c
* `QUIT 3` Come INT ma produce un dump del processo. Ctrl+D
* `KILL 9` Termina il programma forzatamente
* `TERM 15` Causa il termine. Può essere però bloccato, ignorato. 
* `CONT 18` Resuma il processo
* `STOP 19` Sospende il processo forzatamente.
* `TSTP 20` Uno STOP più educato, bloccabile, ignorabile... Ctrl+Z

### Mandare processi in background
Per mandare un processo in background, Ctrl+z

## Load Averages
Rappresenta il carico del sistema negli ultimi 1,5,15 minuti, sulla base dei running e uninterruptible
Tutte le CPU sono sommate, in quanto i processi si potrebbero spostare da una all'altra
Linux conta ogni core/hyperthread come una CPU a parte, con request queues separate
Possiamo ottenere i load averages tramite top, uptime e w
Una CPU idle ha load 0; ogni processo aggiunge 1. 

## Comandi
* `ps` mostra i processi correnti.
  * `-a` processi associati al terminale
  * `-u` mostra l'utente collegato al processo
  * `aux` mostra tutti i processi, con user e processi senza terminale
  * `lax` mostra tutti i processi con dettagli
  * `j` mostra i jobs
  * `--sort` sorta i processi
* `&` Possiamo avviare un comando in background inserendo una & alla fine. Ci verrà mostrato il PID.
* `jobs` Permette di vedere i jobs della sessione
* `fg` Permette di portare un job in foreground tramite il suo id (%ID)
* `bg` Permette di avviare un job stoppato, in background
* `kill` Invia un segnale a un processo selezionato da ID
* `killall` Invia un segnale a tutti i processi che matchano un `pattern` 
* `pkill` Come killall ma con filtri più avanzati 
  * `-` killa tutti i processi di un utente
* `w` lista gli utenti correntemente loggati e le loro attività cumulative.
 * Tutti hanno un terminale principale, listato come pts/N per GUI o ttyN per terminali.
  * `-f` mostra il connecting system
* `top` mostra i processi di sistema dinamicamente, con un refresh continuo
 * Dati mostrati:
   * `PID`	Process ID
   * `USER` username
   * `VIRT` memoria virtuale utilizzata, includendo il resident set, shared libs, swap
   * `RES` memoria fisica utilizzata
   * `S` Process state: D (uninterruptable), R (running), S (sleeping), T (stopped/traced), Z(zombie)
   * `TIME` Tempo di processo CPU
   * `COMMAND` command name
* `systemd` fornisce un metodo per attivare risorse di sistema, daemons...
* `systemctl` gestisce gli oggetti di systemd, detti units. Alcune tipologie di units:
   * `.service` rappresenta un system service -> avvia frequently accessed daemons
   * `.socket` rappresenta socket di comunicazione inter-processo. 
   * `.path` utilizzati per ritardare l'attivazione di un servizio finché avviene uno specifico cambiamento di file
 * Opzioni:
   * `status	`Mostra lo status di un servizio
   * `--type`	Mostra lo status di determinato type (service/socket/path)
   * `is-active`
   * `is-enabled`
   * `list-units`
   * `list-unit-files`
   * `--failed`
 * Comandi:
   * `mask` maschera un servizio per impedirne l'avvio
   * `unmask` unmaschera
   * `disable` disabilita a boot
   * `enable` avvia a boot
   * `stop`
   * `start`
   * `restart`	
   * `reload` ricarica config
   * `list-dependencies`
### Daemon
Processo che gira in background, avviato al boot, funziona fino allo shutdown. 
Per convenzione, finiscono per d

### Socket
Canale di comunicazione con client locali e remoti. Creabile da daemons, o separati. Passato al daemon quando viene istanziata una connessione.

### Service
Si riferisce di solito a uno o più daemon, ma avviare/stoppare un servizio fa un one-time change allo stato del sistema, che non richiede un daemon.

# Ricerca
`grep` permette di utilizzare RegEx per isolare dati matchanti
 * utilizzo basic: grep 'RegEx' /file
 * utilizzo piped: ps aux | grep '^student'
 * Opzioni:
   * -i disattiva case sensitivity
   * -v Inverti la ricerca
   * -r ricerca ricorsiva
   * -A mostra n linee dopo il match
   * -B mostra n linee prima del match
   * -e multiple regex con or
* `which` mostra il path dell'eseguibile di un comando

# Scripts
## \#
È un commento per script bash, a meno che non sia escapato

## Variabili
Possiamo creare variabili con l'uguale, referenziarle con $ oppure ${}, preferibile in caso di ambiguità

## Arithmetic Expansion
Possiamo anche eseguire arithmetic expansion grazie alle doppie parentesi, come $((2+2)).
Gli operatori sono i soliti.

## For loop
il for scorre gli argomenti di <LIST> uno ad uno.
for <VARIABLE> in <LIST>; do <COMMAND> done

## Debug
Aggiungiamo -x a #!/bin/bash per attivare il debug, o -x all'esecuzione
Possiamo anche usare l'istruzione "set +x" e "set -x" nello script

## Parametri posizionali
I parametri posizionali salvano i valori degli argomenti command-line
0 contiene lo script name, i seguenti gli argomenti
$* contiene tutti gli argomenti insieme
$@ contiene gli argomenti separati
$# contiene il numero di argomenti passati

## Exit codes
Possiamo uscire dallo script con codice. 0 è tutto ok, gli altri no. 
L'exit code viene salvato nella variabile $?
Usciamo dallo script con exit n

## Test degli input
è good practice verificare gli input con le espressioni
`["$a" -eq "$b"]`
 * Opzioni:
   * `-eq`	uguale
   * `-ne`	diverso
   * `-gt`	maggiore	
   * `-lt`	minore
   * `-ge`	maggiore uguale
   * `-le`	minore uguale
 * Opzioni per stringhe:
   * `=` uguale a
   * `==`	uguale a
   * `!=`	diverso
 * Opzioni unary:
   * `-z`	lunghezza zero
   * `-n`	not null
 * Opzioni unary per file:
   * `-b`	il file esiste ed è block special
   * `-c`	il file esiste ed è char special	
   * `-d` 	esiste ed è una directory	
   * `-e`	esiste
   * `-f`	file regolare
   * `-L`	esiste ed è un symbolic link
   * `-r`	esiste ed è permessa la lettura
   * `-s`	esiste ed ha size > 0
   * `-w`	esiste ed è permessa la scrittura	
   * `-x`	esiste ed è permessa l'esecuzione
 * Opzioni binarie per file:	
   * `-ef`	stesso device ed inode
   * `-nt`	modification date più nuova
   * `-ot`	modification date più vecchia
 * Leganti:
   * `$$	and`
   * `|| 	or`

## if/fi
if permette di definire le condizioni così:
`if <CONDITION>; then <COMMAND> fi`

### else
Possibile anche l'else:
`if <CONDITION>; then <COMMAND> else <COMMAND> fi`

### elif
Possibile l'else if
`if <CONDITION>; then <COMMAND> elif <CONDITION>; then <COMMAND> else <COMMAND> fi`

## Case statement
Possibile il multi condizionale:
`case <VALUE> in <PATTERN1>) <COMMAND> ;; <PATTERN2>) <COMMAND>;; *) <DEFCOMMAND> ;; esac`

## export
Permette di esportare variabili all'environment

## Differenza tra profile e RC
Solitamente, usiamo profile per settare ed esportare l'environment, RC per eseguire comandi
profile è eseguito alla login shell, RC ogni volta che una shell è creata, login o non login

## alias
alias permette di definire comandi personalizzati
Per renderli permanenti, li aggiungiamo a .bashrc

## Funzioni
possiamo definire semplici funzioni con la notazione classica di C
Definirle in `.bashrc` equivale ad usarle come comandi

# Firewall
 * firewalld è il metodo di default di CentOS7 per gestire host-level firewalls. 
 * Separa l'incoming traffic in zone, ognuna avente il suo set di regole.
 * In ordine, matcha: indirizzo source, interfaccia, default.
 * La zona di default è di solito la zona public. 
 * Configurazioni di default:
   * `trusted`
   * `home`
   * `internal`
   * `work`
   * `public`
   * `external`
   * `dmz`
   * `block`	
   * `drop`
Modifichiamo firewalld con `firewall-cmd` o con i file `/etc/firewalld`. (meglio la prima)

## firewall-cmd
 * Opzioni:
   * `--get-default-zone` auto-explainatory
   * `--set-default-zone`=ZONE cambia sia runtime che permanent
   * `--get-zones` lista tutte le zone disponibili
   * `--get-services` lista i servizi predefiniti
   * `--add-source=<CIDR>`  routa tutto il traffico dall'ip alla zona specificata con `--zone=<>`, o default
   * `--remove-source=<CIDR>` rimuove la rule legata al CIDR. possiamo specificare la zona da rimuovere
   * `--add-interface=<INT>` routa tutto il traffico dall'interfaccia alla zona, se non spec. default
   * `--change-interface=<INT>` associa l'interfaccia con la zona specificata al posto di quella di ora
   * `--list-all` lista tutte le interfacce, source, servizi, porte per `--zone=<ZONE`>
   * `--list-all-zones` lista tutto per tutte le zone
   * `--add-service=<SERV>` permette il traffico a `<SERV>`, possibile specificare `--zone`
   * `--add-port=<PORT>` permette traffico alla porta/protocollo. Permette `--zone`
   * `--remove-service=<SERV>` rimuove il servizio dai permessi della zona specificata
   * `--remove-port=<PORT>` rimuove la porta/protocollo dai permessi della zona specificata
   * `--reload` droppa la configurazione runtime e carica la persistent
   * `--add-rich-rule='RULE'` aggiunge una rich rule
   * `--remove-rich-rule='R'` rimuove una rich rule
   * `--query-rich-rule='RULE'` ritorna 0 se la rule è presente nella zona, 1 se no
   * `--list-rich-rules` outputta tutte le tule nella zona specificata
  
## Rich rules
Le rich rules permettono sintassi più complesse per le rules del firewall.
* Struttura: 
  * `rule`
  * `[source]`
  * `[destination]`
  * `service|port|protocol|icmp-block|masquerade|forward-port`
  * `[log]`
  * `[audit]`
  * `[accept|reject|drop]`
Per altre regole, firewalld.richlanguage(5)
Possiamo usare le rich rules anche per loggare roba

### Ordine delle rules
L'ordinamento basic prevede
  * `[port farwarding/masquerading]`
  * `[logging]`
  * `[deny]`
  * `[allow]`
Il primo match vince. Se un pacchetto non matcha nulla, di solito è denied.
Le direct rules vengono parsate prima di tutte.	

### Logging con rich rules
 * Possiamo loggare le connessioni a syslog o al kernel audit.
 * La sintassi basic per syslog è `log [prefix=""] [level=<LEV>] [limit value="<RATE/DURATION>"]`
  * Con loglevel= emerg, alert, crit, error, warning, notice, info, debug
  * duration in s, m, h, d (es. 3/m è 3 al minuto) * 
 * Sintassi simile è usabile con audit: `audit [limit value="<RATE/DURATION>"]`

### NAT con firewalld
firewalld supporta due tipi di NAT: masquerading (IPv4 only) e port forwarding.
Col primo inoltriamo pacchetti non diretti a noi ad un altro IP, modificandone la source. Quando la risposta arriva, li modifichiamo di nuovo con la destination giusta.
attiviamo con `firewall-cmd --permanent --zone=<ZONE> --add-masquerade` o con rich rules.

### Port forwarding
Questo, invece, inoltra il traffico di una porta ad un'altra porta, interna o esterna
Per attivarlo, `firewall-cmd --permanent --zone=<ZONE> --add-forward-port=port=<PORTNO>:proto=<PROTOCOL>[:toport=<PORTNO>][:toaddr=<IPADDR>]`
Uno dei due tra toport e toaddr va messo.

# Apache HTTPD
Il webserver Apache implementa un server con supporto http, estensibile con moduli. 
Una volta installato httpd-manual (incluso in httpd), e httpd.service avviato, basta vedere localhost/manual
La configurazione default è in `/etc/httpd/conf/httpd.conf`. Questa serve i contenuti di `/var/www/html`
Per attivare `apache systemctl enable httpd.service`, poi `systemctl start httpd.service`
Bisogna poi aprire firewalld:`firewall-cmd --permanent --add-service=http --add-service https` e ricaricarlo `firewall-cmd --reload`

## Virtual Hosts
I virtual hosts permettono di hostare più domini sullo stesso apache, in base a ip o nome.
Per comodità, definiamo i virtual hosts in file a parte in `/etc/httpd/conf.d/site1.conf`

## `genkey`
* genera un set di chiavi per TLS con il parametro obbligatorio <FQDN>, il nome di dominio completo.
* chiede inoltre una lunghezza della chiave (scegliere minimo 2048 bits)
* sputa fuori 3 file:
  * `<fqdn>.key` (chiave privata, deve avere perm 0600 o 0400)
  * `<fqdn>.csr` file necessario per richiedere la firma a una CA
  * `<fqdn>.crt` il certificato pubblico per i self-signed. Permessi 0644

### HTTPS su Apache
Dobbiamo prima installare il modulo mod_ssl, che genererà una configurazione default `ssl.conf`

# Docker
## Comandi
* `run <IMAGE>` esegue un'image in un container, pullandola se necessario. Dopo, esegue il comando scritto, se non c'è nulla esce subito.
  * `-it` crea una shell interattiva
  * `-e` aggiunge un'env
  * `-P` pubblica a porte random
  * `-p` pubblica a porte arbitrarie
  * `--name` sceglie un nome
  * `-v` binda un volume (-v local/folder:container/folder oppure /contfolder per Docker volume)
* `ps` visualizza tutti i container attivi
  * `-a` mostra anche i non attivi
* `images` mostra le images salvate localmente
* `search` cerca images su docker hub
* `rm <HASH/NAME>` elimina un container stoppato
* `rmi <IMAGE>` elimina un'image
* `stop` stoppa un container
* `rename <NOME1> <NOME2>` rinomina un container	
* `pull` pulla un'immagine da registry
* `save -o <filename>.tar <NOMECONT>` esporta l'image ad un archivio
* `port` mostra le porte associate

## Utilizzo con nginx
Per avviare nginx: `docker run --detach -p 80:80 --name web nginx:latest`
