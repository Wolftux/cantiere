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
