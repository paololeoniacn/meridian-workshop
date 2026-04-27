# Research Notes — Meridian Components
> Semilavorato Act 1 / Step 2. Fonti: meridian-background.md + vendor-handoff.md + codice.
> Alimenta: executive-summary, technical-approach, clarifying-questions.

---

## Profilo cliente

| Campo | Dettaglio |
|---|---|
| Azienda | Meridian Components, Inc. — privata, fondata 2009, HQ San Francisco |
| Settore | Distribuzione parti per automazione industriale (sensori, attuatori, controller, circuit board, power supply) |
| Revenue | ~$9.6M (FY25) |
| Dipendenti | ~180 |
| Warehouse | San Francisco (HQ + distribuzione primaria), London (EMEA), Tokyo (aperto 2023, APAC OEM) |
| Team Tokyo | ~12 persone, inglese variabile → driver reale dell'i18n ask |

---

## Buying committee — chi decide e cosa vuole

| Persona | Ruolo | Priorità | Come parlargli |
|---|---|---|---|
| **J. Okafor** | Director of Procurement | Timeline e prevedibilità del prezzo | Criteri RFP, milestone chiare, no sorprese |
| **R. Tanaka** | VP Operations | Vuole il Restocking (R2). Frustrata col vendor precedente | Parlare ai pain operativi quotidiani, non al tech stack |
| **IT (anonimo)** | Gatekeeper tecnico | Ha bloccato modifiche perché non ci sono test | R3 testing è il prerequisito — va consegnato per primo |

**Insight strategico:** Tanaka è il champion interno. Se la proposta parla solo alla lista tecnica di Okafor, perdiamo l'alleato che può spingere internamente. L'executive summary deve rispecchiare i pain di chi usa il dashboard ogni giorno.

---

## Analisi vendor precedente (vendor-handoff.md)

### Cosa hanno consegnato
- Stack funzionante: Vue 3 + FastAPI + JSON mock data
- Documentazione: file map + lista API endpoint — **nient'altro**
- Nessun test, nessun diagramma, nessuna decision log

### Cosa NON hanno consegnato (finding espliciti nel loro stesso handoff)
- Reports: filtri non cablati
- Test: zero copertura
- Migrazione Composition API: incompleta — alcune view ancora in Options API

### Finding impliciti (leggendo il codice)
- `/api/reports/quarterly` e `/api/reports/monthly-trends` non supportano i filtri warehouse/category/status che tutti gli altri endpoint hanno → inconsistenza API
- `vendor-handoff.md` è 57 righe per un sistema con 6+ viste e 15+ endpoint → documentazione inadeguata per un handoff professionale

### Come usare questo nella proposta
- **Non attaccare il vendor precedente** — dire che "la documentazione ricevuta è limitata, come da prassi in questi handoff, e il nostro onboarding plan prevede un audit completo prima di stimare il remediation"
- Il contrasto implicito ("noi facciamo diversamente") emerge da solo dal nostro approccio strutturato

---

## Analisi tecnica del codebase (da code inspection)

### Architettura attuale

```
Browser (Vue 3 / Vite / port 3000)
    ↓ axios → client/src/api.js
FastAPI (Python / port 8001)
    ↓ import
mock_data.py → server/data/*.json  (no database)
```

### Viste esistenti
- Dashboard (summary + KPI)
- Inventory (filtri: warehouse, category)
- Orders (filtri: warehouse, category, status, month)
- Reports (problemi noti)
- Spending (summary, monthly, categories, transactions)
- Backlog + Purchase Orders

### Rischi tecnici identificati

| Rischio | Probabilità | Impatto | Mitigazione |
|---|---|---|---|
| Options API mista a Composition API | Alta | Regressioni durante R1 fix | Audit completo prima di toccare Reports |
| No database → mock data | Bassa (per ora) | R2 Restocking solo in memoria | Accettabile per scope attuale; notare in proposta |
| Zero test | Certezza | Qualsiasi fix può rompere altro | R3 in fase 1 prima di R1 |
| Handoff doc lacunosa | Certezza | Sottostima delle ore | Buffer del 20% sulla stima R1 |

---

## Opportunità di differenziazione

1. **Abbiamo già letto il codice** — nessun altro offerente lo ha fatto (o lo dichiara). Citarlo nell'executive summary.
2. **R3 come sblocco** — IT è un gatekeeper. Chi capisce questo vince la fiducia del buying committee.
3. **Proposta parlante per Tanaka** — se la nostra exec summary parla di "operations team che non può fidarsi dei dati di Reports" invece di "remediation of module defects", siamo gli unici a parlare la sua lingua.
4. **Onestà sui rischi** — Meridian è stata scottata da un vendor che ha sotto-consegnato. Trasparenza su rischi e assunzioni vale più del prezzo più basso.
