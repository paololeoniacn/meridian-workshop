# 3 — R1 Reports Module Remediation
> File da toccare: `server/main.py`, `client/src/views/Reports.vue`, `client/src/api.js`
> Regola: ogni fix → test Playwright corrispondente

## Bug catalog (da audit pre-engagement)

### BUG-01 — Endpoint quarterly senza filtri [BACKEND]
**File:** `server/main.py` L231–274
**Problema:** `GET /api/reports/quarterly` non accetta warehouse/category/status/month
**Fix:**
```python
@app.get("/api/reports/quarterly")
def get_quarterly_reports(
    warehouse: Optional[str] = None,
    category: Optional[str] = None,
    status: Optional[str] = None,
):
    filtered_orders = apply_filters(orders, warehouse, category, status)
    # ... resto della logica invariata ma su filtered_orders
```

### BUG-02 — Endpoint monthly-trends senza filtri [BACKEND]
**File:** `server/main.py` L276–305
**Problema:** `GET /api/reports/monthly-trends` non accetta filtri
**Fix:** stesso pattern di BUG-01

### BUG-03 — Reports.vue non usa useFilters() [FRONTEND]
**File:** `client/src/views/Reports.vue` L127+
**Problema:** il componente chiama `axios` direttamente, ignorando il filtro globale di `useFilters.js`
**Fix:** importare `useFilters`, leggere `getCurrentFilters()` e passarli all'API call

### BUG-04 — Reports.vue bypass di api.js [FRONTEND]
**File:** `client/src/views/Reports.vue` L156, L162
**Problema:** usa `axios.get('http://localhost:8001/api/reports/...')` direttamente
invece di usare il client centralizzato `api.js`
**Fix:**
1. Aggiungere `getQuarterlyReports(filters)` e `getMonthlyTrends(filters)` in `api.js`
2. Sostituire le chiamate axios dirette in `Reports.vue`

### BUG-05 — Options API invece di Composition API [FRONTEND]
**File:** `client/src/views/Reports.vue`
**Problema:** tutto il componente usa `export default { data(), mounted(), methods: {} }`
invece del pattern `setup()` usato in tutto il resto del progetto
**Fix:** riscrivere con Composition API (ref, computed, onMounted, watch)

### BUG-06 — Console noise (7 console.log) [FRONTEND]
**File:** `client/src/views/Reports.vue` L145, L153, L158, L163, L167, L214, L256, L262
**Problema:** log di debug lasciati in produzione — ogni render chiama `formatNumber` e `getBarHeight`
con un log → spam in console
**Fix:** rimuovere tutti i `console.log` di debug

### BUG-07 — calculateSummaryStats usa ES5 (var + for loop) [FRONTEND]
**File:** `client/src/views/Reports.vue` L181–211
**Problema:** usa `var` con shadowing (3 variabili `i` nello stesso scope) e loop for stile ES5
**Fix:** riscrivere con `const`, `reduce()`, `Math.max()` — già parte della migrazione BUG-05

### BUG-08 — v-for con index come key [FRONTEND]
**File:** `client/src/views/Reports.vue` L28, L51, L82
**Problema:** `:key="index"` in tutti i v-for invece di ID unici
**Fix:** usare `q.quarter` e `month.month` come key (sono unici per definizione)

## Ordine di fix consigliato

```
1. BUG-01 + BUG-02  → backend filtri (sblocca tutto il resto)
2. BUG-04           → api.js aggiornato
3. BUG-03           → Reports.vue usa useFilters
4. BUG-05 + BUG-07  → migrazione Composition API (refactor completo)
5. BUG-06           → rimozione console.log
6. BUG-08           → fix v-for keys
```

## Watch per filtri (da aggiungere dopo migrazione)

```javascript
// In setup() di Reports.vue dopo migrazione
const { getCurrentFilters } = useFilters()

watch(getCurrentFilters, async () => {
  await loadData()
}, { deep: true })
```

## Prompt utile

```
Fai un audit di client/src/views/Reports.vue e degli endpoint
/api/reports/* in server/main.py.
Poi fixa BUG-01 e BUG-02 (filtri backend) e aggiungi getQuarterlyReports()
e getMonthlyTrends(filters) in api.js.
```
