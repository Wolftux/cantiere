---
name: cantiere
description: "Fase di design: da un'idea grezza al documento di design, poi si ferma. Comando: /cantiere [idea grezza]"
argument-hint: "<idea grezza, oppure cartella di materiale + idea>"
disable-model-invocation: true
---

# /cantiere: dall'idea grezza al documento di design

L'idea dell'utente è: $ARGUMENTS

Se l'idea è vuota, chiedi all'utente di scriverla in chat e aspetta.

/cantiere è la fase di design: porta l'idea fino al documento di design e si ferma. La produzione la fa /produci in una chat nuova, leggendo quel documento.

Come lavori:
- Esegui i passi in ordine, senza chiedere conferme tra un passo e l'altro, salvo dove il passo lo dice.
- Le domande scritte da questa skill usano il selettore a scelta multipla (AskUserQuestion): una domanda per volta, opzione consigliata per prima. Le skill che chiami (court, grilling) usano il loro formato.
- Prima del passo 0 controlla nella tua lista di skill di avere `court-skill:court` e `grilling` (anche come `mattpocock-skills:grilling`), da qualunque marketplace vengano. Court gira solo in Claude Code, perché usa python3 e sub-agent: se non sei in Claude Code, dillo e fermati. Per ogni skill che manca, scrivi una riga all'utente e installa solo quella dal marketplace cantiere con Bash: `claude plugin install court-skill@cantiere` per court, `claude plugin install mattpocock-skills@cantiere` per grilling. Poi chiedi all'utente di lanciare `/reload-plugins` (o di aprire una sessione nuova) e di rilanciare /cantiere, e fermati. Se l'installazione fallisce, mostra l'errore e fermati.

## Passo 0: riscrittura fedele e classificazione

1. **Riscrittura fedele.** Riscrivi l'idea in 3-6 righe ordinate, usando solo ciò che l'utente ha detto. Sotto, elenca i **buchi**: cosa manca per capire problema, destinatari, soluzione e vincoli. Mostra riscrittura e buchi e chiedi conferma (Confermo / Correggo). Se l'utente corregge, aggiorna e chiedi di nuovo.
2. **Ampiezza.** Se l'idea contiene più prodotti distinti (per esempio app + sito + corso), elencali e chiedi da quale partire. Il prodotto scelto fa tutto il flusso; gli altri vanno nella sezione "Fasi successive" del documento, ognuno con un suo /cantiere più avanti.
3. **Classificazione.**
   - Tipo: **Software** (app, sito, web app, tool, script), **Testo** (articolo, tesi, capitolo, documento, post lungo) o **Slide** (presentazione, deck, lezione).
   - Per Testo e Slide, registro: **accademico/scientifico** (tesi, articolo scientifico, abstract, poster) o **divulgativo** (post, blog, Instagram, materiale per corsi).
   - Per Software, interfaccia: **sì** (app web, sito, app mobile o desktop) o **no** (script, CLI, API).

   Se tipo, registro o interfaccia non si capiscono, fai una domanda. Comunica la classificazione in una riga.

Il passo è finito quando la riscrittura è confermata, il prodotto è uno solo e sono fissati il tipo e il registro (Testo, Slide) o l'interfaccia (Software).

## Passo 0b: materiale esistente (graphify, facoltativo)

Il passo vale solo se il lavoro parte da qualcosa che esiste già ed è corposo: un progetto software da estendere (repository con codice) o una cartella di materiale da usare come base (per esempio PDF di letteratura per una tesi). Per un'idea nuova senza materiale, passa al passo 1.

Se la skill `graphify` è disponibile, proponi con una domanda di eseguire `/graphify <cartella>`; nella domanda avvisa che documenti e PDF passano dal modello, quindi attenzione se la cartella contiene dati di pazienti. Se l'utente accetta:
- esegui `/graphify <cartella>` e leggi `GRAPH_REPORT.md`;
- nei passi successivi usa il grafo come contesto: interroga con query, path ed explain invece di rileggere i file;
- i collegamenti INFERRED sono dedotti dal modello: per lavori accademici verifica sul documento originale ogni collegamento che usi.

Se graphify manca, dillo in una riga e prosegui.

Il passo è finito quando il grafo è letto, oppure l'utente ha rifiutato, oppure il passo non si applica.

## Passo 1: interrogatorio breve

