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
* `ctrl+e`	fine della command line
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

## Comandi
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

### $HOME
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