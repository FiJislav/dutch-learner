# Comparative/Superlative + Grammar Overview Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add 7 comparative/superlative adjective cards with a new Forms practice mode (table), and add a permanent Grammar Overview reference tab with jump-to-practice buttons.

**Architecture:** All changes in `dutch_learner.html`. New `type:"adjective"` cards added to DECK. New `#adj-box` HTML section + `#overview-box` section. `setMode` / `BOXES` / `updateModeTabs` extended. New JS functions: `buildAdjDeck`, `showAdjForm`, `checkAdjForm`, `buildOverview`, `handleOverviewPractice`.

**Tech Stack:** Vanilla HTML/CSS/JS, no build step.

---

## Files

- Modify: `c:\cody\NL_learning\dutch_learner.html`

---

## Task 1: Add 7 adjective cards to DECK

**File:** `dutch_learner.html` — DECK array, after the `// ── Modal Verbs ──` section and before `// ── Verbs ──`

- [ ] **Step 1: Find the insertion point**

Find this line:
```javascript
  // ── Verbs ──
```

Insert the following block immediately before it:

```javascript
  // ── Adjectives ──
  { id:"adj001", type:"adjective", dutch:"klein", english:"small", tag:"Comparative", regular:true,
    forms:{ comparatief:"kleiner", superlatief:"kleinst", equal:"even klein als" },
    grammarSentences:[
      { s:"Dit kamertje is ___ dan mijn slaapkamer.",   a:["kleiner"],                          full:"Dit kamertje is kleiner dan mijn slaapkamer.",   hint:"en: This little room is smaller than my bedroom.",         topic:"Comparative — comparatief" },
      { s:"Dat is het ___ huis in de straat.",           a:["kleinste"],                         full:"Dat is het kleinste huis in de straat.",          hint:"en: That is the smallest house in the street.",            topic:"Comparative — superlatief" },
      { s:"Jouw tas is ___ de mijne.",                   a:["even klein als","net zo klein als"], full:"Jouw tas is even klein als de mijne.",            hint:"en: Your bag is as small as mine. (even...als / net zo...als)", topic:"Comparative — even...als" }
    ]
  },
  { id:"adj002", type:"adjective", dutch:"groot", english:"big / large", tag:"Comparative", regular:true,
    forms:{ comparatief:"groter", superlatief:"grootst", equal:"even groot als" },
    grammarSentences:[
      { s:"Amsterdam is ___ dan Utrecht.",                a:["groter"],                           full:"Amsterdam is groter dan Utrecht.",                hint:"en: Amsterdam is bigger than Utrecht.",                    topic:"Comparative — comparatief" },
      { s:"Dat is het ___ winkelcentrum van de stad.",    a:["grootste"],                         full:"Dat is het grootste winkelcentrum van de stad.",  hint:"en: That is the biggest shopping centre in the city.",     topic:"Comparative — superlatief" },
      { s:"Zijn huis is ___ het onze.",                   a:["even groot als","net zo groot als"], full:"Zijn huis is even groot als het onze.",           hint:"en: His house is as big as ours.",                         topic:"Comparative — even...als" }
    ]
  },
  { id:"adj003", type:"adjective", dutch:"duur", english:"expensive", tag:"Comparative", regular:true,
    forms:{ comparatief:"duurder", superlatief:"duurst", equal:"even duur als" },
    grammarSentences:[
      { s:"Dit horloge is ___ dan dat horloge.",          a:["duurder"],                          full:"Dit horloge is duurder dan dat horloge.",         hint:"en: This watch is more expensive than that watch.",         topic:"Comparative — comparatief" },
      { s:"Dat is het ___ restaurant in de buurt.",       a:["duurste"],                          full:"Dat is het duurste restaurant in de buurt.",      hint:"en: That is the most expensive restaurant nearby.",         topic:"Comparative — superlatief" },
      { s:"Deze jas is ___ die jas.",                     a:["even duur als","net zo duur als"],   full:"Deze jas is even duur als die jas.",              hint:"en: This coat is as expensive as that coat.",              topic:"Comparative — even...als" }
    ]
  },
  { id:"adj004", type:"adjective", dutch:"goed", english:"good", tag:"Comparative", regular:false,
    forms:{ comparatief:"beter", superlatief:"best", equal:"even goed als" },
    grammarSentences:[
      { s:"Jouw idee is ___ dan het mijne.",              a:["beter"],                            full:"Jouw idee is beter dan het mijne.",               hint:"en: Your idea is better than mine. (irregular: goed → beter)", topic:"Comparative — comparatief (irregular)" },
      { s:"Hij is de ___ student van de klas.",           a:["beste"],                            full:"Hij is de beste student van de klas.",            hint:"en: He is the best student in the class. (irregular: goed → best)", topic:"Comparative — superlatief (irregular)" },
      { s:"Zijn Engels is ___ het mijne.",                a:["even goed als","net zo goed als"],   full:"Zijn Engels is even goed als het mijne.",         hint:"en: His English is as good as mine.",                       topic:"Comparative — even...als" }
    ]
  },
  { id:"adj005", type:"adjective", dutch:"veel", english:"much / many", tag:"Comparative", regular:false,
    forms:{ comparatief:"meer", superlatief:"meest", equal:"even veel als" },
    grammarSentences:[
      { s:"Hij heeft ___ geld dan zijn broer.",           a:["meer"],                             full:"Hij heeft meer geld dan zijn broer.",             hint:"en: He has more money than his brother. (irregular: veel → meer)", topic:"Comparative — comparatief (irregular)" },
      { s:"Zij heeft de ___ boeken van allemaal.",        a:["meeste"],                           full:"Zij heeft de meeste boeken van allemaal.",        hint:"en: She has the most books of all. (irregular: veel → meest)", topic:"Comparative — superlatief (irregular)" },
      { s:"Ik drink ___ koffie als jij.",                 a:["even veel als","net zo veel als"],   full:"Ik drink even veel koffie als jij.",              hint:"en: I drink as much coffee as you.",                        topic:"Comparative — even...als" }
    ]
  },
  { id:"adj006", type:"adjective", dutch:"weinig", english:"little / few", tag:"Comparative", regular:false,
    forms:{ comparatief:"minder", superlatief:"minst", equal:"even weinig als" },
    grammarSentences:[
      { s:"Er zijn ___ mensen dan verwacht.",             a:["minder"],                           full:"Er zijn minder mensen dan verwacht.",             hint:"en: There are fewer people than expected. (irregular: weinig → minder)", topic:"Comparative — comparatief (irregular)" },
      { s:"Hij heeft het ___ geld van de groep.",         a:["minste"],                           full:"Hij heeft het minste geld van de groep.",         hint:"en: He has the least money of the group. (irregular: weinig → minst)", topic:"Comparative — superlatief (irregular)" },
      { s:"Ik slaap ___ mijn zus.",                       a:["even weinig als","net zo weinig als"],full:"Ik slaap even weinig als mijn zus.",            hint:"en: I sleep as little as my sister.",                       topic:"Comparative — even...als" }
    ]
  },
  { id:"adj007", type:"adjective", dutch:"graag", english:"gladly / with pleasure", tag:"Comparative", regular:false,
    forms:{ comparatief:"liever", superlatief:"liefst", equal:"even graag als" },
    grammarSentences:[
      { s:"Ik ga ___ naar de bioscoop dan naar het theater.", a:["liever"],                       full:"Ik ga liever naar de bioscoop dan naar het theater.", hint:"en: I prefer going to the cinema. (irregular: graag → liever)", topic:"Comparative — comparatief (irregular)" },
      { s:"Wat doet hij het ___ van alles?",              a:["liefst"],                           full:"Wat doet hij het liefst van alles?",              hint:"en: What does he like doing most of all? (irregular: graag → liefst)", topic:"Comparative — superlatief (irregular)" },
      { s:"Zij zingt ___ haar zus.",                      a:["even graag als","net zo graag als"], full:"Zij zingt even graag als haar zus.",              hint:"en: She sings as gladly as her sister.",                    topic:"Comparative — even...als" }
    ]
  },

```

