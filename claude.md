# INSPIRED Book Study — Session Context

## Project Overview
Book study of **"INSPIRED: How to Create Tech Products Customers Love"** by Marty Cagan — **always the 2nd Edition**. Never reference or draw from the 1st Edition.

Two artifacts are maintained and updated part-by-part (with chapters nested within each part):

| Artifact | File | Purpose |
|---|---|---|
| Personal Study Guide | `inspired-study-guide.html` | Interactive web app — Parts as primary nav, chapters accessible within |
| Team Training Deck | `inspired-team-deck.pptx` | Facilitation deck — Part overview slide + Chapter detail slides |
| PPTX Build Script | `/sessions/beautiful-stoic-feynman/pptx-work/build-deck.js` | Node.js script that generates the PPTX |

---

## Edition Rules

- **Always use the 2nd Edition (2017/2018).** Never reference or draw from the 1st Edition (2008).
- The 1st Edition was ~200 pages, ~20 chapters, with a simpler structure. It predates key concepts like Empowered Teams, Feature Factory, the IT Model, and the 5-part structure.
- All content in both artifacts has been audited and verified as 2nd Edition only (as of Part 1).
- When writing summaries, key points, or quotes for future parts: all material must be sourced exclusively from the 2nd Edition.

---

## Parts Completed

- [x] Part 1 — Lessons from Top Tech Companies (Ch. 1–6) ✓ content audited, 2nd Edition verified

---

## Book Structure

| Part | Title | Chapters |
|---|---|---|
| Part 1 | Lessons from Top Tech Companies | Ch. 1–6 |
| Part 2 | The Right People | Ch. 7–20 (approx.) |
| Part 3 | The Right Product | Ch. 21–25 (approx.) |
| Part 4 | The Right Process | Ch. 26–31 (approx.) |
| Part 5 | The Right Culture | Ch. 32–? |

---

## How to Add a New Part

When the user asks to summarize a part or chapter:

1. **Write the summary** in the conversation
2. **Update the HTML** — insert a new part object into `const parts = [...]` in `inspired-study-guide.html`
3. **Update the PPTX** — insert a new part object into `const parts = [...]` in `build-deck.js`, then run `node build-deck.js`
4. **QA the PPTX slides** — render to images via soffice + pdftoppm and visually inspect the Part slide and first Chapter slide
5. **Update CLAUDE.md** — mark the part complete in "Parts Completed"
6. **Deliver** both updated file links

---

## HTML Data Schema

### Part Object (top-level entry in `const parts = [...]`)

```javascript
{
  id: "part1",                  // e.g. "part2"
  number: 1,
  title: "...",
  chaptersRange: "Chapters 1–6",
  subtitle: "...",              // one-line theme/hook
  summary: "...",               // 2-4 sentence paragraph
  keyPoints: [                  // 4-6 bullets (HTML ok, e.g. <em>)
    "...",
  ],
  concepts: [                   // 4-6 core concepts/frameworks
    { emoji: "🎯", name: "Concept Name", desc: "Short description" },
  ],
  quotes: [                     // optional
    { text: "...", attr: "Marty Cagan, INSPIRED Ch. N" }
  ],
  questions: [                  // 4-6 part-level reflection questions
    "...",
  ],
  takeaways: [                  // 4-6 concise takeaways
    "...",
  ],
  chapters: [                   // one entry per chapter in this part
    {
      id: "ch1",
      number: 1,
      title: "...",
      subtitle: "...",          // one-line hook
      brief: "...",             // 1-2 sentence summary shown in chapter list card
      summary: "...",           // full paragraph for chapter detail view
      keyPoints: ["..."],       // 4-5 bullets (HTML ok)
      concepts: [...],          // optional chapter-level concepts
      quotes: [...],            // optional
      questions: ["..."],       // 3-5 chapter-level reflection questions
      takeaways: ["..."]        // 3-5 concise takeaways
    },
    // ... more chapters
  ]
}
```

---

## PPTX Data Schema

### Part Object (top-level entry in `const parts = [...]` in `build-deck.js`)

