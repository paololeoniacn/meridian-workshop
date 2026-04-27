# 2 — R3 Automated Browser Testing
> Output: `tests/e2e/*.spec.js` — baseline Playwright sui 5 critical flows
> Prerequisito: MCP Playwright connesso (`/mcp` in Claude Code)

## Verifica MCP

```
/mcp
```
Deve mostrare `playwright` come connesso. Se non appare: riavvia Claude Code nella cartella
e approva i server MCP alla richiesta di avvio.

## 5 Critical flows da coprire

### Flow 1 — Inventory
**File:** `tests/e2e/inventory.spec.js`

```
- Apri localhost:3000
- Naviga a Inventory
- Verifica che la tabella carichi (almeno 1 riga)
- Applica filtro warehouse → verifica che i risultati cambino
- Applica filtro category → verifica che i risultati cambino
- Applica entrambi → verifica risultati filtrati
- Reset filtri → verifica ritorno a lista completa
```

### Flow 2 — Orders
**File:** `tests/e2e/orders.spec.js`

```
- Naviga a Orders
- Verifica caricamento tabella
- Filtra per status "Delivered" → solo delivered visibili
- Filtra per mese → risultati ridotti
- Filtra status + mese insieme
```

### Flow 3 — Reports (baseline — alcuni test saranno rossi)
**File:** `tests/e2e/reports.spec.js`

```
- Naviga a Reports
- Verifica che Quarterly Performance table carichi
- Verifica che Monthly Revenue chart carichi
- Applica filtro warehouse → [ATTESO FALLIMENTO - bug R1]
- Verifica che i dati NON cambino (documenta il bug)
```
> Questi test rossi sono la baseline che dimostra i bug. Non sono errori del test.

### Flow 4 — Dashboard
**File:** `tests/e2e/dashboard.spec.js`

```
- Naviga a Dashboard
- Verifica presenza KPI (total inventory value, low stock, pending orders)
- Applica filtro warehouse → KPI si aggiornano
- Applica filtro category → KPI si aggiornano
- Verifica che i numeri siano > 0
```

### Flow 5 — Restocking (scritto dopo R2)
**File:** `tests/e2e/restocking.spec.js`

```
- Naviga a Restocking (dopo che R2 è implementato)
- Inserisci budget ceiling (es. $50,000)
- Verifica che le raccomandazioni appaiano
- Verifica che il totale non superi il budget
- Filtra per warehouse → raccomandazioni si aggiornano
```

## Struttura file test

```
tests/
└── e2e/
    ├── inventory.spec.js
    ├── orders.spec.js
    ├── reports.spec.js      ← alcuni test rossi = documentazione bug R1
    ├── dashboard.spec.js
    └── restocking.spec.js   ← dopo R2
```

## Prompt utile

```
Verifica che playwright MCP sia connesso con /mcp.
Poi scrivi tests/e2e/inventory.spec.js e tests/e2e/orders.spec.js
usando mcp__playwright__* tools contro localhost:3000.
Eseguili e riporta i risultati.
```
