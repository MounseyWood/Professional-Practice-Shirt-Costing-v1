# Knitwear Costing & Pricing Tool — Plan

**Audience:** Level 4 (first-year) undergraduate, Kingston School of Art
**Purpose:** Teach transparent cost build-up from yarn to retail price, with responsible
design and circular-economy thinking built into the numbers rather than bolted on.
**Status:** Plan for discussion. Nothing built yet.

---

## 1. Why this can't be the shirt tool with the words changed

The existing `index.html` (KSA Fashion Costing & Planning Tool v2.2) models a **woven,
cut-and-sew shirt**. Its BOM driver is *metres of fabric × price per metre*, and its
labour model is *cut / sew / finish / button* minutes. Knitwear breaks all of that:

| | Woven shirt (existing tool) | Knitwear (this tool) |
|---|---|---|
| Material bought by | Length (metres) | **Weight (kg)** |
| Consumption driver | Lay plan / marker efficiency | **Grams per garment** (panel weights + yarn wastage) |
| Material constraint | Fabric width, shrinkage | **Yarn count (Nm) must suit machine gauge (GG)** |
| Fabric origin | Bought as finished fabric | **Fabric is created as the garment is made** |
| Primary conversion cost | Sewing minutes | **Knitting machine minutes** + linking |
| Assembly | Sewing | Linking / looping (skilled), or overlock for cut-and-sew |
| Waste | Cutting waste, ~10–15% | **0–20% depending on construction method** |
| Finishing | Press, QC | **Wash / mill / soften / steam / mend** — changes handle, not optional |

The consequence: a knitwear tool needs a **panel-based** cost model (front, back, sleeves,
trims — each with a weight and a knit time), not a fabric-yardage model. That is the core
of the build.

### Design decisions being carried over
Reuse from `index.html`, deliberately:
- The KU CSS custom properties (`--ku-black #1D1D1B`, `--ku-yellow #FFF200`,
  `--ku-blue-dark #002b49`, `--ku-blue-mid #005ea5`, `--ku-blue-light #009cde`,
  `--ku-orange #e35205`) so the two tools read as one family.
- The section-by-section navigation metaphor and the Save / Load / Report affordances.
- The margin/markup vocabulary and the FOB → LCP → Retail spine.

### Things being fixed, not inherited
1. **Tailwind CDN dependency.** The existing tool loads Tailwind from a CDN for "minor
   utility classes only". That breaks in an offline lab, breaks if embedded in Canvas
   behind a strict CSP, and breaks whenever the CDN changes. Write the ~40 lines of CSS
   instead. A teaching tool must work with no network.
2. **localStorage-only persistence.** The existing tool warns "data is stored only in
   *this* browser" — on a shared studio machine that means students lose work. Add
   **JSON file export/import** as the primary save route, localStorage as convenience.
3. **Literal markdown in HTML.** The instructions modal contains `**1. Product Info**`,
   which renders as visible asterisks. Fix in the new tool.
4. **Accessibility.** The existing tool has no ARIA on its tabs or modals, no focus
   management, and inline `onclick`. Kingston is a public sector body — WCAG 2.2 AA is a
   legal expectation, not a nice-to-have. Build to it from the start: real `role="tablist"`,
   focus trap in dialogs, labelled inputs, no colour-only signalling, keyboard-operable
   everywhere.
5. **Level.** The existing tool is pitched at MA ("Sustainable MA Project" is the default
   designer name). L4 needs materially more scaffolding — see §5.

---

## 2. The cost model

### 2.1 Sections

```
0. Guided tutorial (overlay, can be re-entered at any time)
1. Product & Brief          — garment type, market level, order quantity, target price
2. Construction Method      — cut-and-sew / fully fashioned / whole garment
3. Yarn & Gauge             — yarn(s), Nm count, price/kg, gauge, wastage
4. Panels & Knitting        — panel weights (g) and knit times (min)
5. Making-Up & Finishing    — linking, overlocking, mending, wash, press, QC
6. Trims & Packaging        — buttons, zips, labels, care label, bags, hangtags
7. Factory Cost → FOB       — overheads, seconds/rejects, factory margin, commission
8. Freight & Import → LCP   — freight mode, duty, clearance, UK transport
9. Business Model & Pricing — the branch point: designer-maker / DTC / wholesale / volume
10. Circularity & Cost per Wear
11. Analysis                — break-even, sensitivity, scenario comparison
12. Report                  — printable summary + CSV + student rationale
```