```javascript
{
  number: 1,
  title: "...",
  chaptersRange: "Ch. 1–6",
  theme: "...",                  // one-line framing (italic subtitle on slide)
  bigIdea: "...",                // most important insight (shown in gold-accented box)
  keyThemes: [                   // exactly 4 themes for left panel
    "...", "...", "...", "..."
  ],
  chapterBriefs: [               // one per chapter — shown as compact chips on right panel
    { number: 1, title: "...", brief: "..." },
  ],
  discussion: [                  // exactly 4 team discussion prompts (2 shown on Part slide)
    "...", "...", "...", "..."
  ],
  callout: "...",                // bold phrase in blue callout box (bottom right)
  calloutLabel: "...",           // small label above callout
  chapters: [                    // one entry per chapter — each generates a Chapter slide
    {
      number: 1,
      title: "...",
      theme: "...",              // italic subtitle
      bigIdea: "...",            // shown in gold-accented box
      keyPoints: [               // exactly 4 for left panel
        "...", "...", "...", "..."
      ],
      discussion: [              // exactly 4 prompts for right panel Q-cards
        "...", "...", "...", "..."
      ],
      callout: "...",
      calloutLabel: "..."
    },
    // ... more chapters
  ]
},
```

---

## PPTX Slide Structure Per Part

For each studied part, two types of slides are generated:

1. **Part Overview Slide** — Big watermark number, title, Big Idea box, 4 Key Themes (left panel) + Chapter list chips, 2 discussion questions, callout (right panel)
2. **Chapter Detail Slides** (one per chapter) — Breadcrumb "Part N › Ch. NN", title, theme, Big Idea, 4 Key Points (left panel) + 4 Discussion Q-cards, callout (right panel)

---

## PPTX Rebuild Commands

```bash
cd /sessions/beautiful-stoic-feynman/pptx-work

# Rebuild the deck
node build-deck.js

# Convert to PDF for QA
python /sessions/beautiful-stoic-feynman/mnt/.skills/skills/pptx/scripts/office/soffice.py \
  --headless --convert-to pdf \
  /sessions/beautiful-stoic-feynman/mnt/Inspired/inspired-team-deck.pptx \
  --outdir /sessions/beautiful-stoic-feynman/pptx-work/

# Render slides to images
rm -f slide-*.jpg
pdftoppm -jpeg -r 150 inspired-team-deck.pdf slide
ls slide-*.jpg
```

---

## File Locations

- Workspace: `/sessions/beautiful-stoic-feynman/mnt/Inspired/`
- PPTX build script: `/sessions/beautiful-stoic-feynman/pptx-work/build-deck.js`
- npm packages: `/sessions/beautiful-stoic-feynman/pptx-work/node_modules/` (pptxgenjs, react, react-icons, sharp)

---

## Design Notes

**Brand: Neurapulse** (neurapulse.com)
- Primary blue: `#2563EB` (blue-600)
- Light blue: `#93C5FD` (blue-300) — used where gold/highlights appear
- Dark bg: `#0F172A` (slate-900)
- Mid navy: `#1E293B` (slate-800)
- Blue-900 panel accent: `#1E3A8A`

**HTML Study Guide** — dark theme, Neurapulse blue/light-blue accents
- CSS vars: `--accent: #2563eb`, `--gold: #93c5fd`, `--teal: #60a5fa`, `--bg: #0f172a`
- Sidebar: Parts as collapsible nav items; chapters as sub-items within each part
- Part view: 5 tabs — Summary, Chapters (with clickable chapter cards), Concepts, Reflection, Takeaways
- Chapter view: 4 tabs — Summary, Concepts, Reflection, Takeaways; breadcrumb + back button to Part
- Progress counter shows `X of 5 Parts`
- Neurapulse SVG logo mark in header

**PPTX Deck** — Neurapulse brand palette (primary `2563EB` blue, `93C5FD` light blue, `0F172A` navy)
- Color constants in `build-deck.js`: `C.coral = "2563EB"`, `C.gold = "93C5FD"`, `C.navydark = "0F172A"`
- Slide 2 card icons use react-icons (FaBookOpen, FaFileAlt, FaComments, FaCheckCircle) — no emoji
- Slide 1: Title (with Parts/Chapters count)
- Slide 2: How to Use This Guide
- Slide 3: About the Book
- Slides 4+: For each Part → Part Overview Slide → Chapter Detail Slides (×N)
