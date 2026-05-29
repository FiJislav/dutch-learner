# Chapter & Grammar Navigation — Design Spec
**Date:** 2026-05-29
**File:** `c:\cody\NL_learning\dutch_learner.html`

---

## Overview

Restructure the top-level navigation so the user can pick a chapter or grammar area, then within a chapter choose between word drills and grammar (verb conjugation). Grammar practice (fill-in sentences) and grammar topic drills (modal verbs, zijn/hebben) become first-class top-level entries.

---

## Current State

Single `chapter-select` dropdown with flat options:
- All cards
- Ch 7: In een kledingzaak
- Modal verbs
- Verbs only

`buildActiveDeck()` filters `DECK` by `currentTag`. No concept of words vs grammar within a chapter.

---

## New Navigation Structure

### Top controls bar

```
[Chapter ▼]   [Words | Grammar]   [⚡ Weak words]
```

- **[Words | Grammar] toggle** — visible only when a chapter entry is selected (All, Ch 7, Ch 8, …). Hidden when Grammar practice or Grammar topics is selected.
- A **subcategory row** appears below the top controls when Grammar is active or a grammar top-level entry is selected. Hidden otherwise.

### Chapter dropdown options

```
All
Ch 7: In een kledingzaak
Ch 8: (populated as chapters are added)
─────────────────────────────────────
Grammar practice
Grammar topics
```

The divider (`<optgroup>` or `<hr>` in the dropdown) visually separates chapter entries from grammar-only entries.

---

## Behaviour Per Selection

### Chapter selected (All / Ch 7 / Ch 8) + Words active

- `activeDeck` = word cards (`type:"word"`) matching the chapter tag (or all chapters for "All")
- [Words | Grammar] toggle visible, Words highlighted
- All 5 mode tabs available
- No subcategory row

### Chapter selected (All / Ch 7 / Ch 8) + Grammar active

- Subcategory row appears with one button per verb tag present in that chapter:
  - Ch 7 selected → **Verb conjugation (Ch 7)**
  - All selected → **Verb conjugation (Ch 7)** · **Verb conjugation (Ch 8)** · …
- `activeDeck` = verb cards (`type:"verb"`) matching the selected subcategory tag
- Verbs mode tab is primary; all 5 tabs still available

### Grammar practice selected

- [Words | Grammar] toggle hidden
- No subcategory row
- `activeDeck` = all word cards from all chapters (ignores chapter filter)
- Grammar mode tab is primary; all 5 tabs still available

### Grammar topics selected

- [Words | Grammar] toggle hidden
- Subcategory row: **Modal verbs** · **Verb forms** (zijn/hebben)
- Default subcategory: Modal verbs
- **Modal verbs**: `activeDeck` = verb cards tagged `"Modal verbs"` (v014–v019)
- **Verb forms**: `activeDeck` = verb cards tagged `"verbs"` (v001 zijn, v002 hebben)
- Default subcategory on first load: Modal verbs
- Verbs mode tab is primary; Grammar mode tab shows fill-in sentences (`grammarSentences`) for modals; all 5 tabs available

---

## State Model

Two new state variables replace `currentTag`:

```javascript
currentChapter  // "all" | "Ch 7" | "Ch 8" | "grammar-practice" | "grammar-topics"
currentSubcat   // null | "Ch 7" | "Ch 8" | "Modal verbs" | "verbs"
contentMode     // "words" | "grammar"  (only relevant when currentChapter is a chapter entry)
```

`buildActiveDeck()` derives the deck from these three variables.

---

## UI Components

### Words/Grammar toggle

Two buttons styled as a segmented control (reuse existing `.mode-btn` active/inactive pattern):

```html
<div id="content-toggle" style="display:none">
  <button id="btn-words" class="content-btn active">📖 Words</button>
  <button id="btn-grammar" class="content-btn">✏️ Grammar</button>
</div>
```

Shown/hidden by JS when chapter selection changes.

### Subcategory row

```html
<div id="subcat-row" style="display:none">
  <!-- populated dynamically by JS -->
</div>
```

Buttons inside use same `.content-btn` styling; active one highlighted. Clicking a subcategory button updates `currentSubcat` and rebuilds the deck.

---

## buildActiveDeck() Logic

```javascript
function buildActiveDeck() {
  let cards;

  if (currentChapter === "grammar-practice") {
    cards = DECK.filter(c => c.type === "word");

  } else if (currentChapter === "grammar-topics") {
    const tag = currentSubcat || "Modal verbs";
    cards = DECK.filter(c => c.type === "verb" && c.tag === tag);

  } else if (contentMode === "grammar") {
    // chapter selected + Grammar toggle active
    const tag = currentSubcat;
    cards = tag ? DECK.filter(c => c.type === "verb" && c.tag === tag)
                : DECK.filter(c => c.type === "verb");

  } else {
    // chapter selected + Words toggle active
    cards = currentChapter === "all"
      ? DECK.filter(c => c.type === "word")
      : DECK.filter(c => c.type === "word" && c.tag === currentChapter);
  }

  if (weakMode) cards = cards.filter(c => isWeak(c.id));
  return cards;
}
```

---

## Subcategory Row Population

```javascript
function updateSubcatRow() {
  const row = document.getElementById("subcat-row");
  row.innerHTML = "";
  row.style.display = "none";

  if (currentChapter === "grammar-topics") {
    row.style.display = "";
    [["Modal verbs", "Modal verbs"], ["Verb forms", "verbs"]].forEach(([label, tag]) => {
      const btn = makeSubcatBtn(label, tag);
      row.appendChild(btn);
    });

  } else if (contentMode === "grammar" && isChapterEntry(currentChapter)) {
    // collect verb tags available in this chapter
    const tags = [...new Set(
      DECK.filter(c => c.type === "verb" && (currentChapter === "all" || c.tag === currentChapter))
          .map(c => c.tag)
    )];
    if (tags.length) {
      // default to first tag if currentSubcat not valid for this chapter
      if (!tags.includes(currentSubcat)) currentSubcat = tags[0];
      row.style.display = "";
      tags.forEach(tag => {
        const label = tag === "verbs" ? "Verb forms" : `Verb conjugation (${tag})`;
        row.appendChild(makeSubcatBtn(label, tag));
      });
    }
  }
}
```

---

## Dropdown HTML

```html
<select id="chapter-select">
  <option value="all">All cards</option>
  <optgroup label="Chapters">
    <option value="Ch 7">Ch 7: In een kledingzaak</option>
    <!-- Ch 8 added here when content is added -->
  </optgroup>
  <optgroup label="Grammar">
    <option value="grammar-practice">Grammar practice</option>
    <option value="grammar-topics">Grammar topics</option>
  </optgroup>
</select>
```

---

## Removed / Replaced

- `value="Modal verbs"` option — replaced by Grammar topics → Modal verbs subcategory
- `value="verbs"` option — replaced by Grammar topics → Verb forms subcategory
- `currentTag` state variable — replaced by `currentChapter` + `currentSubcat` + `contentMode`

---

## Out of Scope

- No changes to the 5 mode tabs or their internal logic
- No changes to card data (`DECK`, conjugations, sentences)
- No changes to stats, weak-words mode, or export/import
- Ch 8 content (vocabulary, verbs) is not part of this spec — the navigation structure accommodates it when content is added
