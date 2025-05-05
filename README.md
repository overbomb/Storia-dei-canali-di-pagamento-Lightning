# Storia dei canali di pagamento Lightining

Traduzione in italiano dell'articolo - Technical: A Brief History of Payment Channels: from Satoshi to Lightning Network by Alan Manuel k.Gloria

Sito dell'articolo: https://cryptowords.github.io/technical-a-brief-history-of-payment-channels


**12 Luglio 2019**

A chi importa dei tweet politici di qualche presidente di un paese a caso quando i canali di pagamento sono molto più interessanti e realmente capaci di trasmettere valore?

Quindi facciamo una breve storia delle varie tecnologie dei canali di pagamento.

**Generazione 0: Canali nSequence rotti di Satoshi**

La visione di Satoshi includeva i canali di pagamento, tranne che la sua implementazione era così scadente che abbiamo dovuto sistemarla e aggiungere RBF come sottoprodotto.

Originariamente, il piano per nSequence era che i nodi nella mempool avrebbero sostituito qualsiasi transazione che spendeva determinati input con un'altra transazione che spendeva gli stessi input, ma solo se il campo nSequence del sostituto era maggiore.

Poiché 0xFFFFFFFF era il valore più alto che nSequence poteva avere, questo avrebbe contrassegnato una transazione come "finale" e non sostituibile nella mempool.

In effetti, questo "canale nSequence" che descriverò è il motivo per cui abbiamo questa strana regola riguardo a nLockTime e nSequence. nLockTime funziona effettivamente solo se nSequence non è 0xFFFFFFFF, cioè finale. Se nSequence è 0xFFFFFFFF, allora nLockTime viene ignorato, perché questa è la versione "finale" della transazione.

Quindi, quello che faresti sarebbe qualcosa del genere:

1. Vai in un bar e prometti al barista di pagare entro la chiusura del bar. Poiché questo è l'universo di Bitcoin, il tempo è misurato in altezza di blocco, quindi l'orario di chiusura del bar è indicato come un'altezza di blocco futura.

2. Per il tuo primo drink, faresti una transazione pagando al barista per quel drink, pagando da alcune monete che hai. La transazione ha un nLockTime uguale all'orario di chiusura del bar e un nSequence iniziale di 0. Consegni la 
  transazione e il barista ti consegna il tuo drink.

3. Per il drink successivo, rifaresti la stessa transazione, aggiungendo il pagamento per quel drink all'output della transazione che va al barista (in modo che quell'output continui a crescere, in base all'importo del pagamento), e avendo 
  un nSequence che è uno più alto del precedente.

4. Alla fine, devi smettere di bere. Si riduce a una delle due possibilità:
   
 - Bevi fino alla chiusura del bar. Adesso che è giunto il momento specificato da nLockTime nella transazione, il barista può trasmettere l'ultima transazione e dire ai buttafuori di cacciarti fuori dal bar.
 - Oppure consideri saggiamente lo stato del tuo fegato. Quindi rifirmi l'ultima transazione con un nSequence "finale" di 0xFFFFFFFF, cioè il valore massimo che può avere. Questo consente al barista di ricevere immediatamente i suoi fondi      (nLockTime viene ignorato se nSequence è 0xFFFFFFFF), quindi lui o lei dice ai buttafuori di lasciarti uscire dal bar.
   
Ovviamente, questo è un canale di pagamento. I pagamenti individuali (acquisti di alcol, quindi immagino che comprare caffè non sia rilevante per i canali di pagamento). La chiusura avviene creando una transazione "finale" che è la somma dei pagamenti individuali. Certo, non c'è instradamento, i canali sono unidirezionali ed hanno una durata massima, ma dai un po' di tregua a Satoshi, stava anche inventando Bitcoin in quel momento.

Ora, se hai notato, ho chiamato questo tipo di canale di pagamento "rotto". Questo perché le regole del mempool non sono regole di consenso e non possono essere validate (nulla riguardo al mempool può essere validato on-chain: sospiro ogni volta che qualcuno propone "facciamo in modo che la dimensione del blocco dipenda dalla dimensione del mempool", lo stato del mempool non può essere validato dai dati on-chain). I nodi completi non possono vedere tutte le transazioni che hai firmato e poi convalidare che l'ultima con il massimo nSequence sia quella effettivamente utilizzata on-chain. Quindi puoi fare quanto segue:


