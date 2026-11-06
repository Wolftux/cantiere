---
name: produci
description: "Fase di produzione: legge il documento di design scritto da /cantiere e realizza il prodotto. Comando: /produci [percorso del documento]"
argument-hint: "<percorso del documento di design>"
disable-model-invocation: true
---

# /produci: dal documento di design al prodotto

Percorso del documento di design: $ARGUMENTS

/produci è la seconda metà di /cantiere: realizza il prodotto descritto nel documento di design, in una chat diversa da quella del design. Le decisioni sono già nel documento: prendile come date e chiedi solo ciò che il documento lascia aperto.

Come lavori:
- Esegui i passi in ordine.
- Le domande scritte da questa skill usano il selettore a scelta multipla (AskUserQuestion): una per volta, opzione consigliata per prima. Le skill che chiami usano il loro formato.
- Quando un turno finisce e resta lavoro, chiudilo con `wrap_up` e un pulsante "Continua", se lo strumento c'è.
- Prima del passo 0 controlla nella tua lista di skill di avere quelle che servono, da qualunque marketplace vengano: `superpowers:brainstorming` (plugin superpowers), `humanizer:humanizer` (plugin humanizer) e `ui-ux-pro-max:ui-ux-pro-max` (plugin ui-ux-pro-max). Per ogni plugin che manca, scrivi una riga all'utente e installa solo quello dal marketplace cantiere con Bash: `claude plugin install <plugin>@cantiere`. Poi chiedi all'utente di lanciare `/reload-plugins` (o di aprire una sessione nuova) e di rilanciare /produci, e fermati. Se non sei in Claude Code o l'installazione fallisce, di' cosa manca e fermati.

## Passo 0: avvio e ripresa

1. Se il percorso è vuoto, cerca il più recente `docs/cantiere/*-design.md` e chiedi conferma con un clic. Se non ce n'è nessuno, fermati e di' di lanciare prima /cantiere.
2. Leggi tutto il documento. Se manca una delle sezioni "Tipo, registro, interfaccia", "Struttura del prodotto" o "Decisioni prese", fermati e di' quale.
3. Ripresa:
   - Software: riparti dalla prima fase non completata tra design system (`design-system/<slug>/MASTER.md`, solo con interfaccia), spec in `docs/superpowers/specs/`, piano in `docs/superpowers/plans/` ed esecuzione; spec e piano validi sono quelli che citano questo documento. Con piano e ledger (`.superpowers/sdd/<piano>/progress.md`) l'esecuzione di superpowers riparte da sola dal primo task senza `Task N: complete`. Il ledger può stare in un worktree git del progetto (`git worktree list`): se è lì, riprendi lavorando in quel worktree.
   - Testo e Slide: se `docs/cantiere/avanzamento.md` esiste con voci aperte, mostra lo stato in due righe e, dopo un clic di conferma, riprendi dalla prima voce non spuntata.

Il passo è finito quando conosci tipo, registro, interfaccia e il punto da cui partire.

## Passo 1: lista dei lavori

- Testo e Slide: se `docs/cantiere/avanzamento.md` non esiste, crealo dalla "Struttura del prodotto" in questo formato:

  ```markdown
  # Avanzamento: <nome prodotto>
  Documento: docs/cantiere/<file>-design.md

  - [x] Sezione 1: Introduzione (fatta il AAAA-MM-GG)
  - [ ] Sezione 2: ...
  - [ ] Revisione humanizer
  - [ ] Chiusura
  ```

  Spunta ogni voce, con la data, subito dopo averla finita e verificata, prima di passare alla successiva.
- Software: la lista dei lavori è il piano di writing-plans con il suo ledger; tieni solo quelli.

Questi file fanno fede e la chat di avanzamento li legge; le liste dell'app e di claude-mem sono solo viste.

## Modifiche in corso d'opera (in qualunque momento)

Quando l'utente cambia qualcosa durante la produzione:
1. aggiungi una riga alla tabella "Modifiche in corso d'opera" del documento di design: data, cosa è cambiato, perché;
2. aggiorna subito la lista dei lavori: voci di `avanzamento.md`, oppure task del piano (e lo spec, se il cambio tocca il design).

Documento, lista e chat di avanzamento dicono sempre la stessa cosa.

## Passo 2: produzione

Leggi `rami/<tipo>.md` nella cartella di questa skill (la "Base directory" stampata in cima): `rami/software.md`, `rami/testo.md` o `rami/slide.md`. Seguilo fino al suo criterio di fine.

### Fonti (Testo e Slide)

- Registro accademico/scientifico: per ogni affermazione da sostenere cerca PRIMA di scriverla con i connettori di letteratura disponibili (PubMed, Europe PMC, Clinical Trials, ICD-10 e simili) e cita con PMID o DOI presi dal risultato. Ciò che non trovi lo scrivi marcato "da verificare".
- Registro divulgativo: stesse regole; il riferimento può essere informale (link, oppure autore e anno).
- Senza connettori di letteratura (per esempio nella chat di claude.ai): dillo in una riga e marca "da verificare" ogni affermazione da sostenere.
- Ogni citazione viene da un risultato di ricerca: mai fonti inventate.

## Passo 3: revisione con humanizer (Testo e Slide)

Chiama la skill `humanizer` (plugin `humanizer:humanizer`) in modalità embedded: restituisce solo il testo finale. È l'unica riscrittura del testo.

- Dati, numeri, dosaggi, citazioni, riferimenti, termini tecnici e sigle restano identici.
- Dopo la riscrittura confronta con la bozza: nessuna affermazione aggiunta o persa.
- Il testo è in italiano: applica i pattern strutturali (non X ma Y, chiusure a effetto, terne forzate, trattini, enfasi gonfiata, riempitivi) e salta i controlli pensati per l'inglese: §10 coppie con trattino, §12 parole AI inglesi, §21 virgolette curve.
- Se l'utente ha dato un testo di stile, la voce segue quello.
- Registro accademico/scientifico: tono neutro e impersonale; il passivo e le cautele sui dati restano (salta §11); nessuna opinione aggiunta.
- Registro divulgativo: voce dell'autore, esempi concreti, rivolgersi al lettore.

Il passo è finito quando il testo rivisto è salvato nel documento o nel deck e "Revisione humanizer" è spuntata.

## Passo 4: chiusura

- Consegna il prodotto con una riga di contesto; per Testo e Slide spunta "Chiusura".
- Di' dove sta tutto: prodotto, documento di design, lista dei lavori, e design system, spec e piano se ci sono.
- Promemoria: prova il prodotto con 2-3 persone reali del pubblico; per lavori accademici verifica le regole del corso sull'uso dell'AI.

## Regole generali

- Distingui ciò che è certo (detto dall'utente, scritto nel documento o verificato) da ciò che è dedotto, e scrivilo come tale. Dati, fonti e funzionalità vengono dall'utente o da una fonte verificata: mai inventati.
- Se manca una skill richiesta, fermati e dillo. Facoltativi (una riga e avanti): graphify, 21st, connettori di letteratura.
- Scrivi asciutto: fatti, domande e decisioni, senza complimenti né riempitivi.