- [ ] **Step 2: Verify**

Open browser console, run: `DECK.filter(c => c.type === "adjective").length`
Expected: `7`

- [ ] **Step 3: Commit**
```bash
git add dutch_learner.html
git commit -m "feat: add 7 comparative/superlative adjective cards to DECK"
```

---

## Task 2: Add HTML — adj-box, overview-box, new mode tabs

**File:** `dutch_learner.html` — HTML section

- [ ] **Step 1: Add two new mode tab buttons**

Find:
```html
  <button class="mode-btn" data-mode="verb">🔀 Verbs</button>
```

Replace with:
```html
  <button class="mode-btn" data-mode="verb">🔀 Verbs</button>
  <button class="mode-btn" data-mode="adj">🔡 Forms</button>
  <button class="mode-btn" data-mode="overview">📋 Overview</button>
```

- [ ] **Step 2: Add `#adj-box` after `#verb-box`**

Find:
```html
<!-- Word list -->
```

Insert immediately before it:
```html
<!-- Adjective forms -->
<div class="exercise-box" id="adj-box">
  <div id="adj-title" style="font-size:1.1rem;font-weight:700;color:#f0c070;margin-bottom:4px;"></div>
  <div id="adj-table-el" style="width:100%;overflow-x:auto;">
    <table style="width:100%;border-collapse:collapse;">
      <thead>
        <tr>
          <th style="padding:4px 10px;color:#a0c4ff;text-align:left;font-size:0.78rem;border-bottom:1px solid #1e2d5a;">basisvorm</th>
          <th style="padding:4px 10px;color:#a0c4ff;text-align:left;font-size:0.78rem;border-bottom:1px solid #1e2d5a;">comparatief</th>
          <th style="padding:4px 10px;color:#a0c4ff;text-align:left;font-size:0.78rem;border-bottom:1px solid #1e2d5a;">superlatief</th>
          <th style="padding:4px 10px;color:#a0c4ff;text-align:left;font-size:0.78rem;border-bottom:1px solid #1e2d5a;">even … als</th>
        </tr>
      </thead>
      <tbody id="adj-table-body"></tbody>
    </table>
  </div>
  <div class="ex-feedback" id="adj-feedback"></div>
  <div class="ex-actions">
    <button id="btn-adj-check" style="background:#f0c070;color:#1a1a2e;font-weight:700;">Check ↵</button>
    <button id="btn-adj-prev" style="background:#444;color:#eee;">← Prev</button>
    <button id="btn-adj-next" style="background:#a0c4ff;color:#1a1a2e;">Next →</button>
  </div>
</div>

<!-- Grammar Overview -->
<div class="exercise-box" id="overview-box" style="width:560px;max-width:96vw;align-items:flex-start;gap:16px;">
</div>

```

