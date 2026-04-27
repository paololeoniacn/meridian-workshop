# Act 2 — Deliver the Engagement
> Scenario: Meridian ha scelto noi. Ora mettiamo le mani nel codice.
> Stack: Vue 3 (port 3000) + FastAPI (port 8001) + JSON mock data

---

## 0. Get it running

**Obiettivo:** Avviare backend e frontend e verificare che l'app giri prima di toccare qualsiasi cosa.

**Attività:**
- Nel terminale Claude Code, digita `/start` (slash command del progetto)
- In alternativa: `./scripts/start.sh`
- Apri `http://localhost:3000` nel browser
- Clicca tutte le pagine — nota cosa sembra rotto (Reports è il candidato principale)

**Output:** App funzionante in locale. Lista visiva dei problemi evidenti.

**Prompt utile:**
```
/start
```

---

## 1. R4 — Architecture Documentation

**Obiettivo:** Capire il codebase prima di modificarlo. Produrre doc utile a Meridian IT.

**Attività:**
- Esplora la struttura: `client/src/views/`, `client/src/api.js`, `server/main.py`, `server/mock_data.py`
- Mappa i flussi: Vue component → api.js → FastAPI endpoint → JSON data
- Identifica il tech debt: Options API residua, endpoint senza filtri, zero test
- Genera `proposal/architecture.html` — diagramma interattivo, catalogue endpoint, component map

**Output:** `proposal/architecture.html`

**Prompt utile:**
```
Esplora il codebase e genera proposal/architecture.html con:
- diagramma architettura (Vue → api.js → FastAPI → JSON)
- tabella endpoint con filtri supportati
- component map per ogni vista
- lista tech debt trovato
Aprilo nel browser quando fatto.
```

---

## 2. R3 — Automated Browser Testing

**Obiettivo:** Stabilire la baseline di test PRIMA di correggere qualsiasi bug.
Consegnato in Phase 1 — sblocca IT per approvare tutti i cambiamenti successivi.

**Prerequisito:** MCP Playwright connesso. Verifica con `/mcp` nel prompt Claude Code.
Se non connesso: riavvia Claude Code nella cartella e approva i server MCP alla richiesta.

**Attività:**
- Digita `/mcp` per confermare che `playwright` è connesso
- Scrivi test per i 5 critical flows (usando `mcp__playwright__*` tools):

| Flow | File test | Cosa coprire |
|---|---|---|
| Inventory | `tests/e2e/inventory.spec.js` | Caricamento, filtro warehouse, filtro category, combinato |
| Orders | `tests/e2e/orders.spec.js` | Filtro status, filtro mese, combinato |
| Reports | `tests/e2e/reports.spec.js` | Quarterly load, monthly trends, filtri (anche se rotti — baseline) |
| Dashboard | `tests/e2e/dashboard.spec.js` | KPI presenti, filtri aggiornano i valori |
| Restocking | `tests/e2e/restocking.spec.js` | Scritto dopo R2, ma pianificato ora |

**Output:** `tests/e2e/*.spec.js` — baseline verde (o rosso dove i bug esistono, documentato)

**Prompt utile:**
```
Verifica che playwright MCP sia connesso, poi scrivi i test Playwright per i critical flows
inventory e orders. Eseguili contro localhost:3000 e riporta i risultati.
```

---

## 3. R1 — Reports Module Remediation

**Obiettivo:** Correggere tutti i bug della pagina Reports. Ogni fix va con un test.

**Bug noti da correggere (da audit pre-engagement):**

| # | Bug | File | Fix |
|---|---|---|---|
| B1 | `/api/reports/quarterly` non ha filtri warehouse/category/status | `server/main.py` | Aggiungere parametri + `apply_filters()` |
| B2 | `/api/reports/monthly-trends` non ha filtri | `server/main.py` | Aggiungere parametri + `apply_filters()` |
| B3 | Frontend Reports non passa i filtri agli endpoint | `client/src/views/ReportsView.vue` | Cablare i filtri globali alla chiamata API |
| B4 | Options API residua nei componenti Reports | `client/src/views/ReportsView.vue` | Migrare a Composition API |
| B5 | Console errors su filter state change | da identificare durante audit | Investigate + fix |
| B6–B8+ | Da identificare durante audit completo | TBD | TBD |

