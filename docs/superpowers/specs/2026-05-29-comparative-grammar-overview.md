# Comparative/Superlative + Grammar Overview — Design Spec
**Date:** 2026-05-29
**File:** `c:\cody\NL_learning\dutch_learner.html`

---

## Overview

Two additions:
1. **Comparative/Superlative adjective cards** — new `type:"adjective"` data model, a new "Forms" practice mode (table + cards), and fill-in grammar sentences.
2. **Grammar Overview tab** — always-visible reference tab showing all grammar phenomena with descriptions and jump-to-practice buttons.

---

## Part 1 — Adjective Cards

### Data Model

```javascript
{
  id: "adj001",
  type: "adjective",
  dutch: "klein",           // base form
  english: "small",
  tag: "Comparative",
  regular: true,            // false for goed/veel/weinig/graag
  forms: {
    comparatief: "kleiner", // fill-in column 1
    superlatief: "kleinst", // fill-in column 2
    equal: "even klein als" // fill-in column 3
  },
  grammarSentences: [
    {
      s: "Dit huis is ___ dan dat huis.",
      a: ["kleiner"],
      full: "Dit huis is kleiner dan dat huis.",
      hint: "en: This house is smaller than that house.",
      topic: "Comparative — comparatief"
    }
  ]
}
```

### Cards to add (7 total)

**Regular (tag: "Comparative", regular: true):**
| dutch | english | comparatief | superlatief | equal |
|---|---|---|---|---|
| klein | small | kleiner | kleinst | even klein als |
| groot | big | groter | grootst | even groot als |
| duur | expensive | duurder | duurst | even duur als |

**Irregular (tag: "Comparative", regular: false):**
| dutch | english | comparatief | superlatief | equal |
|---|---|---|---|---|
| goed | good | beter | best | even goed als |
| veel | much/many | meer | meest | even veel als |
| weinig | little/few | minder | minst | even weinig als |
| graag | gladly | liever | liefst | even graag als |

Each card gets 2–3 `grammarSentences` covering comparatief, superlatief, and equal forms.

### Grammar sentences per card

**klein:**
- comparatief: `{ s:"Dit kamertje is ___ dan mijn slaapkamer.", a:["kleiner"], full:"Dit kamertje is kleiner dan mijn slaapkamer.", hint:"en: This little room is smaller than my bedroom.", topic:"Comparative — comparatief" }`
- superlatief: `{ s:"Dat is het ___ huis in de straat.", a:["kleinste"], full:"Dat is het kleinste huis in de straat.", hint:"en: That is the smallest house in the street.", topic:"Comparative — superlatief" }`
- equal: `{ s:"Jouw tas is ___ ___ ___ de mijne.", a:["even klein als"], full:"Jouw tas is even klein als de mijne.", hint:"en: Your bag is as small as mine.", topic:"Comparative — even...als" }`

*(Note: superlatief in a sentence uses the inflected form `kleinste`, not `kleinst`. This distinction is noted in the hint.)*

**groot:**
- `{ s:"Amsterdam is ___ dan Utrecht.", a:["groter"], full:"Amsterdam is groter dan Utrecht.", hint:"en: Amsterdam is bigger than Utrecht.", topic:"Comparative — comparatief" }`
- `{ s:"Dat is het ___ winkelcentrum van de stad.", a:["grootste"], full:"Dat is het grootste winkelcentrum van de stad.", hint:"en: That is the biggest shopping centre in the city.", topic:"Comparative — superlatief" }`
- `{ s:"Zijn huis is ___ ___ ___ het onze.", a:["even groot als"], full:"Zijn huis is even groot als het onze.", hint:"en: His house is as big as ours.", topic:"Comparative — even...als" }`