- [ ] **Step 3: Verify in browser**

The mode tab bar should now show 7 buttons. No JS errors. The new boxes are hidden initially.

- [ ] **Step 4: Commit**
```bash
git add dutch_learner.html
git commit -m "feat: add adj-box, overview-box HTML and two new mode tabs"
```

---

## Task 3: Add CSS for adj-box and overview-box

**File:** `dutch_learner.html` — CSS, after the `.vcell-sentence button` rule (around line 235)

- [ ] **Step 1: Find the end of the verb table CSS section**

Find:
```css
.vcell-sentence button { background: transparent; border: none; cursor: pointer; color: #6c63ff; font-size: 0.82rem; padding: 0 2px; opacity: 0.7; vertical-align: middle; }
```

Add immediately after:
```css

/* ── ADJ FORMS BOX ── */
#adj-box { background: linear-gradient(135deg, #16213e, #0f3460); border: 2px solid #f0c070; width: 560px; }
.adj-inp {
  width: 100%; padding: 4px 8px;
  background: #0d1b38; border: 2px solid #333;
  border-radius: 6px; color: #fff; font-size: 0.88rem;
  outline: none; text-align: center; transition: border-color 0.15s;
}
.adj-inp:focus   { border-color: #f0c070; }
.adj-inp.correct { border-color: #4caf93; background: #0d2b1e; color: #4caf93; }
.adj-inp.wrong   { border-color: #e07070; background: #2b0d0d; color: #e07070; }
.adj-cell { padding: 5px 8px; }
.adj-base { color: #f0c070; font-weight: 700; font-size: 1rem; padding: 5px 10px; }
#adj-table-body tr + tr td { border-top: 1px solid #1e2d5a; }

/* ── OVERVIEW BOX ── */
#overview-box { background: linear-gradient(135deg, #0f1e3a, #0a1628); border: 2px solid #6c63ff; }
.overview-section { width: 100%; border-bottom: 1px solid #1e2d5a; padding-bottom: 12px; }
.overview-section:last-child { border-bottom: none; }
.overview-section h3 { color: #a0c4ff; font-size: 0.95rem; margin-bottom: 6px; }
.overview-section p  { color: #bbb; font-size: 0.82rem; line-height: 1.6; margin-bottom: 8px; }
.overview-table { width: 100%; border-collapse: collapse; font-size: 0.8rem; margin-bottom: 8px; }
.overview-table th { color: #6c63ff; font-weight: 700; padding: 3px 8px; text-align: left; border-bottom: 1px solid #1e2d5a; }
.overview-table td { color: #ddd; padding: 3px 8px; }
.overview-table tr:hover td { background: #1e2d5a; }
.overview-practice-btn {
  background: #6c63ff; color: #fff; font-size: 0.78rem;
  padding: 4px 14px; border-radius: 20px; cursor: pointer; border: none;
  transition: opacity 0.2s;
}
.overview-practice-btn:hover { opacity: 0.8; }
```

