# Cantiere

Due skill per Claude Code, in italiano, che portano un'idea grezza fino a un prodotto finito:

- `/cantiere <idea>`: la fase di design. Riscrive l'idea senza aggiunte, fa poche domande, la mette sotto processo con Court, stringe le decisioni rimaste aperte con Grilling e scrive un documento di design nel progetto. Poi si ferma.
- `/produci <documento>`: la fase di produzione, in una chat nuova nella stessa cartella. Legge il documento di design e realizza il prodotto: software, testo o slide.

## Installazione

Serve Claude Code. Nel terminale:

```bash
claude plugin marketplace add Wolftux/cantiere
claude plugin install cantiere@cantiere
```

Poi riavvia Claude Code (oppure lancia `/reload-plugins`) e scrivi `/cantiere` seguito dalla tua idea.

## Skill di appoggio

Cantiere non copia le skill di altri autori: le richiama. Al primo avvio ogni comando controlla quali mancano e installa solo quelle, da questo stesso marketplace. Se le hai già, da qualunque marketplace, non installa niente.

| Skill | Autore | Licenza | Usata da |
|---|---|---|---|
| [Court](https://github.com/alexyc9381/court-skill) | Alex Chen | MIT | /cantiere |
| [Grilling](https://github.com/mattpocock/skills) (nel pacchetto mattpocock-skills) | Matt Pocock | MIT | /cantiere |
| [Superpowers](https://github.com/obra/superpowers) | Jesse Vincent | MIT | /produci |
| [Humanizer](https://github.com/blader/humanizer) | blader | MIT | /produci |
| [UI/UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | Next Level Builder | MIT | /produci |

Puoi installarle anche a mano, per esempio `claude plugin install court-skill@cantiere`.

## 21st.dev (facoltativo)

Per i progetti software con interfaccia, /produci può usare i componenti di [21st.dev](https://21st.dev) tramite il suo server MCP. Serve un account e una chiave API: /produci ti guida e ti fa lanciare nel terminale lo script `plugins/cantiere/produci/scripts/installa-21st.sh`, che registra il server con `claude mcp add`. La chiave non va mai scritta in chat.

## Cosa lascia nel progetto

- Il documento di design in `docs/cantiere/AAAA-MM-GG-<nome>-design.md`.
- Per testi e slide, la lista dei lavori in `docs/cantiere/avanzamento.md`.
- I file del processo di Court in `.court/`.

## Limiti

- Court e il ramo software di /produci girano solo in Claude Code (sub-agent e python3).
- I giurati di Court sono tutti Claude: trovano obiezioni, non validano l'idea.
- Un prodotto per cartella di progetto: due /cantiere nella stessa cartella si sovrascrivono.

## Licenza

MIT, Mattia Balboni. Le skill di appoggio restano dei rispettivi autori, con le loro licenze.

---

# English

**From a rough idea to a finished product, in two deliberate halves.**

Cantiere (Italian for *construction site*) is a pair of Claude Code skills that take an idea you can barely articulate and carry it, step by step, to something you can ship: a piece of software, a long-form text, or a slide deck. It separates *deciding what to build* from *building it*, and it refuses to let the second begin before the first is finished. The skills speak Italian.

## `/cantiere`: the design phase

Hand it a raw idea. It rewrites the idea faithfully, using only what you said, and lists the gaps. A short interrogation follows: no more than eight questions, on the problem, the audience, the solution and the constraints. Then the idea goes on trial.

**Court** convenes a jury of twelve independent Claude jurors, each with a different background, plus a prosecutor and a defence. They argue the case, vote, and return a verdict together with the objections that weighed most and the conditions that would overturn it. The verdict never stops the work: it sharpens it. Every objection is put back to you as a decision: fix it, accept the risk, or cut that part.

What remains open is then settled by **Grilling**, a relentless round of pointed questions on the design decisions still unresolved, until nothing is left to decide.

The result is a design document written into your project, complete enough to stand on its own: decisions, reasons, objections, accepted risks and the structure of the product. Then `/cantiere` stops.

## `/produci`: the production phase

In a fresh chat, in the same folder, `/produci` reads the design document and builds what it describes. Decisions are taken as given; only what the document leaves open is asked.

- **Software** goes through brainstorming, a written spec, a plan and a test-driven execution, with a design system when there is an interface.
- **Text and slides** are drafted section by section against a progress list, with every claim backed by a source found at writing time (never invented), and a final editorial pass that strips the usual machine-written tells while keeping data, figures and references untouched.

Work can be interrupted and resumed: the progress list and the design document always agree on where things stand.

## Built on the shoulders of others

Cantiere does not copy other authors' skills; it calls them, and installs only the ones you are missing, from its own marketplace: Court by Alex Chen, Grilling by Matt Pocock, Superpowers by Jesse Vincent, Humanizer by blader, UI/UX Pro Max by Next Level Builder. All MIT, all credited.

## Honest about its limits

The jurors are all Claude: they find objections, they do not validate your idea. Test it with two or three real people from your audience. For academic work, check your course's rules on the use of AI. One product per project folder.

## Install

```bash
claude plugin marketplace add Wolftux/cantiere
claude plugin install cantiere@cantiere
```

Then restart Claude Code (or run `/reload-plugins`) and type `/cantiere` followed by your idea.

Cantiere is also listed on [ClaudeMarketplace](https://www.claudemarketplace.net/), a directory of Claude Code skills and MCP servers.