Scopo: dare all'idea la sostanza minima per un processo. Le domande le scrivi tu, partendo dai buchi del passo 0, su quattro temi in quest'ordine:
1. problema che risolve;
2. destinatari;
3. soluzione principale;
4. vincoli: tempo, competenze, strumenti, stack se già deciso.

Al massimo 8 domande in tutto; salta ciò che l'idea già dice. Resta sulla sostanza: i dettagli di design arrivano al passo 3.

Il passo è finito quando l'utente conferma con un clic un riassunto di 5-10 righe. Quel riassunto è il **caso** da processare.

## Passo 2: court

Lanciando /cantiere l'utente ha già chiesto il processo: parti senza chiedere conferma. Prima scrivi una riga: "Convoco court con 12 giurati: circa 17 chiamate a sub-agent, scrive in `.court/` nella cartella del progetto; conviene la modalità accept-edits (Shift+Tab)."

Poi chiama la skill `court` dalla cartella del progetto, con argomento `--jury 12` seguito dal caso. Il file del caso: in italiano, solo fatti detti dall'utente, con destinatari e cosa vuol dire riuscire, così anche i giurati con una lente da business hanno presa su tesi e articoli.

Riporta il verdetto, le obiezioni principali e le condizioni che lo ribalterebbero. Prosegui qualunque sia il verdetto.

Il passo è finito quando il verdetto è riportato.

## Passo 3: grilling mirato

1. **Obiezioni (scritto qui).** Per ogni obiezione e condizione della giuria, una domanda con tre opzioni: correggere l'idea / accettare il rischio / scartare quella parte. Una per volta; annota scelta e motivo.
2. **Decisioni di design.** Chiama la skill `grilling` sulle decisioni di design ancora aperte. Passale come già decisi il caso del passo 1 e le scelte sulle obiezioni, così non li richiede. grilling lavora a giri di domande numerate con risposta consigliata: segui il suo formato.

Il passo è finito quando la frontiera di grilling è vuota e l'utente conferma che non restano decisioni di design aperte.

## Passo 4: documento di design

Scrivi tu il documento in `docs/cantiere/AAAA-MM-GG-<slug>-design.md` nella cartella del progetto (crea la cartella se manca; `<slug>` è il nome del prodotto in minuscolo con trattini). Leggi `modelli/documento-di-design.md` nella cartella di questa skill (la "Base directory" stampata in cima) e riempi ogni sezione.

Il documento deve bastare da solo, perché la chat di /produci non vede questa conversazione: scrivi per esteso decisioni, motivi, obiezioni e rischi accettati.

Il passo è finito quando il file è scritto e hai stampato il percorso.

## Passo 5: chat di avanzamento

Leggi `modelli/sal.md` nella cartella di questa skill, sostituisci i segnaposto (nome del prodotto, percorso del documento) e salva il risultato in `docs/cantiere/sal.md`.

Poi, se hai lo strumento `mcp__ccd_session__start_session` (app desktop; se è tra gli strumenti differiti, caricalo con ToolSearch), avvia una sessione con `initiation: user_asked`, `context: fresh`, `use_worktree: false`, titolo "Avanzamento: <nome>", `prompt` "Leggi docs/cantiere/sal.md e seguilo." e come `background` due frasi: cos'è il prodotto e dove sta il documento di design. Se lo strumento non c'è, stampa: "Apri una chat nuova in questa cartella e scrivi: leggi docs/cantiere/sal.md e seguilo".

Il passo è finito quando la sessione è avviata o la riga è stampata.

## Passo 6: chiusura

Stampa:
- il percorso del documento di design;
- il comando da incollare in una chat **nuova** nella **stessa** cartella: `/produci docs/cantiere/<file>-design.md`;
- promemoria: i giurati sono tutti Claude, prova l'idea con 2-3 persone reali del pubblico;
- per tesi e lavori accademici: verifica le regole del corso sull'uso dell'AI.

Poi fermati: la produzione parte con /produci.

## Regole generali

- Distingui ciò che è certo (detto dall'utente o verificato) da ciò che è dedotto, e scrivilo come tale. Dati, fonti e funzionalità vengono dall'utente o da una fonte verificata: mai inventati.
- Se manca una skill richiesta, fermati e dillo. Eccezione: graphify, facoltativa.
- Scrivi asciutto: fatti, domande e decisioni, senza complimenti né riempitivi.
- Il documento di design fa fede; le liste dell'app e di claude-mem sono solo viste.