- [ ] **Step 2: Verify**

Temporarily switch to adj or overview mode via browser console: `setMode("adj")`. The box should appear with gold/amber border. `setMode("overview")` shows the overview box with dark background and purple border.

Actually — `setMode` doesn't know about "adj" or "overview" yet (added in Task 5). Skip the browser verification for now, just confirm no CSS parse errors by checking that the file opens without errors.

- [ ] **Step 3: Commit**
```bash
git add dutch_learner.html
git commit -m "feat: add CSS for adj-box and overview-box"
```

---

## Task 4: Add adjective JS — state, buildAdjDeck, showAdjForm, checkAdjForm

**File:** `dutch_learner.html` — JS section

- [ ] **Step 1: Add state variables**

Find:
```javascript
let verbViewMode    = "table";
```

Add immediately after:
```javascript
let adjDeck     = [];
let adjIdx      = 0;
let adjAnswered = false;
```

- [ ] **Step 2: Add `buildAdjDeck` after `buildVerbCardQueue`**

Find:
```javascript
function makeSubcatBtn(label, tag) {
```

Insert immediately before it:
```javascript
function buildAdjDeck(deck) {
  return deck.filter(c => c.type === "adjective");
}

```

- [ ] **Step 3: Add `showAdjForm` and `checkAdjForm`**

Find:
```javascript
// ── EVENT LISTENERS: mode tabs ──
```

Insert immediately before it:
```javascript
// ── ADJ FORMS MODE ──
function showAdjForm() {
  const tbody = document.getElementById("adj-table-body");
  document.getElementById("adj-feedback").textContent = "";
  document.getElementById("adj-feedback").className = "ex-feedback";
  adjAnswered = false;
  if (!adjDeck.length) {
    document.getElementById("adj-title").textContent = "No adjectives in this selection.";
    tbody.innerHTML = "";
    return;
  }
  const card = adjDeck[adjIdx];
  document.getElementById("adj-title").textContent = card.dutch + " — " + card.english;
  tbody.innerHTML = "";
  const tr = document.createElement("tr");
  tr.innerHTML = `
    <td class="adj-base">${card.dutch}</td>
    <td class="adj-cell"><input class="adj-inp" type="text" placeholder="…" autocomplete="off" spellcheck="false" data-field="comparatief" data-answer="${card.forms.comparatief}" /></td>
    <td class="adj-cell"><input class="adj-inp" type="text" placeholder="…" autocomplete="off" spellcheck="false" data-field="superlatief" data-answer="${card.forms.superlatief}" /></td>
    <td class="adj-cell"><input class="adj-inp" type="text" placeholder="even … als" autocomplete="off" spellcheck="false" data-field="equal" data-answer="${card.forms.equal}" /></td>
  `;
  tbody.appendChild(tr);
  updateProgress();
  setTimeout(() => tbody.querySelector(".adj-inp")?.focus(), 50);
}

function checkAdjForm() {
  if (adjAnswered) {
    adjIdx = (adjIdx + 1) % adjDeck.length;
    showAdjForm();
    return;
  }
  const inputs = [...document.querySelectorAll("#adj-table-body .adj-inp")];
  if (!inputs.some(inp => inp.value.trim())) return;
  let allCorrect = true;
  inputs.forEach(inp => {
    const val = inp.value.trim().toLowerCase();
    const accepted = inp.dataset.answer.split(" / ").map(s => s.trim().toLowerCase());
    // for equal field also accept "net zo X als" variants
    if (inp.dataset.field === "equal") {
      const base = inp.dataset.answer.replace("even ", "").replace(" als", "");
      accepted.push(`net zo ${base} als`);
    }
    const correct = val && accepted.includes(val);
    inp.classList.toggle("correct", !!correct);
    inp.classList.toggle("wrong", !correct);
    if (!correct) { allCorrect = false; inp.value = inp.dataset.answer; }
  });
  adjAnswered = true;
  const card = adjDeck[adjIdx];
  recordResult(card.id, allCorrect);
  const fb = document.getElementById("adj-feedback");
  if (allCorrect) {
    sessionC++; fb.textContent = "✓ All correct!"; fb.className = "ex-feedback correct";
  } else {
    sessionW++; fb.textContent = "✗ Correct answers shown in red"; fb.className = "ex-feedback wrong";
  }
  updateScoreBar();
}

```

