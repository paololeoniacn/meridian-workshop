# 4 — R2 Restocking View
> File da creare: `server/main.py` (endpoint), `client/src/views/Restocking.vue`
> File da modificare: `client/src/api.js`, `client/src/App.vue` (nav)
> Usa Plan Mode (Shift+Tab) prima di iniziare

## Architettura

```
Operatore inserisce budget ceiling + filtri opzionali
    ↓
GET /api/restocking?budget=50000&warehouse=SF&category=Sensors
    ↓
FastAPI:
  1. Filtra inventory items sotto reorder point
  2. Incrocia con demand forecasts per trend
  3. Calcola priority score per item
  4. Ordina per priority score DESC
  5. Taglia lista al budget ceiling
    ↓
[ { sku, name, qty_to_order, unit_cost, total_cost, priority_score, trend }, ... ]
    ↓
Vue: tabella raccomandazioni + budget tracker
```

## Backend — nuovo endpoint

**File:** `server/main.py`

### Pydantic model
```python
class RestockingRecommendation(BaseModel):
    id: str
    sku: str
    name: str
    category: str
    warehouse: str
    current_stock: int
    reorder_point: int
    qty_to_order: int
    unit_cost: float
    total_cost: float
    priority_score: float
    demand_trend: str          # "increasing" | "stable" | "decreasing"
    days_to_stockout: Optional[int] = None
```

### Logica priority score
```python
TREND_WEIGHT = {"increasing": 1.5, "stable": 1.0, "decreasing": 0.5}

def calc_priority(item, forecast):
    stock_gap = item["reorder_point"] - item["quantity_on_hand"]
    trend_w = TREND_WEIGHT.get(forecast.get("trend", "stable"), 1.0)
    return round(stock_gap * trend_w * item["unit_cost"], 2)
```

### Endpoint
```python
@app.get("/api/restocking", response_model=List[RestockingRecommendation])
def get_restocking_recommendations(
    budget: float = Query(..., gt=0),
    warehouse: Optional[str] = None,
    category: Optional[str] = None,
):
```

## Frontend — nuova vista

**File:** `client/src/views/Restocking.vue`

### Layout
```
┌─────────────────────────────────────────┐
│  Budget ceiling: [___________] [Apply]  │
│  Warehouse: [All ▼]  Category: [All ▼]  │
├─────────────────────────────────────────┤
│  Budget used: $32,450 / $50,000 (64%)   │
│  [████████████░░░░░░░]                  │
├─────────────────────────────────────────┤
│  SKU  | Name | Qty | Unit$ | Total$ | P │
│  ···  | ···  | ··· | ····  | ·····  | H │
│  ···  | ···  | ··· | ····  | ·····  | M │
└─────────────────────────────────────────┘
```

### Composizione Composition API
```javascript
setup() {
  const budget = ref(50000)
  const recommendations = ref([])
  const loading = ref(false)
  const { selectedLocation, selectedCategory } = useFilters()

  const budgetUsed = computed(() =>
    recommendations.value.reduce((sum, r) => sum + r.total_cost, 0)
  )

  const budgetPercent = computed(() =>
    Math.min(100, (budgetUsed.value / budget.value) * 100)
  )

  const loadRecommendations = async () => { ... }

  watch([budget, selectedLocation, selectedCategory], loadRecommendations)
}
```

## api.js — aggiungere

```javascript
async getRestockingRecommendations(budget, filters = {}) {
  const params = new URLSearchParams({ budget })
  if (filters.warehouse && filters.warehouse !== 'all')
    params.append('warehouse', filters.warehouse)
  if (filters.category && filters.category !== 'all')
    params.append('category', filters.category)
  const response = await axios.get(`${API_BASE_URL}/restocking?${params}`)
  return response.data
},
```

## App.vue — aggiungere voce nav

```javascript
{ path: '/restocking', name: 'Restocking', icon: '📦' }
```

## Prompt utile

```
[Shift+Tab — Plan Mode]
Voglio costruire la Restocking view per Meridian.
Backend: nuovo endpoint GET /api/restocking in server/main.py
  - parametri: budget (required), warehouse, category
  - logica: items sotto reorder point × demand trend → priority score → taglia al budget
Frontend: client/src/views/Restocking.vue
  - input budget + filtri, tabella raccomandazioni, budget tracker
Composition API, stesso stile viste esistenti.
Proponi il piano prima di scrivere.
```
