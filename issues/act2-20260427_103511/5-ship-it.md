# 5 — Ship it
> Commit + Push + PR sul fork personale

## Prerequisiti

- [ ] R4 architecture.html in `proposal/`
- [ ] R3 test baseline verde (o rosso documentato per Reports)
- [ ] R1 tutti i bug fixati
- [ ] R2 Restocking view funzionante
- [ ] App avviata e verificata manualmente su localhost:3000

## Comandi

```bash
# Verifica stato
git status
git diff

# Stage e commit
git add proposal/architecture.html
git add tests/e2e/
git add server/main.py
git add client/src/views/Reports.vue
git add client/src/views/Restocking.vue
git add client/src/api.js

git commit -m "feat: deliver R1-R4 for Meridian Components engagement

- R4: add interactive architecture documentation (proposal/architecture.html)
- R3: add Playwright e2e tests for 5 critical flows
- R1: fix Reports module - wire filters, migrate to Composition API, remove console noise
- R2: add Restocking view with budget ceiling and PO recommendations"

# Push al fork
git push origin main

# Apri PR
gh pr create \
  --title "Meridian engagement: R1 Reports fix, R2 Restocking, R3 Tests, R4 Architecture" \
  --body "..."
```

## Prompt utile

```
Crea un commit con tutti i cambiamenti di Act 2 e apri una PR sul mio fork.
Il titolo deve coprire R1, R2, R3 e R4.
Includi nel body: summary dei cambiamenti, test plan, screenshot se disponibili.
```

## Opzionale — GitHub App per review automatica

```
/install-github-app
```
Segui il flusso OAuth nel browser — Claude non può farlo per te.