- [ ] **Step 4: Add event listeners for adj buttons**

Find:
```javascript
// ── VERB FORMS LISTENERS ──
```

Insert immediately before it:
```javascript
// ── ADJ FORMS LISTENERS ──
document.getElementById("btn-adj-check").addEventListener("click", checkAdjForm);
document.getElementById("btn-adj-prev").addEventListener("click", () => {
  adjIdx = (adjIdx - 1 + adjDeck.length) % adjDeck.length;
  showAdjForm();
});
document.getElementById("btn-adj-next").addEventListener("click", () => {
  adjIdx = (adjIdx + 1) % adjDeck.length;
  showAdjForm();
});
document.getElementById("adj-table-body").addEventListener("keydown", e => {
  if (e.key === "Enter") checkAdjForm();
});

```

- [ ] **Step 5: Verify in browser console**

`typeof showAdjForm` → `"function"`. `typeof checkAdjForm` → `"function"`. No errors.

- [ ] **Step 6: Commit**
```bash
git add dutch_learner.html
git commit -m "feat: add adjective forms JS — buildAdjDeck, showAdjForm, checkAdjForm, event listeners"
```

---

## Task 5: Wire adj mode into BOXES, setMode, rebuildDecks, updateModeTabs

**File:** `dutch_learner.html`

- [ ] **Step 1: Add "adj" and "overview" to BOXES**

Find:
```javascript
const BOXES = { flip: "scene", type: "type-box", mc: "mc-box", grammar: "grammar-box", verb: "verb-box" };
```

Replace with:
```javascript
const BOXES = { flip: "scene", type: "type-box", mc: "mc-box", grammar: "grammar-box", verb: "verb-box", adj: "adj-box", overview: "overview-box" };
```

- [ ] **Step 2: Add "adj" and "overview" subtitles in setMode**

Find:
```javascript
  const subtitles = {
    flip:    "Click the card to reveal the translation",
    type:    "See the English — type the Dutch word",
    mc:      "See the Dutch — pick the correct English",
    grammar: "Fill in the missing Dutch word",
    verb:    "Type the correct conjugated form",
  };
```

Replace with:
```javascript
  const subtitles = {
    flip:     "Click the card to reveal the translation",
    type:     "See the English — type the Dutch word",
    mc:       "See the Dutch — pick the correct English",
    grammar:  "Fill in the missing Dutch word",
    verb:     "Type the correct conjugated form",
    adj:      "Fill in the comparative, superlative and equal form",
    overview: "Grammar reference — click Practice to start drilling",
  };
```

- [ ] **Step 3: Add showAdjForm call in setMode**

Find:
```javascript
  else if (m === "verb")    showVerbForm();
}
```

Replace with:
```javascript
  else if (m === "verb")    showVerbForm();
  else if (m === "adj")     showAdjForm();
  else if (m === "overview") buildOverview();
}
```

- [ ] **Step 4: Add `adjDeck` to rebuildDecks**

