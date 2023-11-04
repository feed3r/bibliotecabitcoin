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
In questo articolo proponiamo procedure computazionalmente accettabili per apporre una firma temporale digitale su tali documenti, in modo che sia impossibile per un utente retrodatare o postdatare il proprio documento, anche con la collusione di un sistema di firma temporale.  
Le nostre procedure mantengono la completa privacy del contenuto dei documenti stessi e non richiedono alcuna registrazione da parte del servizio di firma temporale. 

La gloria del tempo è conciliare i re avversari,  
Svelare il falso e far uscire il vero,  
Porre il suo sigillo su ciò che è antico,  
Destare il giorno e sorvegliare la notte,  
Per far torto a chi fa torto, fino al pentimento. 

William Shakespeare - Lo Stupro di Lucrezia

## 1 - Introduzione

In molte situazioni è necessario certificare la data di creazione o di ultima modifica di un documento. Ad esempio, in materia di proprietà intellettuale, a volte è fondamentale verificare la data in cui un inventore ha per la prima volta messo per iscritto un'idea brevettabile, al fine di stabilirne la precedenza rispetto ad altre rivendicazioni. 
Una procedura attualmente accettata per creare la firma temporale di un'idea scientifica prevede l'annotazione giornaliera del proprio lavoro in un quaderno di laboratorio. Le annotazioni datate vengono inserite una dopo l'altra nel quaderno, senza lasciare alcuna pagina vuota. Le pagine del quaderno, numerate in modo sequenziale e poi cucite, rendono difficile manomettere le registrazioni senza lasciare segni evidenti.
Se il quaderno viene poi vidimato regolarmente da un funzionario pubblico, oppure rivisto e firmato da un dirigente aziendale, la validità del credito è ulteriormente rafforzata. 
Se l'anteriorità delle idee dell'inventore viene contestata in un secondo momento, sia l'evidenza fisica del taccuino che la procedura stabilita servono a dimostrare che è stato l'inventore ad aver avuto l'idea in una certa data o prima di essa.  
Ci sono altri metodi di firma temporale: ad esempio si può spedire una lettera a se stessi e non aprirla. In questo modo si garantisce che la lettera allegata è stata creata prima della data del timbro postale apposto sulla busta. Le aziende incorporano procedure più elaborate nei loro processi quotidiani aumentare la credibilità dei documenti interni, nel caso in cui vengano contestati in un secondo momento. Per esempio, questi metodi possono garantire che i documenti siano gestiti da più di una persona, in modo da evitare che qualsiasi manomissione di un documento da parte di una persona sarà rilevata da un'altra. Tutti questi metodi, tuttavia, si basano su due presupposti. In primo luogo, i documenti possono essere esaminati per trovare eventuali segni di manomissione. In secondo luogo, esiste un'altra parte che visiona il documento la cui integrità e imparzialità è vista come garante della richiesta.   

Riteniamo che questi presupposti siano messi in seria discussione nel caso di documenti creati e conservati esclusivamente in forma digitale. Questo perché i documenti elettronici digitali sono estremamente facili da manomettere, inoltre la modifica non lascia alcun segno sul supporto fisico. Si rende necessario, quindi, un metodo di firma temporale dei documenti con le seguenti due proprietà: 

- in primo luogo, si deve trovare un modo per datare i dati stessi, senza fare affidamento sulle caratteristiche del supporto su cui questi appaiono, in modo che sia impossibile modificare anche un solo bit del documento senza che la modifica sia evidente. 

- in secondo luogo, dovrebbe essere impossibile firmare un documento con una data e ora diverse da quelle attuali.  

Lo scopo di questo articolo è introdurre una soluzione matematicamente valida e computazionalmente plausibile al problema della firma temporale.  

Nelle sezioni che seguono, presenteremo una prima soluzione semplice al problema: la cassetta di sicurezza digitale.  
Questa soluzione ha la funzione didattica di evidenziare le ulteriori difficoltà associate alla firma temporale digitale, oltre a quelle riscontrate nei metodi convenzionali di firma temporale.  
I successivi miglioramenti di questa semplice soluzione conducono infine a delle implementazioni pratiche della firma temporale digitale. 

## 2 - Ambiente di esecuzione

L'ambientazione del nostro problema è una rete distribuita di utenti, che può rappresentare un insieme di individui, aziende diverse o divisioni all'interno della stessa azienda. Ci riferiremo agli utenti come clienti. Ogni cliente ha un numero di identificazione unico.  

La soluzione al problema della firma temporale è costituita da diverse parti: esiste una procedura da eseguire quando un cliente appone la firma temporale su un documento. Il cliente deve poter verificare successivamente che la procedura sia stata eseguita correttamente e dovrebbe esistere anche una procedura per rispondere alle contestazioni di un terzo sulla validità della firma del documento.
Come per qualsiasi problema crittografico, è cruciale caratterizzare con precisione la sicurezza raggiunta da uno schema di firma temporale. Una buona soluzione al problema richiede che, sotto ragionevoli ipotesi riguardo la capacità computazionali degli utenti e la complessità computazionale del problema, sia estremamente difficile o impossibile produrre false firme temporali. Naturalmente, più deboli sono le assunzioni necessarie e meglio è.

## 3 - Una soluzione semplice

Una soluzione semplice, definita come "_cassetta di sicurezza digitale_", potrebbe funzionare come segue:

ogni volta che un cliente deve firmare temporalmente un documento, questo viene trasmesso a un servizio di firma temporale (TSS nel seguito - Time Stamping Service).  
Il servizio registra la data e l'ora in cui il documento è stato ricevuto e ne conserva una copia di sicurezza. Se l'integrità del documento del cliente viene messa in discussione, è possibile confrontarlo con la copia conservata dal TSS. Se le due copie sono identiche, è la prova che il documento non è stato manomesso dopo la data indicata nel TSS.  
Questa procedura soddisfa di fatto il requisito centrale per la firma temporale di un documento digitale. [^1] Questo approccio, tuttavia, solleva diverse preoccupazioni:

[^1]: Gli autori hanno appreso recentemente di una simile proposta da parte di Kanare [14]

- **Privacy**: Questo metodo compromette la riservatezza del documento in due modi:  

  - una terza parte potrebbe conoscere ed intercettare la trasmissione del documento  

  - dopo la trasmissione, inoltre, il documento è disponibile al TSS a tempo indeterminato. In questo modo il cliente non deve preoccuparsi soltanto della sicurezza del documento in suo possesso, ma anche di quella della copia depositata presso il TSS.

- **Banda di trasmissione e storicizzazione**: Sia la quantità di tempo necessaria per inviare un documento per la firma temporale che la quantità di memoria richiesta per la storicizzazione al TSS dipendono dalla dimensione del documento stesso. In questo modo, il costo in termini di tempo e spesa per firmare un documento di grandi dimensioni potrebbe risultare proibitivo. 