1. Fai amicizia con Jihan Wu, perché possiede più del 51% della potenza di mining (ha davvero riorganizzato Bitcoin per invertire l'hack di Binance, giusto?).

2. Offri a Jihan Wu alcune delle bevande più interessanti che stai ordinando come incentivo a collaborare con te. Quindi, supponiamo che tu finisca per ordinare 100 drink, li dividi con Jihan Wu e gli dai 50 drink.

3. Quando il bar chiude, Jihan Wu chiama rapidamente il suo impianto di mining e dice loro di minare la versione della tua transazione con nSequence 0. Sai, quella prima in cui paghi solo per un drink.

4. Poiché i nodi completi non possono convalidare nSequence, accetteranno anche la versione nSequence=0 e la confermeranno, aggiungendo in modo immutabile il tuo pagamento per un singolo drink sulla blockchain.

5. Il barista, arrabbiato per essere stato truffato, estrae un fucile a pompa da sotto il bancone e spara a te e a Jihan Wu.

6. Jihan Wu usa i suoi poteri mistici di chi (in realtà i fumi combinati di tutti i suoi impianti di mining) per rallentare i proiettili del fucile, facendoli colpire te dolcemente come petali che fluttuano nel vento.

7. Il barista mormora alcune parole, i vestiti si strappano mentre lui o lei (difficile credere che possa essere una lei, ma hey) si trasforma in un orso, pronto a sbranarti per averlo truffato del pagamento per tutti i 100 drink che hai       ordinato da lui o lei.

8. Con uno sguardo deciso, ti fermi di fronte all'orso-barista, sfidandolo a toccarti. Hai visto "Revenant", sai che Leonardo DiCaprio potrebbe sopravvivere a un attacco di orso, e se un attore di classe può sopravvivere a questo, sai che      puoi farlo anche tu. Fai una posa. “Attacco logico del troll ubriaco!”

9. Penso di essermi distratto.


**Lezioni apprese?**

- Gli orsi portano cattive notizie.

- Non puoi ragionevolmente invocare "la Visione di Satoshi" e allo stesso tempo rifiutare il Lightning Network perché non è on-chain. La Visione di Satoshi includeva un'implementazione malfatta dei canali di pagamento con nSequence, dove la   transazione on-chain rappresentava più pagamenti logici, esattamente ciò che fanno le moderne tecniche off-chain (eccetto le moderne tecniche off-chain funzionano davvero). nSequence (il campo, ma non il suo significato moderno) è       
  presente in Bitcoin fin dalla versione BitCoin For Windows Alpha 0.1.0. E la sua intenzione originale erano i canali di pagamento. Non puoi avvicinarti di più alla Visione di Satoshi di un campo che Satoshi ha personalmente aggiunto alle 
  transazioni nella prima versione pubblica del software BitCoin, davvero.

- I miner possono totalmente bypassare le regole del mempool. Infatti, il motivo per cui nSequence è stato riproposto per indicare la "sostituzione opzionale tramite commissione" è perché i miner sono già incentivati dal sistema nSequence a   seguire sempre la sostituzione tramite commissione. Voglio dire, cosa pensi che siano quelle bevande che hai passato a Jihan Wu, se non la commissione che gli paghi per minare una versione specifica della tua transazione?

- Satoshi ha commesso errori. Il design originale per nSequence è uno di essi. Oggi, non usiamo più nSequence in questo modo. Quindi, deviare dal design originale di Satoshi fa parte dello sviluppo di Bitcoin, perché nel tempo apprendiamo     nuove lezioni che Satoshi non conosceva. Satoshi è stato un importante punto di riferimento in questa tecnologia. Non sarà l'ultimo, né il più importante, che ricorderemo in futuro: sarà solo il primo.

**Canali di Spilman**

Canale unidirezionale a tempo limitato compatibile con gli incentivi; o, la Visione di Satoshi, Fissa (se la malleabilità delle transazioni non fosse stata un problema, ovviamente).

Ora, sappiamo che il barista si trasformerà in un orso e ti sbranerà se cerchi di imbrogliare il canale di pagamento, e ora che abbiamo rivelato che sei buon amico con Jihan Wu, il barista non accetterà più uno schema di canale di pagamento che ti consente di collaborare con un miner per imbrogliare il barista.

Fortunatamente, Jeremy Spilman ha proposto un modo migliore che non ti permetterebbe di imbrogliare il barista.



Per prima cosa, tu e il barista eseguite questo rituale:


1. Ottieni dei fondi e crea una transazione che paga a un multisig 2-di-2 tra te e il barista. Non la trasmetti ancora: la firmi e ottieni il suo txid.
   
3. Crea un'altra transazione che spende la transazione precedente. Questa transazione (di “rimborso”) ha un nLockTime uguale all'orario di chiusura del bar, più un blocco. La firmi e dai questa transazione di "rimborso" (ma non la transazione precedente) al barista.
   
5. Il barista firma il "rimborso" e te lo restituisce. Ora è valida poiché spende un 2-di-2 tra te e il barista, e entrambi avete firmato la transazione di "rimborso".
  
7. Ora trasmetti la prima transazione on-chain. Tu e il barista aspettate che venga confermata profondamente, poi potete iniziare a ordinare.


Quello sopra è probabilmente vagamente familiare agli utenti del Lightning Network. È il processo di finanziamento dei canali di pagamento! La prima transazione, quella che paga a un multisig 2-di-2, è la transazione di finanziamento che sostiene i fondi del canale di pagamento.

Quindi ora inizi a ordinare in questo modo:

1. Per il tuo primo drink, crei una transazione che spende l'output della transazione di finanziamento e invia il prezzo del drink al barista, restituendo il resto a te.

2. Firmi la transazione e la passi al barista, che ti serve il tuo primo drink.

3. Per i drink successivi, ricrei la stessa transazione, aggiungendo il prezzo del nuovo drink alla somma che va al barista e riducendo la somma restituita a te. Firmi la transazione e la dai al barista, che ti serve il tuo drink successivo.
   
4. Infine:
   
  - Se si raggiunge l'orario di chiusura del bar, il barista firma l'ultima transazione, completando le necessarie 2 di 2 firme e trasmettendo questa al network di Bitcoin. Poiché la transazione di "rimborso" è l'orario di chiusura + 1, non     può essere utilizzata all'orario di chiusura.
    
 - Se decidi di voler andare via presto perché il tuo fegato sta piangendo, puoi semplicemente dire al barista di procedere e chiudere il canale (cosa che il barista può fare in qualsiasi momento semplicemente firmando e trasmettendo 
   l'ultima transazione: il barista non lo farà perché spera che tu rimanga e beva di più).
   
 - Se alla fine sei rimasto semplicemente al bar senza mai ordinare, allora all'orario di chiusura + 1 trasmetti la transazione di "rimborso" e recuperi i tuoi fondi per intero.

Ora, anche se passi 50 drink a Jihan Wu, non puoi dargli la prima transazione (quella che paga solo per un drink) e chiedergli di minarla: sta spendendo un 2-di-2 e la copia che hai contiene solo la tua firma. Hai bisogno della firma del barista per renderla valida, ma lui o lei non collaborerà mai in qualcosa che gli farebbe perdere denaro, quindi una firma del barista che convalida uno stato precedente in cui lui o lei viene pagato di meno non accadrà.

Quindi, problema risolto, giusto? Giusto? Ok, proviamo. Quindi ottieni i tuoi fondi, li metti in una transazione di finanziamento, ottieni la transazione di "rimborso", confermi la transazione di finanziamento...

Una volta che la transazione di finanziamento viene confermata profondamente, il barista ride fragorosamente. Lui o lei chiama i buttafuori, che ti circondano minacciosamente.

“Ti rifiuto il servizio,” dice il barista.

“Va bene,” dici. “Stavo per andarmene comunque;” Sorridi. “Recupererò i miei soldi con la transazione di "rimborso" e scrivendo di quanto sia scadente il tuo servizio su Reddit, così ricevi karma negativo!”

“Non così in fretta,” dice il barista. La sua voce ti fa gelare le ossa. Sembra che il tuo sfruttamento del canale di pagamento nSequence di Satoshi sia ancora fresco nella sua mente. “Guarda il txid della transazione di finanziamento che è  stata confermata.”

“E che c'è di strano?” chiedi con nonchalance, mentre apri il tuo computer desktop e accedi a un esploratore blockchain affidabile.

Quello che vedi ti shocca.

“Cosa diavolo?! il txid è diverso! Tu hai cambiato la mia firma?? Ma come? Ho messo l'unica copia della mia chiave privata in una busta sigillata in una cassetta di ferro dentro a una cassaforte sepolta nel deserto del Gobi, protetta da un clan di nomadi che hanno dedicato le loro vite e quelle dei loro figli a mantenere la mia chiave privata al sicuro in perpetuo!”

“Non lo sapevi?” chiede il barista. “I componenti della firma sono solo numeri molto grandi. Il segno di uno dei componenti della firma può essere cambiato, da positivo a negativo, o da negativo a positivo, e la firma rimarrà valida. Chiunque può farlo, anche se non conosce la chiave privata. Ma poiché Bitcoin include le firme nella transazione quando genera il txid, questo piccolo cambiamento cambia anche il txid.” Lui o lei ride. “Dicono che lo sistemeranno separando le firme dal corpo della transazione. Dicono che questi tipi di malleabilità delle firme non influenzeranno più gli ID delle transazioni dopo aver fatto questo, ma scommetto che posso convincere il mio buon amico Jihan Wu a ritardare questo piano ‘SepSig’ per un bel po’. Un tipo amichevole, questo Jihan Wu, si scopre che tutto quello che dovevo fare era offrirgli 51 drink e lui era disposto a minare una transazione con i segni delle firme invertiti.” Il suo sorriso si allarga. “Temo che la tua transazione di "rimborso" non funzionerà più, poiché spende un txid che non esiste e non sarà mai confermato. Quindi ecco l'affare. Mi paghi il 99% dei fondi nella transazione di finanziamento, in cambio della mia firma sulla transazione che spende con il txid che vedi on-chain. Rifiuta, e perdi il 100% dei fondi e ogni altro HODLer, me compreso, beneficerà della riduzione dell'offerta di monete. Accetta, e ti terrai l'1%. Non perdo nulla se rifiuti, quindi non mi importerà se lo fai, ma considera la differenza tra ottenere nulla e ottenere l'1% dei tuoi fondi.” I suoi occhi brillano. “GENUFLETTI SUBITO.”
 
**Lezione appresa**

 - La vendetta è una brutta bestia.
 - La malleabilità delle transazioni è una bestia ancora più brutta. È per questo che abbiamo dovuto correggere il bug in SegWit. Certo, MtGox sosteneva di essere stata attaccata in questo modo perché qualcuno continuava a interferire con   
   le loro firme di transazione e così hanno perso traccia di dove fossero finiti i loro fondi, ma in realtà, il motivo principale per correggere la malleabilità delle transazioni era supportare i canali di pagamento.
 - Sì, includere le firme nell'hash che definisce infine il txid è stato un errore. Satoshi ne ha fatti molti di questi. Quindi stiamo solo ribadendo la lezione "Satoshi non era un essere infinito di saggezza infinita" qui. Satoshi ha solo  
   un lasciapassare per quanto sia fantastico Bitcoin.


**Canali Spilman protetti da CLTV**

Utilizzando CLTV per il ramo di backoff.

Questa variazione è semplicemente rappresentata dai canali Spilman, ma con la transazione di rimborso sostituita da un ramo di backoff nello SCRIPT a cui si paga. È diventato possibile solo dopo che OP_CHECKLOCKTIMEVERIFY (CLTV) è stato abilitato nel 2015.

Come abbiamo visto nella discussione sui Canali Spilman, la malleabilità delle transazioni significa che qualsiasi transazione offchain pre-firmata può essere facilmente invalidata invertendo il segno della firma della transazione di finanziamento mentre quest'ultima non è ancora confermata.

Questo può essere evitato semplicemente inserendo eventuali requisiti speciali in un ramo esplicito dello SCRIPT di Bitcoin. Ora, il ramo di backoff dovrebbe creare una durata massima per il canale di pagamento, e prima dell'introduzione di OP_CHECKLOCKTIMEVERIFY, questo poteva essere fatto solo avendo una transazione nLockTime pre-firmata.

Con CLTV, tuttavia, ora possiamo rendere espliciti i rami nello SCRIPT a cui la transazione di finanziamento paga.

Invece di pagare a un 2-di-2 per impostare la transazione di finanziamento, si paga a uno SCRIPT che è fondamentalmente “2-di-2, O questa firma singola dopo un tempo di blocco specificato”.

Con questo, non c'è una transazione di rimborso pre-firmata che si riferisce a un txid specifico. Invece, puoi creare la transazione di rimborso in un secondo momento, utilizzando qualsiasi txid sotto cui la transazione di finanziamento viene confermata. Poiché la transazione di finanziamento è immutabile una volta confermata, non è più possibile cambiare il txid successivamente.

**RETI DI MICROPAGAMENTI DI TODD**

Il vecchio modello hub-spoke (che non è come funziona oggi LN).

Uno dei predecessori più diretti della Lightning Network era il modello hub-spoke discusso da Peter Todd. In questo modello, invece di avere pagatori che hanno direttamente canali con i beneficiari, i pagatori e i beneficiari si connettono a un server centrale hub. Questo consente a qualsiasi pagatore di pagare qualsiasi beneficiario, utilizzando lo stesso canale per ogni beneficiario sull'hub. Allo stesso modo, questo consente a qualsiasi beneficiario di ricevere da qualsiasi pagatore, utilizzando lo stesso canale.

Ricordi l'esempio di Spilman sopra? Quando apri un canale con il barista, devi aspettare che la transazione di finanziamento venga confermata. Questo richiederà un'ora al massimo. Ora considera che devi creare canali per tutti quelli a cui vuoi pagare. Non è molto scalabile.

Quindi, il modello hub-spoke di Todd ha una “camera di compensazione” centrale che trasporta denaro dai pagatori ai beneficiari. Il progetto “Moonbeam” adotta questo modello. Naturalmente, questo rivela all'hub chi sono il pagatore e il beneficiario, e quindi l'hub può potenzialmente censurare le transazioni. In generale, però, si considerava che un hub avrebbe censurato in modo più efficiente semplicemente non mantenendo un canale con il pagatore o il beneficiario che desidera censurare (poiché il denaro che possedeva nel canale sarebbe semplicemente bloccato inutilmente se l'hub non elaborasse pagamenti verso/da un utente censurato).

In ogni caso, la capacità dell'hub centrale di monitorare i pagamenti significa che può sorvegliare il pagatore e il beneficiario, e poi vendere questi dati transazionali privati a terzi. Questa perdita di privacy sarebbe intollerabile oggi.

Peter Todd ha anche proposto che potrebbero esserci più hub che potrebbero trasportare fondi l'uno all'altro per conto dei loro utenti, fornendo una privacy leggermente migliore.

Un altro punto da notare è che al momento in cui tali reti sono state proposte, erano disponibili solo canali unidirezionali (Spilman). Pertanto, mentre si poteva essere un pagatore o un beneficiario, si doveva utilizzare canali separati per il proprio reddito rispetto alla propria spesa. Peggio ancora, se si voleva trasferire denaro dal proprio canale di reddito al proprio canale di spesa, si doveva chiudere entrambi e rimescolare il denaro tra di essi, entrambe attività onchain.


Rete Lightning Poon-Dryja

Canali bidirezionali a due partecipanti.

Il meccanismo del canale Poon-Dryja ha due proprietà importanti:

- Bidirezionale.
- Nessun limite di tempo.
  
Sia il canale originale di Satoshi che le due varianti di Spilman sono unidirezionali: c'è un pagatore e un beneficiario, e se il beneficiario desidera effettuare un rimborso o pagare per un servizio o prodotto diverso fornito dal pagatore, non può utilizzare lo stesso canale unidirezionale.

Il meccanismo Poon-Dryja, tuttavia, consente ai canali di essere bidirezionali: non sei né un pagatore né un beneficiario nel canale, puoi ricevere o inviare in qualsiasi momento, purché tu e la controparte del canale siate online.

Inoltre, a differenza delle varianti di Spilman, non c'è un limite di tempo per la durata di un canale. Puoi mantenere il canale aperto per tutto il tempo che desideri.

Entrambe le proprietà, insieme, formano una caratteristica di scalabilità molto potente che credo la maggior parte delle persone non abbia apprezzato. Con i canali unidirezionali, come accennato in precedenza, se guadagni e spendi attraverso la stessa rete di canali di pagamento, avresti canali separati per guadagnare e spendere. Dovresti quindi eseguire operazioni onchain per “invertire” le direzioni dei tuoi canali periodicamente. In secondo luogo, poiché i canali Spilman hanno una durata fissa, anche se non utilizzi mai nessuno dei canali, dovresti periodicamente “aggiornarlo” chiudendolo e riaprendolo.

Con i canali bidirezionali e a vita indefinita, puoi invece aprire alcuni canali quando inizi a gestire il tuo denaro, per poi chiuderli solo dopo che i tuoi avvocati hanno eseguito il tuo testamento su come il denaro nei tuoi canali venga diviso tra i tuoi eredi: questo comporta solo due transazioni onchain nell'intera vita. Questa è la potenzialmente molto potente caratteristica di scalabilità che consentono i canali bidirezionali e a vita indefinita.

Non discuterò la struttura della transazione necessaria per i canali bidirezionali Poon-Dryja — è complicata e puoi facilmente trovare spiegazioni con grafica accattivante altrove.

C'è una debolezza del Poon-Dryja che le persone tendono a trascurare (perché è stata risolta molto bene da /u/RustyReddit):

Devi memorizzare tutte le chiavi di revoca di un canale. Questo implica che stai memorizzando 1 chiave di revoca per ogni aggiornamento del canale, quindi se esegui milioni di aggiornamenti nel corso della tua vita, dovresti memorizzare diversi megabyte di chiavi, per un solo canale. /u/RustyReddit ha risolto questo problema richiedendo che le chiavi di revoca siano generate da una chiave di revoca “Seed”, e ogni chiave è semplicemente l'applicazione di SHA256 su quella chiave, ripetutamente. Ad esempio, supponiamo che ti dica che la mia prima chiave di revoca è SHA256(SHA256(seed)). Puoi memorizzarlo in uno spazio O(1). Poi, per la successiva revoca, ti dico SHA256(seed). Da SHA256(key), puoi calcolare tu stesso SHA256(SHA256(seed)) (cioè la chiave di revoca precedente). Quindi puoi ricordare solo l'ultima chiave di revoca, e da lì saresti in grado di calcolare ogni chiave di revoca precedente. Quando inizi un canale, esegui SHA256 sulla tua seed per diversi milioni di volte, poi usi il risultato come prima chiave di revoca, rimuovendo uno strato di SHA256 per ogni chiave di revoca che devi generare. /u/RustyReddit non solo ha ideato questo, ma ha anche suggerito una struttura di archiviazione efficiente O(log n), la shachain, in modo da poter cercare rapidamente qualsiasi chiave di revoca nel passato in caso di violazione. Le persone non parlano più realmente di questo problema di archiviazione delle revoche O(n) perché è stato risolto molto bene da questo meccanismo.

Un'altra cosa che voglio sottolineare è che, mentre il documento sulla Lightning Network e molte delle presentazioni precedenti si sono sviluppate dal vecchio modello hub-and-spoke di Peter Todd, la moderna Lightning Network arriva alla conclusione logica di rimuovere una netta separazione tra "hub" e "spoke". Qualsiasi nodo sulla Lightning Network può funzionare molto bene come hub per qualsiasi altro nodo. Pertanto, anche se potresti operare principalmente come "pagatore", "nodo di inoltro" o "ricevente", finisci comunque per essere almeno parzialmente un nodo di inoltro ("hub") sulla rete, almeno parte del tempo. Questo riduce notevolmente i problemi di privacy inerenti all'avere solo pochi nodi hub: i nodi di inoltro non possono ottenere dati significativamente utili dai pagamenti che passano attraverso di loro, perché la distanza tra il pagatore e il ricevente può essere così grande che è probabile che il pagatore finale e il ricevente finale possano essere chiunque sulla Lightning Network.


**lezioni apprese?**

 - Possiamo decentralizzare se ci impegniamo abbastanza!

 - “Gli hub sono cattivi” può diventare “gli hub sono buoni” se tutti sono un hub.

 - Le persone intelligenti possono risolvere problemi. È un po' il motivo per cui sono intelligenti.

**FUTURO**

Dopo la LN, ci sono anche i Canali di Micropagamento Duplex Decker-Wattenhofer (DMC). Questo post è già abbastanza lungo così com'è, LOL. Ma per ora, utilizza un nuovo “canale nSequence decrescente”, sfruttando le nuove semantiche di blocco temporale relativo di nSequence (non quella rotta originariamente da Satoshi). In realtà utilizza più di tali costrutti “nSequence decrescenti”, terminando in una coppia di canali Spilman, uno in entrambe le direzioni (quindi “duplex”). Forse ne parlerò un'altra volta.

La realizzazione che le costruzioni di canali potrebbero effettivamente contenere più costruzioni di canali al loro interno (nel modo in cui Decker-Wattenhofer inserisce una coppia di canali Spilman all'interno di una serie di “canali nSequence decrescenti”) ha portato al pensiero successivo dietro le fabbriche di canali Burchert-Decker-Wattenhofer. Fondamentalmente, potresti ospitare più costrutti di canali a due partecipanti all'interno di un costrutto “canale” multipartiutente più grande (cioè ospitare più canali all'interno di una fabbrica).

Inoltre, abbiamo la costruzione Decker-Russell-Osuntokun o “eltoo”. Sosterrei che questo è “nSequence fatto bene”. Scriverò di più su questo in seguito, perché questo post è già abbastanza lungo.

Lezioni apprese?

La scalabilità offchain di Bitcoin è più potente di quanto tu abbia mai pensato.