**duur:**
- `{ s:"Dit horloge is ___ dan dat horloge.", a:["duurder"], full:"Dit horloge is duurder dan dat horloge.", hint:"en: This watch is more expensive than that watch.", topic:"Comparative — comparatief" }`
- `{ s:"Dat is het ___ restaurant in de buurt.", a:["duurste"], full:"Dat is het duurste restaurant in de buurt.", hint:"en: That is the most expensive restaurant in the neighbourhood.", topic:"Comparative — superlatief" }`
- `{ s:"Deze jas is ___ ___ ___ die jas.", a:["even duur als"], full:"Deze jas is even duur als die jas.", hint:"en: This coat is as expensive as that coat.", topic:"Comparative — even...als" }`

**goed (irregular):**
- `{ s:"Jouw idee is ___ dan het mijne.", a:["beter"], full:"Jouw idee is beter dan het mijne.", hint:"en: Your idea is better than mine. (irregular: goed → beter)", topic:"Comparative — comparatief (irregular)" }`
- `{ s:"Hij is de ___ student van de klas.", a:["beste"], full:"Hij is de beste student van de klas.", hint:"en: He is the best student in the class. (irregular: goed → best)", topic:"Comparative — superlatief (irregular)" }`
- `{ s:"Zijn Engels is ___ ___ ___ het mijne.", a:["even goed als"], full:"Zijn Engels is even goed als het mijne.", hint:"en: His English is as good as mine.", topic:"Comparative — even...als" }`

**veel (irregular):**
- `{ s:"Hij heeft ___ geld dan zijn broer.", a:["meer"], full:"Hij heeft meer geld dan zijn broer.", hint:"en: He has more money than his brother. (irregular: veel → meer)", topic:"Comparative — comparatief (irregular)" }`
- `{ s:"Zij heeft de ___ boeken van allemaal.", a:["meeste"], full:"Zij heeft de meeste boeken van allemaal.", hint:"en: She has the most books of all. (irregular: veel → meest)", topic:"Comparative — superlatief (irregular)" }`
- `{ s:"Ik drink ___ ___ ___ koffie als jij.", a:["even veel als"], full:"Ik drink even veel koffie als jij.", hint:"en: I drink as much coffee as you.", topic:"Comparative — even...als" }`

**weinig (irregular):**
- `{ s:"Er zijn ___ mensen dan verwacht.", a:["minder"], full:"Er zijn minder mensen dan verwacht.", hint:"en: There are fewer people than expected. (irregular: weinig → minder)", topic:"Comparative — comparatief (irregular)" }`
- `{ s:"Hij heeft het ___ geld van de groep.", a:["minste"], full:"Hij heeft het minste geld van de groep.", hint:"en: He has the least money of the group. (irregular: weinig → minst)", topic:"Comparative — superlatief (irregular)" }`
- `{ s:"Ik slaap ___ ___ ___ mijn zus.", a:["even weinig als"], full:"Ik slaap even weinig als mijn zus.", hint:"en: I sleep as little as my sister.", topic:"Comparative — even...als" }`

**graag (irregular):**
- `{ s:"Ik ga ___ naar de bioscoop dan naar het theater.", a:["liever"], full:"Ik ga liever naar de bioscoop dan naar het theater.", hint:"en: I prefer going to the cinema over the theatre. (irregular: graag → liever)", topic:"Comparative — comparatief (irregular)" }`
- `{ s:"Wat doet hij het ___ van alles?", a:["liefst"], full:"Wat doet hij het liefst van alles?", hint:"en: What does he like doing most of all? (irregular: graag → liefst)", topic:"Comparative — superlatief (irregular)" }`
- `{ s:"Zij zingt ___ ___ ___ haar zus.", a:["even graag als"], full:"Zij zingt even graag als haar zus.", hint:"en: She sings as gladly as her sister.", topic:"Comparative — even...als" }`

---

## Part 2 — Adjective Forms Practice Mode

### New HTML section `#adj-box`

