# Act 1 — Completed
> Data: 2026-04-27 | Sessione: VS Code + Claude Code

---

## Stato finale: ✅ Completato

Tutti i deliverable richiesti dall'RFP §4 sono stati prodotti in `proposal/`.

---

## Deliverable prodotti

| File | Descrizione | Stato |
|---|---|---|
| `proposal/executive-summary.md` | 1 pagina, problema + approccio + differenziatori | ✅ |
| `proposal/technical-approach.md` | R1–R4 con dettaglio tecnico + assunzioni A1–A5 | ✅ |
| `proposal/timeline.md` | 3 fasi, 6 settimane, milestone + rischi | ✅ |
| `proposal/pricing.md` | T&M not-to-exceed, $90K required / $122K con optional | ✅ |
| `proposal/capabilities-deck.html` | 10 slide HTML autocontenuto, navigabile con ←→ | ✅ |

---

## Semilavorati usati (questa cartella)

| File | Alimenta |
|---|---|
| `rfp-analysis.md` | executive-summary, technical-approach |
| `research-notes.md` | executive-summary, technical-approach, pricing |
| `clarifying-questions.md` | technical-approach (sezione Assumptions A1–A5) |

---

## Sintesi del lavoro per step

**Step 1 — Read & Analyze RFP**
- 4 requisiti obbligatori (R1–R4), 3 desiderati (D1–D3)
- 5 ambiguità identificate: budget, critical flows R3, UI standards D1, lingue i18n D2, deployment
- Insight chiave: R3 testing è il gatekeeper IT — va consegnato in Phase 1

**Step 2 — Research the Client**
- Buying committee: Okafor (procurement), Tanaka (VP Ops, champion), IT anonimo (gatekeeper)
- Vendor precedente: handoff di 57 righe, zero test, migrazione Composition API incompleta
- Differenziatore: abbiamo letto il codice prima di scrivere la proposta

**Step 3 — Clarifying Questions**
- 5 domande a procurement (deadline 28 apr): budget, critical flows, UI standards, lingue i18n, deployment
- Per ciascuna: assunzione di fallback usata nel technical approach

**Step 4 — Write the Proposal**
- Executive summary: tono operativo (parla a Tanaka, non solo a Okafor)
- Technical approach: R1 audit-first + 20% buffer, R2 logica restocking, R3 in Phase 1, R4 all'onboarding
- Timeline: 3 fasi con gate e sign-off scritto a ogni milestone
- Pricing: T&M NTE, $90K required, $122K con D1–D3, pagamento in 3 tranche

**Step 5 — Capabilities Deck**
- 10 slide HTML, stile dark/light, serif, accent arancio
- Navigabile con tasti freccia e anchor `#N`
- Aperto nel browser, pronto per iterazione

---

## Lezione implicita di Act 1

Ogni step ha dimostrato una tecnica concreta di collaborazione con Claude Code:

| Step | Tecnica dimostrata |
|---|---|
| 1 | Claude come research partner su documenti |
| 2 | Sintesi multi-documento → insight strategici |
| 3 | Claude che genera domande + assunzioni di fallback |
| 4 | Collaboration loop: Brief → Draft → Review → Refine, una sezione alla volta |
| 5 | Claude che genera artefatti visivi (HTML deck) da semilavorati testuali |

---

## Prossimo passo: Act 2

*"We submitted. Two weeks later Meridian picked us. Everything up to now has been documents — from here it's hands on keyboard in the actual codebase."*

Aprire Claude Code nel terminale:
```bash
cd meridian-workshop
claude
```
Poi digitare `/start` per avviare backend e frontend, e iniziare da R4 (architecture) o R1 (reports audit).
