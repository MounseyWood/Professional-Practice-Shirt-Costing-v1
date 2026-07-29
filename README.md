# Kingston School of Art — Fashion Costing Tools

Self-contained, browser-based costing tools for Fashion students at Kingston School of Art.
Each is a single HTML file with no dependencies and no build step: open it from disk, a USB
stick, GitHub Pages, or a Canvas upload, online or off.

| File | Tool | Level | Garment type |
|---|---|---|---|
| [`index.html`](index.html) | Fashion Costing & Planning Tool v2.2 | MA | Woven / cut-and-sew (shirt) |
| [`knitwear.html`](knitwear.html) | Knitwear Costing & Pricing Tool v1.0 | L4 | Knitted (all three constructions) |

## Knitwear Costing & Pricing Tool

Built for Level 4, and deliberately not a re-skin of the shirt tool — knitwear inverts most
of the assumptions in a woven costing. Yarn is bought by weight rather than length, so
consumption is driven by **grams per garment** taken from panel weights. Yarn count (Nm) is
coupled to machine gauge (GG), and the tool checks that pairing live because it is the most
common mistake in student knitwear specifications. The fabric is created as the garment is
made, so the conversion cost is **knitting machine minutes**, not sewing minutes. And the
choice of construction changes how much yarn is thrown away, from almost nothing to a fifth
of what was bought.

**Eleven sections** run from the brief through construction, yarn and gauge, panels and
knitting, making-up and finishing, trims, factory cost to FOB, freight and import to landed
cost, business model and retail price, circularity and cost per wear, and analysis.

### Responsible design is in the arithmetic, not a questionnaire

Sustainability sections that consist of self-scored dropdowns teach students that
sustainability is a form you fill in afterwards. Here every circularity figure is derived
from decisions made elsewhere in the tool, so the only way to move it is to change the
garment:

- **Cost per wear**, with expected wears estimated from fibre, construction, gauge and
  repairability — and the working printed so it can be argued with.
- **Waste priced in pounds**, per garment and across the whole order.
- **Recyclability derived from the bill of materials**, including the genuine tension that
  elastane extends life in use while blocking fibre-to-fibre recycling at end of life.
- **Longevity provisions as costed line items** — repair allowances, spare yarn, reinforcement
  — that raise the works cost and have to be justified against margin.
- **A circularity index out of 100** computed from mono-material share, waste, repairability,
  disassembly, freight and end-of-life route. There is no box to tick to raise it.

### Retail pricing branches by business model

Everything to landed cost is common; then the same garment at the same cost prices four
different ways, because each model funds different work:

- **Designer-maker** — no factory margin, but own labour must be paid at a real rate.
- **Direct-to-consumer** — a 55–65% *gross* margin visibly eroded by acquisition, returns,
  fulfilment and fees into a much thinner contribution.
- **Wholesale** — wholesale price and retailer multiple, with a live gauge on the
  40–60%-of-retail band a buyer will accept.
- **Volume retail** — intake margin versus **achieved margin** after markdown and sell-through.

### Teaching features

Guided step-by-step walkthrough (20 steps), a full written guide with glossary and
self-check questions, a worked example preloaded so nothing starts blank (7GG lambswool
crew neck, fully fashioned in the UK, 300 units), sanity-check feedback rather than bare
validation errors, a three-way construction comparison, sensitivity testing, named scenarios
for A/B comparison, reflection fields that flow into the output, and a printable report plus
CSV export.

### Saving work

**Download my work (.json)** is the primary route and the only one that survives a shared
studio machine. Quick save uses `localStorage`, which is tied to one browser on one computer.

### Notes for staff

- No CDN or framework, so it cannot break because something external changed.
- Built to WCAG 2.2 AA: proper tab semantics, dialog focus management, keyboard operation
  throughout, labelled controls, no colour-only signalling.
- All default figures sit in a single `REF` object at the top of the first script block, each
  with a comment on where it came from. They are **indicative teaching starting points, not
  price benchmarks**, and students are expected to replace them with real supplier quotations.
- **Guide section 9 lists every default with its basis and how far to trust it** — marked
  *verified*, *anchored*, *well established*, *indicative*, *estimate*, or *our own
  construction* — so students learn to demand provenance rather than just receive it. The
  percentages and structural relationships rest on consistent trade practice; the absolute
  hourly rates are the softest figures, and the tool says so.
- **Guide section 8 gives real UK retail prices** to check an answer against, which is the one
  check the arithmetic cannot do. It doubles as the clearest illustration of the business-model
  point: a UK-made lambswool crew at £87 sold direct is not worse than one at £220 sold through
  shops, it is a different business model.

See [`KNITWEAR-TOOL-PLAN.md`](KNITWEAR-TOOL-PLAN.md) for the design rationale and sources.

---

© Kingston University