```html
<div id="adj-box" class="ex-box" style="display:none">
  <div id="adj-infinitive" class="verb-infinitive"></div>
  <!-- table view -->
  <div id="adj-table-el">
    <table>
      <thead>
        <tr>
          <th>basisvorm</th>
          <th>comparatief</th>
          <th>superlatief</th>
          <th>even … als</th>
        </tr>
      </thead>
      <tbody id="adj-table-body"></tbody>
    </table>
  </div>
  <div id="adj-feedback" class="ex-feedback"></div>
  <div class="ex-controls">
    <button id="btn-adj-check">Check ↵</button>
    <button id="btn-adj-prev">← Prev</button>
    <button id="btn-adj-next">Next →</button>
  </div>
</div>
```

### Mode tab

Add `<button class="mode-btn" data-mode="adj">🔡 Forms</button>` after the Grammar tab. Visible only when `activeDeck` contains adjective cards — controlled by `updateModeTabs()`.

### State variables

```javascript
let adjDeck  = [];
let adjIdx   = 0;
let adjAnswered = false;
```

### `buildAdjDeck(deck)`

```javascript
function buildAdjDeck(deck) {
  return deck.filter(c => c.type === "adjective");
}
```

### `showAdjForm()`

Renders one row per adjective in `adjDeck[adjIdx]`:
- Column 1: basisvorm (read-only text)
- Columns 2–4: `<input>` fields with `data-answer` set to the form value

### `checkAdjForm()`

Checks all 3 inputs. Marks correct/wrong. On wrong: fills in correct answer. Reveals grammar sentences below (like verb table).

### Navigation

Prev/Next cycle through `adjDeck`. Check button advances after answering (same pattern as verb table).

---

## Part 3 — Grammar Overview Tab

### New mode tab

```html
<button class="mode-btn" data-mode="overview">📋 Overview</button>
```

**Always visible** — exempt from `updateModeTabs()` hiding logic.

### New HTML section `#overview-box`

Static content generated at startup by `buildOverview()`. Structure:

```
── Vergelijkingen (Comparatives) ──────────────
[Rule text]
[Mini table: klein → kleiner → kleinst]
[Practice →] button

── Modale werkwoorden (Modal verbs) ──────────
[Rule/usage text per modal]
[Practice →] button

── Werkwoordsvervoeging (Verb conjugation) ───
[Rule text]
[Practice →] button
```

### `buildOverview()`

Generates the overview HTML from static content (not from DECK). Each section has:
- `<h3>` title
- `<p>` description (2–4 sentences)
- Example table or list
- `<button class="overview-practice-btn">Practice →</button>` with `data-chapter`, `data-contentmode`, `data-subcat`, `data-mode` attributes

### `handleOverviewPractice(btn)`

Reads data attributes from the button:
```javascript
currentChapter = btn.dataset.chapter;
contentMode    = btn.dataset.contentmode;
currentSubcat  = btn.dataset.subcat || null;
updateContentToggle(); updateSubcatRow(); updateModeTabs();
rebuildDecks();
setMode(btn.dataset.mode);
```

---

## Part 4 — Navigation Updates

### Grammar topics subcategory row

Add "Comparative" button alongside "Modal verbs" and "Verb forms":
```javascript
[["Modal verbs", "Modal verbs"], ["Verb forms", "verbs"], ["Comparative", "Comparative"]]
```

`buildActiveDeck()` for `grammar-topics` already handles this via `c.tag === tag`.

### `updateModeTabs()` changes

- Forms tab (`adj`): visible when `activeDeck` has adjective cards
- Overview tab: always visible (excluded from show/hide logic)

### `rebuildDecks()` addition

```javascript
adjDeck = buildAdjDeck(activeDeck);
adjIdx  = 0;
```

---

## State variables summary (new)

| Variable | Type | Purpose |
|---|---|---|
| `adjDeck` | array | Adjective cards for Forms mode |
| `adjIdx` | number | Current position in adjDeck |
| `adjAnswered` | boolean | Whether current card has been checked |

---

## Out of Scope

- No audio (TTS) for adjective forms
- No weak-word tracking for adjective form slots (only per-card tracking)
- No in-app editing of grammar rules
- Ch 8 vocabulary words not in this spec