**Attività:**
- Audit completo della pagina Reports — enumerare tutti i bug prima di fixare
- Fix in ordine di impatto operativo (filtri prima, poi i18n, poi console noise)
- Ogni fix → test Playwright corrispondente
- Chiudi con checklist di accettazione

**Prompt utile:**
```
Fai un audit completo della pagina Reports: leggi ReportsView.vue e gli endpoint
/api/reports/* in main.py. Elenca tutti i bug trovati prima di toccare il codice.
Poi proponi l'ordine di fix.
```

---

## 4. R2 — Restocking View

**Obiettivo:** Nuova vista che raccomanda purchase orders dato stock + demand + budget.

**Attività:**

### Backend — nuovo endpoint
- File: `server/main.py`
- Endpoint: `GET /api/restocking?budget=&warehouse=&category=`
- Logica:
  1. Prendi items sotto il reorder point da `/api/inventory`
  2. Incrocia con demand forecast da `/api/demand`
  3. Calcola priority score: (reorder_point - quantity_on_hand) × demand_trend_weight
  4. Calcola PO quantity per coprire la domanda prevista
  5. Ordina per priority score, taglia al budget ceiling
- Modello Pydantic: `RestockingRecommendation`

### Frontend — nuova vista
- File: `client/src/views/RestockingView.vue`
- Componenti: input budget, filtri warehouse/category, tabella raccomandazioni, budget tracker
- Pattern: Composition API, stesso stile delle viste esistenti
- Aggiungere voce di menu in `App.vue` o router

> Usa **Plan Mode** (`Shift+Tab`) prima di iniziare il build — è il momento giusto per allinearsi sull'architettura prima di scrivere codice.

**Prompt utile:**
```
[Shift+Tab per Plan Mode]
Voglio costruire la Restocking view: endpoint FastAPI + vista Vue 3.
Input: budget ceiling + filtri opzionali.
Output: lista PO raccomandati ordinati per priorità, budget tracker.
Proponi l'architettura prima di scrivere il codice.
```

---

## 5. Ship it — Commit, Push, PR

**Obiettivo:** Consegnare il lavoro come farebbe un vero engagement.

**Attività:**
- `git add` dei file modificati
- `git commit` con messaggio descrittivo
- `git push` al fork personale
- Aprire PR su GitHub con summary delle modifiche
- Opzionale: installare GitHub App per review automatica (`/install-github-app`)

**Prompt utile:**
```
Crea un commit con tutti i cambiamenti di Act 2, apri una PR sul mio fork con
un summary che copra R1, R2, R3 e R4.
```

---

## Checklist di completamento Act 2

- [ ] App avviata con `/start` e verificata nel browser
- [ ] `proposal/architecture.html` generato e aperto
- [ ] MCP Playwright connesso (`/mcp`)
- [ ] Test baseline scritti per inventory e orders
- [ ] Audit Reports completato — tutti i bug enumerati
- [ ] R1 Reports — tutti i bug fixati con test
- [ ] R2 Restocking — endpoint backend funzionante
- [ ] R2 Restocking — vista frontend funzionante
- [ ] R3 Test Restocking flow aggiunto
- [ ] Commit + Push + PR aperta

---

## Tecniche Claude Code da usare in Act 2

| Momento | Tecnica |
|---|---|
| Avvio app | `/start` — slash command di progetto |
| Verifica MCP | `/mcp` — controlla i server connessi |
| Prima di R2 build | `Shift+Tab` — Plan Mode per allinearsi sull'architettura |
| Context pesante | `/compact` — compatta la conversazione |
| Subagent frontend | `.claude/agents/vue-expert.md` — se il lavoro Vue diventa sostanziale |
| Riferimento a file | `@nomefile` — porta un file nel contesto |
| Riferimento a doc | `#` — cerca nei file del progetto |
| Fine lavoro | `git commit` + `gh pr create` via Claude |
