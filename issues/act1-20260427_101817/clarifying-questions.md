# Clarifying Questions — RFP MC-2026-0417
> Semilavorato Act 1 / Step 3.
> Da inviare a procurement@meridiancomponents.example entro 28 aprile 2026 (§6 RFP).
> Le assunzioni di fallback entrano direttamente nel Technical Approach se non riceviamo risposta.

---

## Q1 — Budget

**Domanda:**
> "The RFP does not include a budget range or not-to-exceed figure. To ensure our pricing proposal is
> calibrated to your expectations, could you share an indicative range or confirm whether a fixed-fee
> or T&M model is preferred?"

**Perché la chiediamo:** Senza un riferimento, rischiamo di sovrastimare (e perdere) o sottostimare
(e perdere margine). Okafor tiene al price — dargli una proposta calibrata aumenta la fiducia.

**Assunzione di fallback:**
Proposta in modalità T&M con not-to-exceed. Stima basata su team di 2 senior consultant per 6 settimane.
R1–R4 obbligatori inclusi nel not-to-exceed. D1–D3 come opzioni separate a richiesta.

---

## Q2 — Critical flows per R3 (testing)

**Domanda:**
> "R3 requires automated browser test coverage for 'critical user flows.' To scope this accurately,
> could your IT team identify the 3–5 flows they consider non-negotiable for sign-off? Examples might
> include: inventory filtering, order status update, reports export, or the new restocking workflow."

**Perché la chiediamo:** "Critical flows" è vago. Se non li definiamo ora, il cliente le sposta in
avanti durante la consegna. IT è il gatekeeper — questa domanda li coinvolge direttamente e li
responsabilizza sulla definizione.

**Assunzione di fallback:**
Copertura dei seguenti 5 flussi principali:
1. Inventory view — caricamento e filtro per warehouse/category
2. Orders view — filtro per status e mese
3. Reports view — quarterly e monthly trends con filtri
4. Restocking view — input budget e generazione raccomandazioni (R2)
5. Dashboard summary — KPI con filtri combinati

---

## Q3 — UI standards per D1

**Domanda:**
> "D1 mentions aligning the UI to 'current standards.' Do you have an existing design system,
> brand guidelines, or a reference UI you'd like us to match? If not, we'll propose a direction
> and iterate with your team."

**Perché la chiediamo:** Senza un riferimento, "modernization" può significare tutto o niente.
Se esiste un brand guideline e non lo seguiamo, il lavoro viene rifiutato. Se non esiste, dobbiamo
dire che proponiamo noi la direzione.

**Assunzione di fallback:**
Nessun design system esistente. Proponiamo un visual refresh conservativo: miglioramento
della spaziatura, gerarchia tipografica, e consistenza dei colori di stato. Nessun redesign
strutturale delle viste. Palette attuale mantenuta come base (slate/gray).

---

## Q4 — Lingue target per D2 (i18n)

**Domanda:**
> "D2 references the Tokyo warehouse team specifically. Beyond Japanese, are there other target
> languages you anticipate needing — now or in the next 12 months? Also, does any existing
> localization infrastructure exist in the codebase (i18n library, translation files), or is
> this to be built from scratch?"

**Perché la chiediamo:** Costruire i18n per una lingua è molto diverso da costruirlo per cinque.
L'architettura cambia. E se c'è già `vue-i18n` installato (ma non lo sappiamo), il lavoro si dimezza.

**Assunzione di fallback:**
Lingua target: giapponese (JA) + inglese (EN) come base. Architettura i18n estendibile (vue-i18n).
Nessuna infrastruttura esistente — da costruire. Stringhe EN estratte da tutte le viste attuali.

---

## Q5 — Deployment e infrastruttura

**Domanda:**
> "The current system appears to run locally. Is there an existing hosting environment
> (internal server, cloud) where the updated system will be deployed, or is deployment
> out of scope for this engagement? If in scope, what are the infrastructure constraints?"

**Perché la chiediamo:** Il vendor precedente non ne parla. Se il deployment è in scope e
non lo prezzizziamo, è un buco nel contratto. Se è fuori scope, va detto esplicitamente
per evitare aspettative.

**Assunzione di fallback:**
Deployment fuori scope. Consegna come applicazione funzionante in locale con documentazione
per il team IT di Meridian su come ospitarla. Eventuali attività di deploy come change request separata.

---

## Riepilogo assunzioni (da inserire nel Technical Approach)

| # | Argomento | Assunzione adottata |
|---|---|---|
| A1 | Budget | T&M con not-to-exceed. Team 2 senior, 6 settimane. D1–D3 opzionali. |
| A2 | Critical flows R3 | 5 flussi coperti: inventory, orders, reports, restocking, dashboard |
| A3 | UI modernization D1 | Visual refresh conservativo. Nessun redesign strutturale. Palette attuale. |
| A4 | i18n D2 | EN + JA. vue-i18n da costruire. Stringhe estratte da tutte le viste. |
| A5 | Deployment | Fuori scope. Documentazione handoff a IT inclusa. |