Find:
```javascript
function rebuildDecks() {
  activeDeck    = buildActiveDeck();
  idx           = 0;
  grammarQueue  = buildGrammarQueue(activeDeck);
  grammarIdx    = 0;
  verbDeck      = buildVerbDeck(activeDeck);
  verbIdx       = 0;
  verbCardQueue = buildVerbCardQueue(activeDeck);
  verbCardIdx   = 0;
  updateProgress();
  updateWordTable();
}
```

Replace with:
```javascript
function rebuildDecks() {
  activeDeck    = buildActiveDeck();
  idx           = 0;
  grammarQueue  = buildGrammarQueue(activeDeck);
  grammarIdx    = 0;
  verbDeck      = buildVerbDeck(activeDeck);
  verbIdx       = 0;
  verbCardQueue = buildVerbCardQueue(activeDeck);
  verbCardIdx   = 0;
  adjDeck       = buildAdjDeck(activeDeck);
  adjIdx        = 0;
  updateProgress();
  updateWordTable();
}
```

- [ ] **Step 5: Update updateModeTabs to handle adj and overview tabs**

Find:
```javascript
function updateModeTabs() {
  const wordsActive = contentMode === "words" && isChapterEntry(currentChapter);
  const show = {
    flip: wordsActive, type: wordsActive, mc: wordsActive,
    grammar: !wordsActive,
    verb: !wordsActive && currentChapter !== "grammar-practice"
  };
  Object.entries(show).forEach(([mode, visible]) =>
    document.querySelector(`.mode-btn[data-mode='${mode}']`).style.display = visible ? "" : "none"
  );
  if (!show[currentMode]) currentMode = wordsActive ? "flip" : "grammar";
}
```

Replace with:
```javascript
function updateModeTabs() {
  const wordsActive = contentMode === "words" && isChapterEntry(currentChapter);
  const hasAdj  = adjDeck.length > 0;
  const show = {
    flip:     wordsActive,
    type:     wordsActive,
    mc:       wordsActive,
    grammar:  !wordsActive,
    verb:     !wordsActive && currentChapter !== "grammar-practice",
    adj:      !wordsActive && hasAdj,
    overview: true,
  };
  Object.entries(show).forEach(([mode, visible]) =>
    document.querySelector(`.mode-btn[data-mode='${mode}']`).style.display = visible ? "" : "none"
  );
  if (!show[currentMode]) currentMode = wordsActive ? "flip" : "grammar";
}
```

- [ ] **Step 6: Add "Comparative" to grammar-topics subcat row**

Find:
```javascript
    [["Modal verbs", "Modal verbs"], ["Verb forms", "verbs"]].forEach(([label, tag]) =>
```

Replace with:
```javascript
    [["Modal verbs", "Modal verbs"], ["Verb forms", "verbs"], ["Comparative", "Comparative"]].forEach(([label, tag]) =>
```

- [ ] **Step 7: Update buildActiveDeck for adjective cards**

The current `grammar-topics` branch in `buildActiveDeck` only returns `type:"verb"` cards. Adjective cards have `type:"adjective"` so they'd be missed. Fix:

Find:
```javascript
  } else if (currentChapter === "grammar-topics") {
    const tag = currentSubcat || "Modal verbs";
    cards = DECK.filter(c => c.type === "verb" && c.tag === tag);
```

Replace with:
```javascript
  } else if (currentChapter === "grammar-topics") {
    const tag = currentSubcat || "Modal verbs";
    cards = DECK.filter(c => (c.type === "verb" || c.type === "adjective") && c.tag === tag);
```

Also fix the `contentMode === "grammar"` branch to include adjectives for chapter grammar:

Find:
```javascript
  } else if (contentMode === "grammar") {
    const tag = currentSubcat || getChapterVerbTags(currentChapter)[0];
    cards = tag ? DECK.filter(c => c.type === "verb" && c.tag === tag) : [];
```

Replace with:
```javascript
  } else if (contentMode === "grammar") {
    const tag = currentSubcat || getChapterVerbTags(currentChapter)[0];
    cards = tag ? DECK.filter(c => (c.type === "verb" || c.type === "adjective") && c.tag === tag) : [];
```

- [ ] **Step 8: Verify in browser**

