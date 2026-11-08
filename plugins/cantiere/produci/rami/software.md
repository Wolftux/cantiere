# Ramo Software

Parte di /produci. Il documento di design è la fonte di scopo, vincoli e decisioni.

Prima del punto 1, solo se interfaccia = sì e gli strumenti `21st` mancano (cerca `21st` anche tra gli strumenti differiti, con ToolSearch): proponi di aggiungerlo, con la guida qui sotto. Se l'utente ha già detto no in questo progetto (è scritto nella lista dei lavori), salta.

**Guida 21st.** Un passo per messaggio, aspettando la risposta.
1. **Proposta.** In due righe: 21st.dev è una libreria di componenti di interfaccia già pronti; con il suo MCP li cerco e li adatto al progetto invece di disegnarli da zero. Serve un account 21st.dev. Chiedi: Sì, guidami / No, prosegui senza. Con No, scrivi "21st: no" nella lista dei lavori e vai al punto 1.
2. **Account.** Apri https://21st.dev (su macOS con `open`, altrimenti dai il link) e di' all'utente di accedere o di registrarsi. Chiedi: Fatto / Ho un problema. Con un problema, aiutalo; non inserire mai tu dati personali o password.
3. **Chiave e configurazione.** La chiave API la incolla l'utente nel wizard, mai in chat: se l'utente la scrive in chat, non usarla e digli di crearne una nuova. Il wizard è `scripts/installa-21st.sh` nella cartella di questa skill (percorso assoluto dalla cartella base della skill). Ha bisogno di un terminale vero: se hai uno strumento che apre un terminale dell'utente, lancialo lì con `bash "<percorso assoluto>"`; altrimenti dai all'utente quel comando in un blocco `bash` da incollare nel Terminale. Il wizard apre la pagina della chiave, la chiede in modo nascosto, registra il server con `claude mcp add` e verifica la connessione.
4. **Ripresa.** Quando il wizard dice che 21st risponde, aggiorna la lista dei lavori e chiedi all'utente di aprire una sessione nuova e rilanciare /produci: riparte da qui. Se il wizard fallisce, leggi l'errore con l'utente; se non si risolve, prosegui senza 21st.

1. **Design system** (solo se interfaccia = sì). Usa la skill `ui-ux-pro-max` (plugin `ui-ux-pro-max:ui-ux-pro-max`).
   - Se esiste `design-system/<slug>/MASTER.md`, leggilo e tienilo com'è.
   - Altrimenti esegui il suo `search.py` (il percorso è nelle istruzioni della skill) con `"<tipo di prodotto> <settore> <parole di stile>" --design-system --persist -p "<nome del prodotto>" --output-dir "<radice del progetto>"`, prendendo tipo, pubblico e stile dal documento.
   - Mai `--force`.
2. **Brainstorming.** Chiama `superpowers:brainstorming` e passale il percorso del documento di design (e di MASTER.md, se c'è) come fonte di scopo, vincoli e decisioni già prese: brainstorming riflette ciò che è già fornito invece di richiederlo. Segui il suo percorso completo, quello con lo spec scritto: approcci, design a sezioni, spec in `docs/superpowers/specs/` che cita in testa il documento di design, revisione dell'utente.
3. **Piano.** `superpowers:writing-plans`: piano in `docs/superpowers/plans/`. In "Global Constraints" cita il documento di design e, con interfaccia, MASTER.md e le regole per i task di interfaccia del punto 4: così le leggono anche i sub-agent.
4. **Esecuzione.** `superpowers:subagent-driven-development` (consigliata) o `superpowers:executing-plans`: test prima del codice, verifica prima di dichiarare finito. Regole per i task di interfaccia:
   - leggi MASTER.md e `design-system/<slug>/pages/<pagina>.md`, se esiste;
   - usa le ricerche `--stack` e `--domain` di ui-ux-pro-max;
   - se hai gli strumenti di 21st (`mcp__21st__*`, o con un altro prefisso che contiene `21st`; anche tra i differiti: caricali con ToolSearch), usa `search`, `get_component` e `get_inspiration` per i componenti, e `generate` solo se compare tra gli strumenti. Se 21st manca, dillo in una riga e prosegui.
5. **Grafo.** A fine lavoro, se la skill `graphify` è disponibile: se nella cartella del progetto esiste già `graphify-out/`, esegui `/graphify <cartella> --update`; altrimenti esegui `/graphify <cartella>` per crearlo. Il grafo nasce al primo /produci e cresce a ogni giro: i /produci e i /cantiere successivi sullo stesso progetto lo trovano pronto e lo usano come contesto invece di rileggere i file. Tra un giro e l'altro il codice si può tenere allineato senza modello con `graphify update <cartella>` (da un hook di fine sessione o post-commit); documenti e PDF nuovi li copre solo il `--update` di questo punto. Se graphify manca, dillo in una riga e prosegui.

Il ramo è finito quando ogni task del piano ha `Task N: complete` nel ledger e la revisione finale di superpowers è chiusa. Poi vai al passo 4 di SKILL.md: il passo 3 (humanizer) vale solo per Testo e Slide.
