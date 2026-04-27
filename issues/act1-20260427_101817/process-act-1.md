# Act 1 — Respond to the RFP
> Scenario: Meridian Components ha emesso l'RFP #MC-2026-0417. Sei il consulente che risponde.
> Tutto l'output va in `proposal/`. Nessun codice in questo atto.

---

## 1. Read & Analyze the RFP

**Obiettivo:** Capire cosa chiede il cliente prima di scrivere una sola parola.

**Attività:**
- Leggi `docs/rfp/MC-2026-0417.md` integralmente
- Separa i requisiti **obbligatori** (R1–R4) da quelli **desiderati** (D1–D3)
- Identifica le **ambiguità deliberate** nel testo:
  - "current standards" per la UI (D1) — non definito
  - Nessun budget range indicato
  - "critical flows" per i test (R3) — non specificato
  - "at least eight issues" sui Reports (R1) — numero vago
- Annota le domande che manderesti a procurement (§6 dell'RFP, deadline 28 aprile)

**Output:** Lista mentale / note di lavoro — non un file.

**Prompt utile:**
```
Leggi @docs/rfp/MC-2026-0417.md e dimmi: quali sono i requisiti obbligatori vs desiderati,
cosa è ambiguo, e cosa chiederei al cliente prima di scrivere la proposta.
```

---

## 2. Research the Client

**Obiettivo:** Capire chi è Meridian per personalizzare la proposta.

**Attività:**
- Leggi `docs/rfp/meridian-background.md` — profilo azienda, contesto operativo
- Leggi `docs/rfp/vendor-handoff.md` — note del vendor precedente (thin = finding di per sé)
- Identifica i pain point operativi: 3 warehouse (SF, London, Tokyo), staff multilingua, ambienti low-light
- Valuta la qualità della documentazione lasciata — se è scarsa, è un rischio da menzionare in proposta

**Output:** Nessun file, ma le informazioni alimentano tutte le sezioni successive.

**Prompt utile:**
```
Leggi @docs/rfp/meridian-background.md e @docs/rfp/vendor-handoff.md.
Cosa devo sapere su Meridian che potrebbe differenziare la nostra proposta?
```

---

## 3. Draft Clarifying Questions

**Obiettivo:** Simulare le domande reali che si manderebbero a procurement.

**Attività:**
- Scrivi 3–5 domande di chiarimento basate sulle ambiguità trovate al punto 1
- Per ciascuna, decidi un'**assunzione di fallback** da usare in proposta se non rispondi
- Esempi tipici:
  - *"Quali sono i 'critical flows' che il vostro IT team vuole coperti dai test?"*
  - *"Esiste un budget indicativo o una fascia di riferimento?"*
  - *"Avete un design system o brand guidelines per il refresh UI (D1)?"*
  - *"Il personale di Tokyo opera in giapponese? Quali altre lingue servono per D2?"*

**Output:** Le risposte diventano **assunzioni esplicite** nella proposta (sezione Technical Approach).

**Prompt utile:**
```
Basandoti sull'RFP, scrivi 3-5 domande che manderesti a procurement@meridiancomponents.example.
Per ciascuna includi l'assunzione che useremmo in proposta se non rispondono.
```

---

## 4. Write the Proposal

Ogni sezione va in un file separato in `proposal/`. Scrivi una sezione alla volta, fermati, rivedi, poi vai avanti.

### 4a. Executive Summary
**File:** `proposal/executive-summary.md`

**Contenuto:**
- 1 pagina max (come richiesto dall'RFP §4)
- Dimostra che hai capito il problema (non solo letto il testo)
- Proposta di valore in 2-3 frasi: perché noi, perché ora
- Richiamo ai 4 requisiti obbligatori senza entrare nel tecnico

**Prompt utile:**
```
[stage] Sei un senior consultant Accenture che risponde a un RFP per modernizzare un inventory dashboard.
[task] Scrivi l'executive summary per proposal/executive-summary.md.
[rules] Max 1 pagina. Tono professionale ma diretto. Niente buzzword. Dimostra comprensione del problema,
non solo lista dei deliverable.
```

### 4b. Technical Approach
**File:** `proposal/technical-approach.md`

**Contenuto:**
- Per ciascuno di R1–R4 (e opzionalmente D1–D3): come lo affronteresti, con quali assunzioni
- R1 Reports: audit + remediation plan (menziona che hai già visto il codice)
- R2 Restocking: architettura della nuova vista (input: stock + demand + budget → output: PO raccomandati)
- R3 Testing: framework scelto (Playwright), coverage plan, criteri di done
- R4 Architecture: formato HTML interattivo, consegnato con l'engagement
- Includi le assunzioni derivate dalle clarifying questions

> Usa **Plan Mode** (`Shift+Tab`) prima di scrivere questa sezione — è il momento giusto per allinearsi sull'outline prima di produrre il testo.

### 4c. Timeline
**File:** `proposal/timeline.md`

**Contenuto:**
- Piano a fasi (suggerito: 3 sprint da 2 settimane)
- Fase 1: Onboarding + R4 Architecture + inizio R1 Reports
- Fase 2: Completamento R1 + R2 Restocking
- Fase 3: R3 Testing + R4 finale + eventuali D1–D3
- Milestone e criteri di accettazione per ciascuna fase
- Note sui rischi: documentazione del vendor precedente lacunosa

### 4d. Pricing
**File:** `proposal/pricing.md`

**Contenuto:**
- Modello: fixed-fee o T&M con not-to-exceed (come richiesto dall'RFP §4)
- Stima per requisito: R1 (X giorni), R2 (Y giorni), R3 (Z giorni), R4 (W giorni)
- Assunzioni su cui si basa il prezzo (es. team size, accesso al codice, nessuna migrazione DB)
- Opzione per i desiderata D1–D3 come change request

---

## 5. Build the Capabilities Deck

**File:** `proposal/capabilities-deck.html`

**Obiettivo:** Presentazione visiva da 10–15 slide, autocontenuta in un singolo HTML.

**Struttura suggerita:**
1. Cover — nome firma + titolo engagement
2. Il problema di Meridian (in 3 bullet)
3. Il nostro approccio (R1→R4 in una slide)
4. R1 — Reports remediation: cosa faremo
5. R2 — Restocking view: come funzionerà
6. R3 — Testing: coverage plan
7. R4 — Architecture docs
8. Timeline a fasi (visual)
9. Perché noi (relevant experience)
10. Next steps

**Note tecniche:**
- Navigabile con frecce / anchor `#1`, `#2`… come il deck Anthropic di oggi
- Stile: sfondo scuro o chiaro, serif grande, accent color, layout pulito
- Apri nel browser appena generato, poi chiedi cosa cambiare e itera almeno una volta

**Prompt utile:**
```
Genera proposal/capabilities-deck.html: un deck HTML autocontenuto da 10 slide per rispondere
all'RFP di Meridian Components. Navigabile via frecce. Stile professionale, sfondo scuro,
font serif, accent arancio. Contenuto basato su executive-summary.md e technical-approach.md.
```

**Opzione PPTX** (se serve un file da condividere):
```
Converti il deck in proposal/capabilities-deck.pptx usando python-pptx.
```

---

## Checklist di completamento Act 1

- [ ] RFP letto e requisiti separati (R vs D)
- [ ] Ambiguità identificate
- [ ] Background client ricercato (`meridian-background.md` + `vendor-handoff.md`)
- [ ] Clarifying questions scritte + assunzioni definite
- [ ] `proposal/executive-summary.md` scritto e rivisto
- [ ] `proposal/technical-approach.md` scritto e rivisto
- [ ] `proposal/timeline.md` scritto e rivisto
- [ ] `proposal/pricing.md` scritto e rivisto
- [ ] `proposal/capabilities-deck.html` generato, aperto nel browser, iterato almeno una volta
- [ ] Transizione ad Act 2 — *"We won the bid. Now it's hands on keyboard."*

---

## Lezione implicita di Act 1

Ogni step di questo atto dimostra una tecnica di collaborazione con Claude:
- **Leggere + analizzare** → Claude come research partner
- **Clarifying questions** → Claude che usa `AskUserQuestion` tool per simulare il cliente
- **Scrivere sezione per sezione** → collaboration loop (Brief → Draft → Review → Refine)
- **Plan Mode prima del Technical Approach** → allineamento prima di produrre
- **Capabilities deck** → Claude che genera artefatti visivi e itera sul feedback