1. Select "Grammar topics" → "Comparative" subcat → Verbs tab shows "No verbs", adj tab appears → click 🔡 Forms → adj table appears showing "klein" with 3 input fields.
2. `BOXES` in console shows 7 entries. No errors.

- [ ] **Step 9: Commit**
```bash
git add dutch_learner.html
git commit -m "feat: wire adj mode into BOXES, setMode, rebuildDecks, updateModeTabs, navigation"
```

---

## Task 6: Add Grammar Overview — buildOverview + handleOverviewPractice

**File:** `dutch_learner.html` — JS, before the `// ── INIT ──` block

- [ ] **Step 1: Add `handleOverviewPractice` and `buildOverview` before the INIT block**

Find:
```javascript
// ── INIT ──
```

Insert immediately before it:

```javascript
// ── GRAMMAR OVERVIEW ──
function handleOverviewPractice(btn) {
  currentChapter = btn.dataset.chapter;
  contentMode    = btn.dataset.contentmode || "grammar";
  currentSubcat  = btn.dataset.subcat || null;
  const targetMode = btn.dataset.mode || "grammar";
  document.getElementById("chapter-select").value = currentChapter;
  updateContentToggle();
  updateSubcatRow();
  updateModeTabs();
  rebuildDecks();
  setMode(targetMode);
}

function buildOverview() {
  const box = document.getElementById("overview-box");
  box.innerHTML = "";

  function section(title, descHtml, tableRows, practiceData) {
    const div = document.createElement("div");
    div.className = "overview-section";
    let html = `<h3>${title}</h3><p>${descHtml}</p>`;
    if (tableRows.length) {
      html += `<table class="overview-table"><thead><tr>${tableRows[0].map(h => `<th>${h}</th>`).join("")}</tr></thead><tbody>`;
      tableRows.slice(1).forEach(row => {
        html += `<tr>${row.map(c => `<td>${c}</td>`).join("")}</tr>`;
      });
      html += `</tbody></table>`;
    }
    div.innerHTML = html;
    const btn = document.createElement("button");
    btn.className = "overview-practice-btn";
    btn.textContent = "Practice →";
    Object.assign(btn.dataset, practiceData);
    btn.addEventListener("click", () => handleOverviewPractice(btn));
    div.appendChild(btn);
    box.appendChild(div);
  }

  section(
    "Vergelijkingen — Comparatief & Superlatief",
    `Gebruik <strong>-er (dan)</strong> voor de comparatief en <strong>(het) -st</strong> voor de superlatief. ` +
    `Zijn dingen gelijk? Gebruik <strong>even … als</strong> of <strong>net zo … als</strong>: ` +
    `<em>Mijn moeder is kleiner dan ik. Jouw kamer is even groot als mijn kamer. Ik ben net zo groot als jij.</em> ` +
    `Let op onregelmatige vormen: goed → beter → best, veel → meer → meest, weinig → minder → minst, graag → liever → liefst.`,
    [
      ["basisvorm", "comparatief", "superlatief", "even … als"],
      ["klein", "kleiner (dan)", "(het) kleinst", "even klein als"],
      ["groot", "groter (dan)", "(het) grootst", "even groot als"],
      ["duur", "duurder (dan)", "(het) duurst", "even duur als"],
      ["goed ⚠️", "beter (dan)", "(het) best", "even goed als"],
      ["veel ⚠️", "meer (dan)", "(het) meest", "even veel als"],
      ["weinig ⚠️", "minder (dan)", "(het) minst", "even weinig als"],
      ["graag ⚠️", "liever (dan)", "(het) liefst", "even graag als"],
    ],
    { chapter: "grammar-topics", contentmode: "grammar", subcat: "Comparative", mode: "adj" }
  );

  section(
    "Modale werkwoorden",
    `Modale werkwoorden staan altijd met een infinitief: <em>Ik <strong>kan</strong> zwemmen. Hij <strong>moet</strong> werken.</em> ` +
    `<strong>kunnen</strong> = can/able to · <strong>willen</strong> = want to · <strong>moeten</strong> = must/have to · ` +
    `<strong>mogen</strong> = may/allowed to · <strong>zullen</strong> = will/shall · ` +
    `<strong>hoeven</strong> = need to (bijna altijd met <em>niet</em>: <em>je hoeft niet te betalen</em>). ` +
    `Let op: <em>niet moeten</em> = must not (verbod!), <em>niet hoeven</em> = don't need to.`,
    [
      ["modal", "ik", "jij/je", "hij/zij/het", "wij/jullie/zij"],
      ["kunnen", "kan", "kunt/kan", "kan", "kunnen"],
      ["willen", "wil", "wilt/wil", "wil", "willen"],
      ["moeten", "moet", "moet", "moet", "moeten"],
      ["mogen", "mag", "mag", "mag", "mogen"],
      ["zullen", "zal", "zal/zult", "zal", "zullen"],
      ["hoeven", "hoef", "hoeft", "hoeft", "hoeven"],
    ],
    { chapter: "grammar-topics", contentmode: "grammar", subcat: "Modal verbs", mode: "verb" }
  );

  section(
    "Werkwoordsvervoeging — Tegenwoordige tijd",
    `De stam = infinitief minus <strong>-en</strong> (werken → werk). ` +
    `<strong>ik</strong> = stam · <strong>jij/je/u/hij/zij/het</strong> = stam + <em>-t</em> ` +
    `(tenzij de stam al op -t eindigt, en let op: <em>jij werkt</em> maar in inversie <em>werk jij</em>). ` +
    `<strong>wij/jullie/zij</strong> = infinitief. ` +
    `Verdubbeling: stam eindigt op korte klank + consonant → verdubbel: <em>stoppen → ik stop, jij stopt</em>.`,
    [
      ["pronoun", "werken", "stoppen", "hebben"],
      ["ik", "werk", "stop", "heb"],
      ["jij / je", "werkt", "stopt", "hebt"],
      ["u", "werkt", "stopt", "hebt/heeft"],
      ["hij / zij / het", "werkt", "stopt", "heeft"],
      ["wij / jullie / zij", "werken", "stoppen", "hebben"],
    ],
    { chapter: "Ch 7", contentmode: "grammar", subcat: "Ch 7", mode: "verb" }
  );
}

