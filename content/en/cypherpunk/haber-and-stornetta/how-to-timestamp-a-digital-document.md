---
title: "How to Time-Stamp a Digital Document"
description: "Pubblicato su Journal of Cryptology, vol 3, no 2, pagine 99-111, 1991"
date: 2023-11-04T12:48:23+01:00
draft: false
weight: 200
images: []
categories: []
contributors: ["feeder"]
---

[Originale](https://link.springer.com/content/pdf/10.1007/3-540-38424-3_32.pdf) - 1991

La prospettiva di un mondo in cui tutti i documenti di testo, audio, immagini e video sono in formato digitale su supporti facilmente modificabili solleva il problema di come fornire una certificazione sicura della data di creazione o di ultima modifica di un documento. Il problema è quello di datare i dati, e non il supporto.  
In questo articolo proponiamo procedure computazionalmente accettabili per apporre una marca temporale digitale su tali documenti, in modo che sia impossibile per un utente retrodatare o postdatare il proprio documento, anche con la collusione di un sistema di marca temporale.  
Le nostre procedure mantengono la completa privacy del contenuto dei documenti stessi e non richiedono alcuna registrazione da parte del servizio di marca temporale. 

La gloria del tempo è conciliare i re avversari,  
Svelare il falso e far uscire il vero,  
Porre il suo sigillo su ciò che è antico,  
Destare il giorno e sorvegliare la notte,  
Per far torto a chi fa torto, fino al pentimento. 

William Shakespeare - Lo Stupro di Lucrezia

## 1 - Introduzione

In molte situazioni è necessario certificare la data di creazione o di ultima modifica di un documento. Ad esempio, in materia di proprietà intellettuale, a volte è fondamentale verificare la data in cui un inventore ha per la prima volta messo per iscritto un'idea brevettabile, al fine di stabilirne la precedenza rispetto ad altre rivendicazioni. 
Una procedura attualmente accettata per creare la marca temporale di un'idea scientifica prevede l'annotazione giornaliera del proprio lavoro in un quaderno di laboratorio. Le annotazioni datate vengono inserite una dopo l'altra nel quaderno, senza lasciare alcuna pagina vuota. Le pagine del quaderno, numerate in modo sequenziale e poi cucite, rendono difficile manomettere le registrazioni senza lasciare segni evidenti.
Se il quaderno viene poi vidimato regolarmente da un funzionario pubblico, oppure rivisto e firmato da un dirigente aziendale, la validità del credito è ulteriormente rafforzata. 
Se l'anteriorità delle idee dell'inventore viene contestata in un secondo momento, sia l'evidenza fisica del taccuino che la procedura stabilita servono a dimostrare che è stato l'inventore ad aver avuto l'idea in una certa data o prima di essa.  
Ci sono altri metodi di marca temporale: ad esempio si può spedire una lettera a se stessi e non aprirla. In questo modo si garantisce che la lettera allegata è stata creata prima della data del timbro postale apposto sulla busta. Le aziende incorporano procedure più elaborate nei loro processi quotidiani aumentare la credibilità dei documenti interni, nel caso in cui vengano contestati in un secondo momento. Per esempio, questi metodi possono garantire che i documenti siano gestiti da più di una persona, in modo da evitare che qualsiasi manomissione di un documento da parte di una persona sarà rilevata da un'altra. Tutti questi metodi, tuttavia, si basano su due presupposti. In primo luogo, i documenti possono essere esaminati per trovare eventuali segni di manomissione. In secondo luogo, esiste un'altra parte che visiona il documento la cui integrità e imparzialità è vista come garante della richiesta.   

Riteniamo che questi presupposti siano messi in seria discussione nel caso di documenti creati e conservati esclusivamente in forma digitale. Questo perché i documenti elettronici digitali sono estremamente facili da manomettere, inoltre la modifica non lascia alcun segno sul supporto fisico. Si rende necessario, quindi, un metodo di marca temporale dei documenti con le seguenti due proprietà: 

- in primo luogo, si deve trovare un modo per datare i dati stessi, senza fare affidamento sulle caratteristiche del supporto su cui questi appaiono, in modo che sia impossibile modificare anche un solo bit del documento senza che la modifica sia evidente. 

- in secondo luogo, dovrebbe essere impossibile firmare un documento con una data e ora diverse da quelle attuali.  

Lo scopo di questo articolo è introdurre una soluzione matematicamente valida e computazionalmente plausibile al problema della marca temporale.  

Nelle sezioni che seguono, presenteremo una prima soluzione semplice al problema: la cassetta di sicurezza digitale.  
Questa soluzione ha la funzione didattica di evidenziare le ulteriori difficoltà associate alla marca temporale digitale, oltre a quelle riscontrate nei metodi convenzionali di marca temporale.  
I successivi miglioramenti di questa semplice soluzione conducono infine a delle implementazioni pratiche della marca temporale digitale. 

## 2 - Ambiente di esecuzione

L'ambientazione del nostro problema è una rete distribuita di utenti, che può rappresentare un insieme di individui, aziende diverse o divisioni all'interno della stessa azienda. Ci riferiremo agli utenti come clienti. Ogni cliente ha un numero di identificazione unico.  

La soluzione al problema della marca temporale è costituita da diverse parti: esiste una procedura da eseguire quando un cliente appone la marca temporale su un documento. Il cliente deve poter verificare successivamente che la procedura sia stata eseguita correttamente e dovrebbe esistere anche una procedura per rispondere alle contestazioni di un terzo sulla validità della firma del documento.
Come per qualsiasi problema crittografico, è cruciale caratterizzare con precisione la sicurezza raggiunta da uno schema di marca temporale. Una buona soluzione al problema richiede che, sotto ragionevoli ipotesi riguardo la capacità computazionali degli utenti e la complessità computazionale del problema, sia estremamente difficile o impossibile produrre false firme temporali. Naturalmente, più deboli sono le assunzioni necessarie e meglio è.

## 3 - Una soluzione semplice

Una soluzione semplice, definita come "_cassetta di sicurezza digitale_", potrebbe funzionare come segue:

ogni volta che un cliente deve firmare temporalmente un documento, questo viene trasmesso a un servizio di marca temporale (TSS nel seguito - Time Stamping Service).  
Il servizio registra la data e l'ora in cui il documento è stato ricevuto e ne conserva una copia di sicurezza. Se l'integrità del documento del cliente viene messa in discussione, è possibile confrontarlo con la copia conservata dal TSS. Se le due copie sono identiche, è la prova che il documento non è stato manomesso dopo la data indicata nel TSS.  
Questa procedura soddisfa di fatto il requisito centrale per la marca temporale di un documento digitale. [^1] Questo approccio, tuttavia, solleva alcune problematiche:

[^1]: Gli autori hanno appreso recentemente di una simile proposta da parte di Kanare [14]

- **Privacy**: Questo metodo compromette la riservatezza del documento in due modi:  

  - una terza parte potrebbe conoscere ed intercettare la trasmissione del documento  

  - dopo la trasmissione, inoltre, il documento è disponibile al TSS a tempo indeterminato. In questo modo il cliente non deve preoccuparsi soltanto della sicurezza del documento in suo possesso, ma anche di quella della copia depositata presso il TSS.

- **Banda di trasmissione e storicizzazione**: Sia la quantità di tempo necessaria per inviare un documento per la marca temporale che la quantità di memoria richiesta per la storicizzazione al TSS dipendono dalla dimensione del documento stesso. In questo modo, il costo in termini di tempo e spesa per firmare un documento di grandi dimensioni potrebbe risultare proibitivo. 

- **Incompetenza**: La copia del documento di competenza del TSS potrebbe essere danneggiata durante la trasmissione, potrebbe essere firmata in maniera errata da parte del TSS, oppure potrebbe essere danneggiata o persa del tutto in qualsiasi momento durante la sua conservazione.  
Ognuno di questi eventi invaliderebbe la rivendicazione della marca temporale da parte del cliente.

- **Fiducia**: Il probiema fondamentale rimane: nulla in questa soluzione impedisce al TSS di accordarsi con un cliente per firmare il documento con una data e ora diversa da quella attuale.

Nella prossima sezione dell'articolo presentiamo una soluzione che affronta i primi tre problematiche presentate sopra. L'ultimo problema, la fiducia, sarà affrontato separatamente in una sezione dedicata di opportuna lunghezza.  

## 4 - Un servizio fidato di Marca Temporale

In questa sezione assumiamo che il TSS sia affidabile, e pesentiamo due miglioramenti rispetto alla soluzione semplice presentata in precedenza.

### 4.1 - Hash

La prima semplificazione consiste nel fare uso di una famiglia di _funzioni Hash senza collisioni_.  

Si tratta di una famiglia di funzioni $h$ : $\{ 0,1 \} ^*$ &rarr; $\{ 0, 1 \} ^l$ che comprimono stringhe di bit di lunghezza arbitraria a stringhe di bit di lunghezza fissa $l$, con le seguenti proprietà:

1. le funzioni $h$ sono computazionalmente semplici da eseguire ed è facile scegliere casualmente una tra le funzioni della famiglia.

2. data una funzione $h$ è computazionalmente impossibile trovare una coppia distinta di stringhe $x$ , $x^1$ che soddisfino la proprietà $h(x) = h(x^1)$. L'esistenza di tale coppia è definita $collisione$.

L'importanza pratica di queste funzioni è nota da molto tempo e sono state utilizzate dai ricercatori in numerose applicazioni, ad esempio [7, 15, 16].

Naor e Yung hanno definito la nozione simile di "funzioni hash universali unidirezionali".
che soddisfano, al posto della seconda condizione di cui sopra, il requisito leggermente più debole che sia computazionalmente impossibile, data una stringa $x$, calcolare un'altra stringa $x' \neq x$ che soddisfi $h(x) = h(x')$ per una qualsiasi $h$ scelta casualmente e sono stati in grado di creare tali funzioni partendo dal presupposto che esistano funzioni unidirezionali uno-a-uno [17].
Rompel ha recentemente dimostrato che tali funzioni esistono se esiste almeno una funzione unidirezionali. Per una discussione sulle differenze tra questi due tipi di funzioni hash crittografiche, si veda il paragrafo 6.3.

Esistono implementazioni pratiche di funzioni hash, ad esempio quella di Rivest [19], che sembrano essere ragionevolmente sicure.
Utilizzeremo le funzioni di hash come segue. Invece di trasmettere il proprio documento $x$ al TSS, un client invierà invece il suo valore di hash $h (x) = y$. 
Ai fini dell'autenticazione, la marca temporale di $y$ è equivalente alla marcatura temporale di $x$. Questo riduce notevolmente sia il problema della larghezza di banda e che di memorizzazione, risolvendo anche il problema della privacy.
A seconda degli obiettivi di progettazione di un'implementazione del sistema di marca temporale,
può esistere un'unica funzione di hash utilizzata da tutti i clienti, o diverse funzioni di hash per i diversi clienti. 
Per il resto di questo documento, parleremo di valori hash di marca temporale $y$ come stringhe casuali di bit di lunghezza fissa.
Parte della procedura di convalida di una marca temporale
sarà quella di produrre il documento pre-immagine $x$ che soddisfi $h(x) = y$; l'incapacità di produrre tale documento invalida la marca temporale.

### 4.2 Firma

Il secondo miglioramento si avvale delle firme digitali.  
Informalmente, uno schema di firma
è un algoritmo che consente a un soggetto, il firmatario, di etichettare i messaggi in modo da identificare univocamente il firmatario.
Le firme digitali sono state proposte da Rabin e da Diffie e Hellman [18, 71].  
Dopo una lunga serie di articoli di molti autori, Rompel [20] ha dimostrato che l'esistenza di funzioni unidirezionali può essere sfruttata per progettare uno schema di firma che soddisfi la nozione di sicurezza molto forte definita per la prima volta da Goldwasser, Micali e Rivest [10].  
Con uno schema di firma sicuro disponibile, quando il TSS riceve il valore di hash aggiunge la data e l'ora, quindi firma il documento appena prodotto e lo invia al cliente.
Controllando la firma, il cliente ha la certezza che il TSS ha effettivamente elaborato la richiesta, che l'hash è stato ricevuto correttamente e che è stata inserita l'ora corretta.  
In questo modo si risolve il problema dell'incompetenza presente e futura del TSS e riduce la necessità del TSS stesso di memorizzare i documenti.


## 5 Due schemi di Marca Temporale  

<p align="center"><em>Sed quis custodiet apsos Custodes?<br>
Juvenal, c. 100 A.D.</em></p>
<p align="center">Ma chi sorveglia i sorveglianti stessi?</p>  

Quello che abbiamo descritto finora è, a nostro avviso, un metodo pratico per la marcatura temporale di documenti digitali di lunghezza arbitraria.  
Tuttavia, né la firma né l'uso di funzioni hash impediscono in qualche modo a un servizio di marcatura temporale di emettere un marchio falso.  
Idealmente, vorremmo un meccanismo che garantisca che, per quanto il TSS sia senza scrupoli, i tempi che certifica saranno sempre quelli corretti e che non sia in grado di emettere marchi errati anche se ci prova.  

Può sembrare difficile specificare una procedura di marcatura temporale in modo da rendere impossibile la produzione di false marche temporali. Dopotutto, se l'output di un algoritmo $A$, dato in ingresso un documento $x$ e alcune informazioni temporali $t$, è una stringa di bit $c = A ( x , t )$ che rappresenta una legittima marca temporale per $x$, cosa impedisce a un falsario, qualche tempo dopo di calcolare le stesse informazioni temporali $t$ e di eseguire $A$ per produrre lo stesso certificato $c$? La domanda è pertinente anche se $A$ è un algoritmo probabilistico.

Il nostro compito può essere visto come il problema di simulare l'azione di un TSS fidato, in assenza di parti fidate. Esistono due approcci piuttosto diversi
che possiamo adottare, e ognuno di questi porta a una soluzione. Il primo approccio consiste nel vincolare un TSS centralizzato, forse non fidato, a produrre timestamp autentici, in modo tale che sia sufficientemente difficile produrne di falsi.  
Il secondo approccio consiste nel distribuire in qualche modo la fiducia richiesta tra gli utenti del servizio. Non è chiaro se uno di questi due approcci sia possibile.

### 5.1 Collegamento

La nostra prima soluzione parte dall'osservazione che la sequenza dei clienti che richiedono le marche temporali ed i valori hash che inviano non possono essere conosciuti in anticipo. Quindi, se il TSS include i bit della precedente sequenza di richieste del cliente nel certificato firmato, allora sappiamo che il timestamp si è verificato dopo queste richieste. Ma il requisito di includere nel certificato i bit dei documenti precedenti può essere utilizzato anche per risolvere il problema di vincolare il tempo nell'altra direzione, perché la società di marcatura temporale
non può emettere certificati successivi a meno di avere disponibile la richiesta attuale.

Descriviamo due varianti di questo schema di collegamento; la prima, leggermente più semplice, evidenzia la nostra idea principale, mentre la seconda può essere preferibile nella pratica.  
In entrambe le varianti, il TSS farà uso di una funzione hash priva di collisioni, da indicare con $H$. Questa procedura deve essere aggiunta all'uso di funzioni hash da parte dei clienti per produrre il valore hash di ogni documento di cui si desidera avere la marca temporale.  
Per essere precisi, una richiesta di marcatura temporale consiste in una stringa di $l$ bit $y$ (presumibilmente il valore di hash del documento) e un numero di identificazione del cliente $ID$. Useremo la notazione $\sigma(\cdot)$ per indicare la procedura di firma utilizzata dal TSS. Il TSS emette $certificati$ firmati, sequenzialmente numerati. In risposta alla richiesta $(y_n, ID_n)$ del nostro cliente,
la $n-esima$ richiesta in sequenza, il TSS esegue due operazioni:

1. Il TSS invia al nostro cliente il certificato firmato $s = \sigma(C_n)$, dove il certificato  

<p align="center"> $C_n = (n, t_n, ID_n, y_n, L_n)$ </p>

è composto dal numero di sequenza n, dall'ora t, dal numero ID del cliente e dal valore di hash $y_n$ della richiesta, e da alcune informazioni di collegamento che provengono dal certificato precedentemente emesso: $L_n = ( t_n-1, ID_n-1, y_n-1, H(L_n-1))$.

2. Quando la richiesta successiva è stata elaborata, il TSS invia al nostro cliente il numero di identificazione $ID_n+1$ per la richiesta successiva.

Dopo aver ricevuto $s$ e $ID_n+1$ dal TSS, il cliente verifica che $s$ sia una firma valida di un certificato valido, ad esempio che sia della forma corretta $(n, t, ID_n, L_n)$, contenente il tempo corretto t.

Se il suo documento x marcato temporalmente viene successivamente contestato, il contestatore controlla innanzitutto
che la marca temporale $( S , I D_n+1 )$ sia della forma corretta (con $S$ che sia la firma valida di un certificato contenente effettivamente l'hash di $x$).
Per garantire che il cliente interessato non abbia colluso con il TSS, il contestatore può chiamare il cliente $IDn+1$ e chiedergli di produrre il suo time-stamp $(s', ID_n+1)$ Questo include una firma

$ S' = \sigma(n + 1, t_n+1, ID_n+1, y_n+1;L_n+1 )$  

di un certificato che contiene nelle sue informazioni di collegamento $L_n+1$ una copia del suo valore hash
$y_n$. Questa informazione di collegamento è ulteriormente autenticata dall'inclusione dell'immagine $H ( L_n)$ delle informazioni di collegamento $L_n.
Uno contestatore particolarmente sospetto può ora chiamare $ID_n+2 e verificare la marca temporale successiva nella sequenza; questo può continuare per tutto il tempo che il contestatore desidera. Allo stesso modo, lo sfidante può anche seguire la catena delle marche temporali a ritroso, a partire dal client $ID_n-1$.

Perché questo vincola il TSS a non produrre firme temporali errate? In primo luogo, si osservi che l'uso della firma ha come effetto che _l'unico_ modo per falsificare una marca temporale è con la collaborazione del TSS.  
Ma il TSS non può post-datare un documento, perché il certificato deve contenere i bit delle richieste immediatamente precedenti al tempo desiderato, perchè il TSS non li ha ancora ricevuti. Il TSS non può retrodatare un documento preparando una falsa datazione per un'epoca precedente, perché i bit del documento in questione devono essere incorporati nei certificati immediatamente successivi a quel momento precedente, ma questi certificati sono già stati emessi. Inoltre, includere correttamente un documento firmato nel flusso già esistente di certificati con marca temporale richiede il calcolo di una collisione per la funzione hash H.  

Pertanto, l'unica possibilità di falsificazione è preparare una falsa catena di marche temporali, abbastanza lunga da esaurire il contestatore più sospetto che si preveda.

Nello schema appena descritto, i clienti devono conservare tutti i loro certificati. Al fine di
allentare questo requisito, nella seconda variante di questo schema colleghiamo ogni richiesta non solo alla
alla richiesta successiva, ma alle successive $k$ richieste. Il TSS risponde all'ennesima
come segue:

1. Come sopra, il certificato $C_n$ è nella forma $C_n = (n, t_n, ID_n, y_n,; L_n), dove ora l'informazione di collegamento $L_n$ è della forma:

$ L_n = [(t_n-k, ID_n-k, y_n-k, H(L_n-k)), ... , (t_n-1, ID_n-1, y_n-1, H(L_n-1))] $

2. Dopo che le successive $k$ richieste sono state processate, il TSS invia al nostro cliente la lista $(ID_n+1, ..., ID_n+k) $

Dopo aver verificato che la marca temporale di questo cliente è corretta, uno contestatore sospetto può chiedere a uno qualsiasi dei successivi $k$ clienti ID_n+i di produrre la propria marca temporale.
Come sopra, la sua marca temporale include la firma di un certificato che contiene nelle sue informazioni di collegamento $L_n+i$ una copia delle parti rilevanti pertinente del certificato con marca temporale contestata $Cn$, autenticata dall'inclusione del valore hash di $H$ delle informazioni di collegamento $L_n$ del cliente contestato.
La sua marca temporale include anche i numeri di cliente (ID_n+i+1, . . . , ID_n+i+k) $, di cui gli ultimi $i$ sono i nuovi; lo sfidante può chiedere a questi clienti le loro marche temporali, 
e questo può continuare per tutto il tempo che lo sfidante desidera.

Oltre ad alleggerire il requisito di salvataggio di tutti i certificati da parte dei clienti, questa seconda variante ha anche la proprietà di incorporare correttamente un nuovo documento nel flusso già esistente di certificati con data e ora richiede il calcolo di una collisione simultanea di k contemporaneamente una collisione $ampia k$ per la funzione hash $H$, invece di una semplice collisione a coppie.

##5.2 Fiducia distribuita

Per questo schema, si ipotizza che esista uno schema di firma sicuro in modo che ogni utente possa firmare i messaggi, e che un generatore di numeri pseudocasuale sicuro standard $G$ sia disponibile a tutti gli utenti. Un _generatore pseudocasuale_ è un algoritmo che, a partire da una breve sequenza di bit passata in ingresso (detta _seme_) genera in uscita sequenze di bit che sono indistinguibili da quelle generate da un qualsiasi algoritmo plausibile; in particolare, sono imprevedibili. Tali generatori sono stati studiati per la prima volta da Blum e Micali [2] e da Yao [22]; Impagliazzo, Levin e Luby hanno dimostrato che esistono se esiste un algoritmo di
hanno dimostrato che esistono se esistono funzioni unidirezionali [12].  

Ancora una volta, consideriamo un valore hash $y$ che il nostro cliente vorrebbe marchiare temporalmente. Egli utilizza $y$ come seme per il generatore pseudocasuale il cui risultato può essere interpretato come una $k-tupla$ di numeri di identificazione del cliente. 

$G(y) = (ID_1, ID_2, ..., ID_k)$

Il nostro cliente invia la sua richiesta $(y, ID)$ a ciascuno di questi clienti. In cambio riceve dal cliente $ID_j$ un messaggio firmato $s_j = /sigma_j( t, ID, y)$ che include il tempo $t$. La sua marca temporale è costituita da $(y, ID), (s_1,. . ., s_k)] . Le $k$ firme $s_j$, possono essere facilmente verificate dal nostro cliente o da un potenziale sfidante. Non sono necessarie ulteriori comunicazioni per
per rispondere a una sfida successiva.  

Perché un tale elenco di firme dovrebbe costituire una marca temporale credibile? Il motivo è che, in queste circostanze, l'unico modo per produrre un documento con una marca temporale valida ma con data e ora falsificate è usare un valore di hash $y$ in modo che $G(y)$ nomini $k$ clienti che sono disposti a collaborare per falsificare la data e l'ora. Se in un qualsiasi momento esiste al massimo una frazione costante $/epsilon$ di clienti potenzialmente  disonesti, il numero atteso di semi $y$ che devono essere provati prima di trovare una $k-tupla$ $G(y)$ contenente solo collaboratori tra questa frazione è $E^-k$. Inoltre, dal momento che abbiamo assunto che G sia un generatore pseudocasuale valido, non esiste un modo più veloce per trovare un seme conveniente $Y$
se non scegliendolo a caso. Questo ignora l'ulteriore problema dell'avversario, nella maggior parte degli scenari reali di trovare un documento plausibile che produca in modo conveniente un valore di hash $Y$.

Il parametro $k$ deve essere scelto in fase di progettazione del sistema in modo che questo sia
un calcolo non fattibile. Si osservi che anche una stima molto pessimistica della popolazione di clienti corruttibili $/epsilon$, che potrebbe essere del 90%, non comporta una scelta proibitiva di $k$. Inoltre, non è necessario che l'elenco dei clienti corruttibili sia fisso, purché la loro percentuale di corruzione non superi mai $/epsilon$ .

Questo schema non deve necessariamente utilizzare un TSS centralizzato. Gli unici requisiti sono che sia possibile comunicare con altri client a piacimento e ricevere da loro le firme richieste e che esista una lista pubblica di clienti in modo che sia possibile interpretare il risultato di $G(y)$ in modo standard come una $k-tupla$ di clienti. Un'implementazione pratica di questo metodo
un'implementazione pratica di questo metodo richiederebbe disposizioni nel protocollo per i clienti
che non possono essere contattati al momento della richiesta di marca-temporale. Ad esempio, per $k' < k$, il sistema potrebbe accettare risposte firmate da un qualsiasi $k'$ dei $k$ clienti
nominati da $G(y)$ come una marca temporale valida per $y$ (in questo caso sarebbe necessario un valore maggiore per il parametro k per ottenere la stessa bassa probabilità di trovare un insieme casuale di collaboratori).






 