### 2.2 Yarn & gauge — the educational constraint

Yarn is priced **per kilogram**. Consumption is **grams per garment**:

```
Gross yarn weight (g) = Net garment weight (g) × (1 + yarn wastage %)
Yarn cost (£)         = Gross yarn weight (g) / 1000 × yarn price (£/kg)
```

Yarn wastage in knitwear is **not** cutting waste — it is set-up ends, thread-ups, tension
runs, mending yarn and knit-down. Industry practice is to add **a minimum of 10%**
([Textile Learning Platform](http://textilelearningplatform.blogspot.com/2016/09/sweater-gauge-counting-yarn-consumption.html)),
before any cut-and-sew cutting waste on top.

**Count / gauge coupling.** `Nm` is the metric count: *the number of 1000 m lengths of yarn
that weigh 1 kg* — so Nm 30 means 30 km per kg, and a **higher Nm is a finer yarn**
([Kniterate](https://support.kniterate.com/hc/en-us/articles/360005872257-Yarn-count-and-converting-yarn-counts)).
Plied yarns are written `2/28Nm` (two ends of Nm 28, giving a resultant Nm 14).

Machine gauge `GG` = needles per inch. A coarse 3GG machine needs a thick yarn; a fine
12GG machine needs a fine one. Feeding the wrong count either jams the machine or knits a
slack, holey fabric. **The tool should encode this as a live sanity check** — pick 12GG and
a 2/8Nm chunky yarn and it warns you, with an explanation. This single interaction teaches
more about knitwear than a paragraph of text, and it is exactly the sort of thing L4
students have no intuition for yet.

Indicative pairing table to ship (student-editable, flagged as indicative):

| Gauge | Typical resultant Nm | Character | Typical garment |
|---|---|---|---|
| 1.5–3 GG | Nm 2–5 | Chunky, hand-knit look | Heavy outerwear knit |
| 5 GG | Nm 5–9 | Chunky-mid | Casual crew, cardigan |
| 7 GG | Nm 9–14 | Mid-weight | Classic lambswool crew |
| 12 GG | Nm 14–28 | Fine | Merino fine-knit, base layers |
| 14–18 GG | Nm 28+ | Very fine | Luxury fine-gauge, jersey-like |

**Yarn prices to seed** (indicative, 2026, flagged as needing verification against the
student's own supplier quotes): cashmere ~£150–200/kg, extra-fine merino and lambswool in
the mid range, wool blends from ~£13/kg, with merino up ~10% year-on-year on drought-reduced
Australian supply ([knitwear.io yarn cost guide](https://knitwear.io/ultimate-guide-yarn-costs-per-kg-2026/)).
The lesson is the **spread**: the same garment shape in cashmere vs. acrylic is a 10×
material cost difference, and that, not the making, is what puts knitwear at its price point.

### 2.3 Construction method — the structural choice

The method selector reconfigures the cost lines and the waste figure:

| | Cut & sew | Fully fashioned | Whole garment |
|---|---|---|---|
| How | Knit fabric by the piece, cut it, sew it | Knit each panel to shape, no cutting | Knit the entire garment in one piece |
| Cutting waste | **15–20%** | ~0% | 0% |
| Knitting time | Lowest | Medium | Highest |
| Assembly labour | Sewing — cheaper, less skilled | **Linking — slow, skilled, expensive** | Minimal, trims only |
| Seams | Bulky | Flat | None |
| Unit cost | **15–25% cheaper** than fully fashioned | Baseline | Machine-dependent |
| Capital cost | Low | Medium | High |
| Typical use | Volume / high street jersey | Mid–luxury classic knitwear | Technical / luxury seamless |

Cost differential and waste behaviour per
[Guoou](https://guooufashion.com/fully-fashioned-vs-cut-and-sew-knitwear-a-comprehensive-comparison/),
[knitwear.io](https://knitwear.io/fully-fashioned-vs-cut-sew-knitwear/) and
[Knit Beat](https://knitbeat.wordpress.com/2018/01/14/cut-and-sew-fully-fashioned-and-whole-garment-manufacturing-for-knitwear/).

**This is where costing and circularity stop being separate topics.** Cut-and-sew is
cheaper per unit *and* throws away a fifth of the yarn. Fully fashioned costs more in
skilled labour *and* wastes almost nothing. Students can see both numbers move at once,
in pounds, and have to argue a position. That is the assessment.

### 2.4 Knitting and making-up

```
Knitting cost   = knit minutes / 60 × machine hour rate (£/hr)
Linking cost    = linking minutes / 60 × linker rate (£/hr)   [or £/garment]
Making-up cost  = mending + wash/mill + press/steam + QC + trim attachment
```

Machine hour rate bundles operator, machine depreciation, power and floor space — worth
saying explicitly, because students otherwise assume it is just wages.

Knit time varies enormously with structure: a plain jersey panel set may knit in around
**15 minutes**, while a heavy cable can take **45–60 minutes per panel**, and coarse-gauge
cabling forces the machine to be slowed to protect needles
([cnsweaters](https://cnsweaters.com/how-cable-knit-sweater-design-affects-cost-and-lead-time/)).
Seeding these as presets by structure (plain / rib / cable / intarsia / jacquard) makes the
design-decision-to-cost link immediate: **choosing a cable is a costing decision**.

Knitwear also always has a **mending / darning** line — dropped stitches are inherent to the
process. Students consistently omit it. It should be a required, pre-populated field.

---

## 3. Circular economy — integrated, not a scorecard

The existing shirt tool has a Sustainability section (§7) of six free-choice 1–5 dropdowns.
The flaw: a student can self-award 5/5 without changing a single design decision, and the
score touches no other number in the tool. It teaches that sustainability is a form you
fill in afterwards.

This tool should do the opposite — **make responsible choices change the money**:

### 3.1 Cost per wear (the headline device)
```
Cost per wear = retail price (inc VAT) / expected wears
```
A £180 fully-fashioned lambswool crew worn 200 times is **£0.90 per wear**. A £25 acrylic
jumper worn 15 times is **£1.67 per wear** — nearly double, for a garment that cost seven
times less. L4 students grasp this instantly, and it reframes durability as an economic
argument they can put to a buyer rather than a moral one. Expected wears should be driven
by the design inputs (fibre, construction, repairability), not typed in freely.

### 3.2 Waste priced in pounds
Show yarn wastage and cutting waste as a **£ per garment** and **£ across the order**, not
just a percentage. "Your cut-and-sew waste is £4.10 a garment — £3,034 across 740 units."
Waste stops being an abstraction at that point.

### 3.3 Recyclability driven by the actual bill of materials
A derived indicator, not a dropdown. Add 3% elastane for recovery and the mono-material
flag drops — surface the genuine tension (elastane extends the *garment's* useful life but
blocks fibre-to-fibre recycling at end of life) rather than pretending there is a clean
answer. Mono-material selection is the single biggest recyclability lever
([Ellen MacArthur Foundation](https://www.ellenmacarthurfoundation.org/topics/fashion/overview),
[Textile School](https://www.textileschool.com/29243/the-circular-economy-in-textiles-redefining-sustainability-in-fashion/)).

### 3.4 Longevity as costed, optional line items
Each of these is a real cost the student must justify against margin:
- Repair / mending allowance
- Spare yarn hank supplied with the garment
- Reinforced elbows / cuffs
- Care labelling and repair instructions
- Take-back or resale provision (% of revenue reserved)
- Extended-durability yarn upgrade

Circular design should appear as **decisions with prices**, so the student learns to make
the business case, which is the skill that survives contact with industry.

### 3.5 Derived circularity index
A 0–100 index computed from: mono-material %, construction waste %, disassembly
(are trims removable?), repairability provisions, freight mode, and end-of-life route.
Because it is derived, it cannot be gamed — the only way to raise it is to change the design.

### 3.6 End-of-life route
Resale / repair / fibre-to-fibre recycling / downcycling / landfill, each with a residual
value line, so students see that designing for recovery retains value.

---

## 4. Costing → retail price by business type

This is the part the brief asks for most explicitly, and the part the existing tool
doesn't do at all. Everything up to **Landed Cost Price** is common; then it branches.

### The common spine
```
Yarn + Trims + Knitting + Linking + Making-up + Finishing   = CMT / works cost
+ factory overhead + seconds/rejects + factory margin + commission = FOB
+ freight + insurance + duty + clearance + UK transport      = LANDED COST (LCP)
```

### The branch — four business models as presets

**A. Designer-maker / self-production**
No factory margin — the student *is* the factory. The critical lesson: **their own labour
must be paid.** L4 students almost universally cost their own time at £0 and then can't
understand why the business doesn't work. Force an hourly rate for own labour and a studio
overhead recovery. Sells DTC only.
```
LCP → + studio overhead recovery + own labour + target margin → RRP inc VAT
```

**B. DTC brand (e-commerce)**
```
LCP → + fulfilment + returns provision + CAC/marketing + payment fees + margin → RRP
```
DTC targets **55–65% margin** vs. wholesale's 30–50% — and the reason matters: the higher
markup funds costs wholesale doesn't have. A wholesale order ships once on net terms with
no acquisition cost and no consumer returns; a DTC order carries CAC, a 25–40% return rate,
free-shipping economics and customer-service load
([In House Made](https://inhousemade.com/blogs/news/clothing-brand-pricing-strategy),
[Shopify](https://www.shopify.com/blog/product-pricing-for-wholesale-and-retail)).
Showing those costs as explicit lines is what stops "DTC has better margins" being
mislearned as "DTC is more profitable".

**C. Wholesale brand (selling to stockists)**
```
LCP → + brand overhead + brand margin → WHOLESALE PRICE
      → retailer markup (keystone 2.0× / keystone-plus 2.2–2.5×) → RRP inc VAT
```
Keystone = retail at 2× wholesale; most modern brands sit at keystone-plus 2.2–2.5×.
The wholesale price needs to land at **40–60% of final retail**: above 60% and the retailer
can't make their margin so they pass; below 40% and the brand is giving away its own
([Wearview](https://www.wearview.co/glossary/keystone-pricing),
[In House Made](https://inhousemade.com/blogs/news/clothing-brand-pricing-strategy)).
The tool should show that band as a live gauge.

**D. High-street / volume retail (own-brand, direct sourcing)**
Introduces the concept students miss most: **intake margin ≠ achieved margin.**
```
LCP → intake margin → RRP → less markdown at X% sell-through → ACHIEVED MARGIN
```
Plan a 65% intake margin, sell 60% at full price and the rest at 40% off, and the achieved
margin is dramatically lower. This is how volume retail actually works and why it drives
volume and low unit cost.

**E. Hybrid (optional 5th)**
Both DTC and wholesale on one product, surfacing the channel-conflict problem: sell DTC at
your own RRP and you compete with your stockists; discount it and you undercut them.

Each preset should reconfigure which cost lines are visible **and** display the benchmark
margin band for that model, with a short "why this model prices this way" explainer. A
cross-model comparison view — same garment, four business types, four retail prices — is
probably the single most valuable screen in the tool.

**Sanity anchor:** COGS should generally be no more than **25–30% of retail** to support
healthy margins in either channel ([Shopify](https://www.shopify.com/blog/product-pricing-for-wholesale-and-retail)).
Flag it when a student's design breaches that.

---

## 5. L4 pedagogy

L4 is first year. They have no industry reference points, no intuition for whether £4.20 of
yarn is a lot, and they will not read a wall of instructions. Design accordingly:

- **Two modes.** *Guided* — one question at a time, next/back, can't get lost, ~20 steps.
  *Full tool* — all sections, for once they know it. Same underlying data.
- **A real tutorial, not a glossary modal.** Walk through one complete garment
  (7GG lambswool crew neck, UK-made, 300 units) with the numbers appearing as they go, and
  a "why does this matter?" note on every input. Re-enterable at any point.
- **Nothing starts blank.** Preload the worked example. A blank costing sheet is paralysing
  at L4; a populated one that they modify is not.
- **Sanity-check feedback, not just validation errors.** "Yarn is 62% of your works cost —
  high even for cashmere. Check your panel weights." Teaching through response is the whole
  point of an interactive tool.
- **Check-your-understanding prompts** at section ends — formative, unmarked, with answers.
- **Reflection fields** that flow into the report. L4 assessment is usually a written
  rationale, so the tool should produce something submittable: "justify your construction
  method", "justify your margin", "what did you trade off?"
- **Glossary as hover/click on every term**, not a separate list to go and find.
- **Scenario A/B comparison.** Save named scenarios and compare side by side —
  cut-and-sew vs fully fashioned, cashmere vs lambswool, DTC vs wholesale. Comparison is
  where the learning happens; a single static answer teaches much less.
- **Export**: print/PDF report + CSV of the full cost build-up.

---

## 6. Technical approach

- **Single self-contained `knitwear.html`.** No build step, no dependencies, no npm.
  Works from GitHub Pages, from a Canvas upload, from a USB stick, offline. This is a hard
  requirement for a studio teaching tool.
- **Vanilla JS**, module-scoped, no framework. One `state` object, one `recalculate()`,
  explicit render functions. Readable by a colleague maintaining it after handover.
- **CSS custom properties** inherited from `index.html` for KU brand consistency.
- **No CDN.** See §1.
- **Persistence:** JSON export/import (primary) + localStorage (convenience) +
  named scenarios.
- **WCAG 2.2 AA** — real tablist semantics, dialog focus management, visible focus,
  labelled inputs, no colour-only signalling, keyboard-operable throughout.
- **Print stylesheet** so the report prints cleanly to PDF for submission.
- **All default figures sourced and flagged as indicative**, in one editable
  `defaults` object at the top of the script, with a comment citing where each came from.
  A costing tool with unsourced magic numbers teaches students to trust unsourced magic
  numbers.

### Repo placement
Add `knitwear.html` alongside the existing `index.html`, leaving the shirt tool untouched
at its current URL so any existing links from Canvas or handbooks keep working. Cross-link
the two. A landing hub page can come later if a third tool appears.

---

## 7. Open decisions

1. **Construction methods** — all three (cut-and-sew, fully fashioned, whole garment), or
   focus on fewer? *Recommendation: all three.* Comparing them is the circular-economy
   lesson, and the marginal build cost is small once the panel model exists.
2. **Business types** — the four above (designer-maker, DTC, wholesale, volume retail),
   plus hybrid? *Recommendation: all four, hybrid if time allows.*
3. **Sourcing** — UK/near-shore making, overseas FOB with the full import chain, or a
   selector? *Recommendation: a selector.* UK-made suits the responsible-design agenda and
   is what many KSA graduates actually do; overseas teaches duty, freight and Incoterms.
   Both are on the curriculum.
4. **Currency** — GBP throughout, or dual-currency like the shirt tool (USD factory / GBP
   landed)? *Recommendation: GBP primary with an optional yarn-purchase currency and
   exchange rate*, since yarn is frequently bought in EUR from Italian spinners. Simpler
   than the shirt tool's USD/GBP split, which conflates factory currency with yarn currency.
5. **Assessment integration** — should the report output map onto a specific KSA assignment
   brief or marking rubric? If there's a brief, the report should be shaped to it.

---

## 8. Sources

- [Textile Learner — consumption and costing for fully fashioned knitwear](https://textilelearner.net/consumption-and-costing-for-fully-fashioned-knitwear/)
- [Textile Learning Platform — sweater gauge counting & yarn consumption](http://textilelearningplatform.blogspot.com/2016/09/sweater-gauge-counting-yarn-consumption.html)
- [Kniterate — yarn count and converting yarn counts](https://support.kniterate.com/hc/en-us/articles/360005872257-Yarn-count-and-converting-yarn-counts)
- [Guoou — fully fashioned vs cut-and-sew knitwear](https://guooufashion.com/fully-fashioned-vs-cut-and-sew-knitwear-a-comprehensive-comparison/)
- [knitwear.io — fully fashioned vs cut & sew](https://knitwear.io/fully-fashioned-vs-cut-sew-knitwear/)
- [Knit Beat — cut and sew, fully fashioned and whole garment manufacturing](https://knitbeat.wordpress.com/2018/01/14/cut-and-sew-fully-fashioned-and-whole-garment-manufacturing-for-knitwear/)
- [knitwear.io — yarn cost guide 2026, prices per kg](https://knitwear.io/ultimate-guide-yarn-costs-per-kg-2026/)
- [cnsweaters — how cable knit design affects cost and lead time](https://cnsweaters.com/how-cable-knit-sweater-design-affects-cost-and-lead-time/)
- [Shopify — how to calculate wholesale product pricing](https://www.shopify.com/blog/product-pricing-for-wholesale-and-retail)
- [Wearview — keystone pricing in fashion retail](https://www.wearview.co/glossary/keystone-pricing)
- [In House Made — pricing for wholesale, DTC & retail](https://inhousemade.com/blogs/news/clothing-brand-pricing-strategy)
- [Ellen MacArthur Foundation — circular fashion overview](https://www.ellenmacarthurfoundation.org/topics/fashion/overview)
- [Textile School — circular economy in textiles](https://www.textileschool.com/29243/the-circular-economy-in-textiles-redefining-sustainability-in-fashion/)

*All indicative figures are seed defaults for students to override with their own supplier
quotes. They are teaching starting points, not price benchmarks.*