```

- [ ] **Step 2: Verify in browser**

Select any chapter, switch to Grammar mode, click 📋 Overview tab. The overview box should appear with 3 sections, each with a table and a "Practice →" button. Clicking "Practice →" on Comparatief should switch to Grammar topics → Comparative → Forms mode.

- [ ] **Step 3: Commit**
```bash
git add dutch_learner.html
git commit -m "feat: add Grammar Overview tab with buildOverview and handleOverviewPractice"
```

---

## Self-Review

### Spec coverage
- [x] 7 adjective cards (klein, groot, duur, goed, veel, weinig, graag) → Task 1
- [x] `grammarSentences` per card with `net zo...als` as alternate accepted answer → Task 1
- [x] `#adj-box` HTML with table (basisvorm + 3 inputs) → Task 2
- [x] `#overview-box` HTML → Task 2
- [x] `🔡 Forms` and `📋 Overview` mode tab buttons → Task 2
- [x] adj-box CSS (gold/amber theme), overview-box CSS → Task 3
- [x] `buildAdjDeck`, `showAdjForm`, `checkAdjForm` → Task 4
- [x] adj event listeners (check, prev, next, enter key) → Task 4
- [x] BOXES updated, setMode updated, rebuildDecks updated → Task 5
- [x] updateModeTabs: Forms tab visible when `hasAdj`, Overview always visible → Task 5
- [x] "Comparative" subcategory in grammar-topics → Task 5
- [x] `buildActiveDeck` includes `type:"adjective"` → Task 5
- [x] `buildOverview` with 3 sections + Practice buttons → Task 6
- [x] `handleOverviewPractice` sets state and switches mode → Task 6

### Placeholder scan
None — all code is complete.

### Type consistency
- `adjDeck`, `adjIdx`, `adjAnswered` declared in Task 4, used in Task 5 (`rebuildDecks`) and Task 4 (`showAdjForm`/`checkAdjForm`) ✓
- `buildAdjDeck` defined before `rebuildDecks` calls it ✓
- `buildOverview` called in `setMode` (Task 5) and defined in Task 6 — these are in the same `<script>` block so declaration order doesn't matter for function declarations ✓
