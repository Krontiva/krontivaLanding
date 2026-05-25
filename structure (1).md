# Krontiva Landing — Structure & Change Spec

> **Scope:** Additive changes only. No page redesigns or structural overhauls.
> Three distinct work items: (1) new transformation pages, (2) replace the "Our Solutions" section on `index.html`, (3) lighten the dark colour tokens across all pages.

---

## Current File Tree

```
krontivaLanding/
├── index.html                        ← homepage
├── about/
│   └── index.html
├── contact/
│   └── index.html
├── faq/
│   └── index.html
├── privacy-policy/
│   └── index.html
├── resources/
│   └── index.html
├── terms-of-service/
│   └── index.html
└── vercel.json
```

---

## Target File Tree (after changes)

```
krontivaLanding/
├── index.html                        ← homepage  [EDIT]
├── about/
│   └── index.html                    [EDIT — colour tokens only]
├── contact/
│   └── index.html                    [EDIT — colour tokens only]
├── faq/
│   └── index.html                    [EDIT — colour tokens only]
├── privacy-policy/
│   └── index.html                    [EDIT — colour tokens only]
├── resources/
│   └── index.html                    [EDIT — colour tokens only]
├── terms-of-service/
│   └── index.html                    [EDIT — colour tokens only]
├── customer-transformation/
│   └── index.html                    [NEW]
├── operational-transformation/
│   └── index.html                    [NEW]
├── workforce-transformation/
│   └── index.html                    [NEW]
├── financial-transformation/
│   └── index.html                    [NEW]
└── vercel.json
```

---

## Change 1 — Colour Token Update (all pages)

Every page declares its own `:root` block. Two tokens need updating sitewide.

| Token | Old value | New value | Notes |
|---|---|---|---|
| `--dark` | `#0d1f13` | `#1b5e37` | Near-black → vibrant forest green |
| `--dark-mid` | `#193326` | `#267a4a` | Darkish green → medium green |

**Where these tokens appear in each page:**
- `nav` background reference via logo colour
- Hero section background (`var(--dark)`)
- `who-strip` gradient start colour
- `footer` background
- `cta-strip` gradient start colour
- Stat numbers (`h3` inside `.stat`)
- `.btn-circle` border/colour
- `.news-date .day` colour
- `.faq-item.open .faq-icon` background

Also update the two hardcoded gradient hex values that reference the old dark directly (not via CSS variable). Search each file for `#0d1f13` and `#193326` and replace with the new values.

**Hardcoded values to find and replace (all files):**

| Find | Replace |
|---|---|
| `#0d1f13` | `#1b5e37` |
| `#193326` | `#267a4a` |
| `1e4a10` (appears in `who-strip` and `cta-strip` gradients) | `2d7a52` |
| `1a3d0e` (appears in `cta-strip` gradient) | `267a49` |

---

## Change 2 — "Our Solutions" Section on `index.html`

### What to remove

The existing section between the `<!-- ═══ OUR SOLUTIONS ═══ -->` comment and the `<!-- ═══ STATS ═══ -->` comment — specifically the `.cards-2` grid containing the two product cards (KronGage and KronFlow).

### What to replace it with

Keep the outer `<section class="section">` wrapper and the eyebrow + heading intact. Replace only the `.cards-2` grid with a **4-card transformation grid** that links to the new pages.

```
Section layout (replacing the .cards-2 div):

  .transform-grid  (CSS grid, 2×2, gap 24px, responsive to 1 col on mobile)
  ├── .transform-card  → /customer-transformation/
  ├── .transform-card  → /operational-transformation/
  ├── .transform-card  → /workforce-transformation/
  └── .transform-card  → /financial-transformation/
```

#### Card content spec

Each card contains:
- A small icon (SVG, ~28px, accent-coloured)
- `<h3>` — transformation name
- `<p>` — one-sentence description (from brand source, see below)
- A CTA button using the existing `.btn-pill` class → links to the individual page

| Card | `<h3>` | `<p>` description | Link |
|---|---|---|---|
| Customer Transformation | Customer Transformation | Businesses must shift their focus from products to customers — we help you leverage customer insights to innovate and create human-centric products and experiences that fuel growth. | `/customer-transformation/` |
| Operational Transformation | Operational Transformation | Harness new technology to reimagine your operations strategy, systems, and processes — building resilience and accelerating value across your entire business. | `/operational-transformation/` |
| Workforce Transformation | Workforce Transformation | The power of people, reimagined. We assist organisations in recruiting the right individuals with the necessary skills, at the right location, time, and cost. | `/workforce-transformation/` |
| Financial Transformation | Financial Transformation | Enhance your finance infrastructure to deliver insight, create transparency, and control risk — preparing your company for the future of finance. | `/financial-transformation/` |

#### New CSS to add (inside the `<style>` block in `index.html`)

```css
/* ─── TRANSFORMATION CARDS GRID ─── */
.transform-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
}
@media (max-width: 720px) { .transform-grid { grid-template-columns: 1fr; } }

.transform-card {
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 36px;
  transition: box-shadow .2s, border-color .2s;
  display: flex;
  flex-direction: column;
  gap: 0;
}
.transform-card:hover {
  box-shadow: 0 4px 24px rgba(0,0,0,.07);
  border-color: var(--accent-lt);
}
.transform-card-icon {
  width: 44px;
  height: 44px;
  background: var(--green-bg);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
}
.transform-card-icon svg {
  width: 22px;
  height: 22px;
  fill: var(--accent);
}
.transform-card h3 {
  font-size: 19px;
  font-weight: 800;
  margin-bottom: 12px;
  letter-spacing: -.2px;
}
.transform-card p {
  color: var(--body);
  font-size: 14px;
  line-height: 1.65;
  margin-bottom: 28px;
  flex: 1;
}
```

#### Updated section heading copy

Change `<p class="section-eyebrow">Our Products</p>` to `<p class="section-eyebrow">What We Do</p>`  
Leave `<h2 class="section-title">Our Solutions</h2>` unchanged.

---

## Change 3 — Navigation & Footer Link Updates

### `index.html` nav (and the same nav in all other existing pages)

Add a **"Solutions" dropdown or direct link** to the nav so the transformation pages are reachable. Simplest approach (no JS dropdown needed) — add a single `Solutions` link pointing to the first section anchor on the homepage or to `/customer-transformation/` as the representative entry point. Insert between the existing `Company` and `Resources` nav items:

```html
<li><a href="/customer-transformation/">Solutions</a></li>
```

Or, if a dropdown is preferred later, this can be upgraded without changing the overall nav structure.

### Footer `f-col` for "Solutions"

Replace the current Solutions column links (KronGage, KronFlow) with the four transformation pages:

```html
<div class="f-col">
  <h4>Solutions</h4>
  <ul>
    <li><a href="/customer-transformation/">Customer Transformation</a></li>
    <li><a href="/operational-transformation/">Operational Transformation</a></li>
    <li><a href="/workforce-transformation/">Workforce Transformation</a></li>
    <li><a href="/financial-transformation/">Financial Transformation</a></li>
  </ul>
</div>
```

Apply the same footer update to all four new transformation pages.

---

## Change 4 — New Transformation Pages (spec)

All four pages share the same HTML skeleton as the existing inner pages (e.g. `about/index.html`). Copy `about/index.html` as the starting template — it already has the correct nav, footer, colour tokens, and font import.

### Shared page skeleton

```
nav
hero (page tag + h1 + subtitle) — white background, same as about page
[page-specific content sections]
cta-strip  ← reuse from index.html
footer     ← same footer as index.html, with updated Solutions links
```

---

### Page A — `customer-transformation/index.html`

**`<title>`:** Customer Transformation — Krontiva Africa  
**Hero page-tag:** What We Do  
**Hero h1:** Customer Transformation  
**Hero subtitle:** Businesses must shift their focus from products to customers in order to succeed.

#### Section 1 — Overview

**Heading:** Putting customers at the centre of everything

**Body copy:**

> To succeed in today's market, companies must put the customer at the centre of everything they do and focus on delivering exceptional service and value. The modern business landscape is highly competitive, with companies vying for the attention and loyalty of customers who face more choices than ever before.
>
> By aligning their core values with their customers' beliefs and investing in innovative technologies and capabilities, companies can create a strong gravitational pull that attracts and retains customers over the long term.

#### Section 2 — Two-column pillar cards

**Pillar 1 — Capabilities Attraction**

Heading: Capabilities Attraction

> *"What capabilities allow you to attract and keep your customers happy?"*

Body: In today's fast-paced marketplace, companies that focus solely on developing capabilities for differentiated products risk falling behind. It is essential to understand the underlying factors that drive customer behaviours and preferences — particularly the values that influence decision-making. By bringing those values into the ongoing conversation around customer experience, companies build loyalty and attract new customers. Success requires prioritising both capabilities and values to create a truly differentiated experience.

**Pillar 2 — Value Attraction**

Heading: Value Attraction

> *"What does your company stand for? What actions are you taking to improve the lives of your customers?"*

Body: To attract and retain customers, companies must create an inspiring ethos aligned with their values and act accordingly through investments and behaviours. A clear, compelling vision of what the company stands for — and how customers experience those values — is crucial. Measuring values attraction internally and externally helps organisations achieve digital empathy, future optimism, sustainability, insight, and inclusion, ultimately establishing new social contracts with their communities.

#### Section 3 — Three feature columns (icon + heading + body)

| Icon theme | Heading | Body |
|---|---|---|
| Data / chart | Valuable insight and data | Decades of experience in consumer behavioural modelling gives us extensive knowledge of your customers' behaviours and preferences. We work with you to create a bespoke customer attraction strategy and move quickly from strategy into execution. |
| Pulse / signal | A pulse on the present, an eye on the future | We continuously experiment with cutting-edge technologies — including AI, augmented reality, blockchain, and digital twins — to enhance the customer experience and drive engagement for our clients. |
| Target / aim | Outcome obsessed | We do not just create presentations. We collaborate with you to transform your business into one that can create, attract, and retain customers while driving sustainable outcomes over the long term. |

#### CTA strip (bottom, before footer)

Reuse the existing `.cta-strip` from `index.html` verbatim.

---

### Page B — `operational-transformation/index.html`

**`<title>`:** Operational Transformation — Krontiva Africa  
**Hero page-tag:** What We Do  
**Hero h1:** Operational Transformation  
**Hero subtitle:** Harness new technology to reimagine your business, build resilience, and accelerate value.

#### Section 1 — Overview

**Heading:** Build resilience through operational transformation

**Body copy:**

> Today's operations teams face challenges that go far beyond the traditional goals of improving efficiency and supporting growth. Worker shortages, supply chain disruptions, and the growing demand for environmentally and socially responsible operations have expanded the COO's mandate significantly.
>
> To address these challenges, operations leaders are rethinking their existing systems and processes, exploring new digital capabilities to enhance efficiency, future-proof their organisations, and drive growth. By embracing digital transformation, operations teams can improve their ability to adapt to market changes and respond to the growing demand for sustainable and socially responsible practices.

#### Section 2 — Transformation Process (4-step horizontal or 2×2 grid)

**Heading:** Our Transformation Process — Align, Innovate, Release, Evolve

| Step | Heading | Body |
|---|---|---|
| 01 | Align | Every engagement begins by creating the right goals and objectives — who is involved, what outcomes we are driving toward, and how we'll measure them. We challenge you to form a team with clear objectives that will deliver solutions to complex problems while keeping purpose front and centre. |
| 02 | Innovate | Innovation brings bold ideas to life, iteratively. Within each iteration, teams learn from the past to understand what works and make adjustments. We help you develop a process of continuous growth and learning so ideas can build on each other and drive real transformation. |
| 03 | Release | Every solution benefits from testing. Whether you are on a strategy project or a technology engagement, it is important to test deliverables as you create them. We ensure you are pushing the boundaries of testing while keeping a steady focus on outcomes. |
| 04 | Evolve | Milestones are moments when you have successfully achieved an outcome — and a prompt to look back, reassess goals, and set new ones. We leverage facts and various definitions of value to measure outcomes and determine the next horizon of capabilities for market advantage. |

#### Section 3 — Operations Consulting Services

**Heading:** Operations Consulting Services

**Sub-heading:** Build connectivity and agility into your operations to deliver adaptability and sustainable growth.

Six service tiles (icon + label + one-line description):

| Service | Description |
|---|---|
| Supply Chain | Tailor the supply chain to deliver superior customer performance, cost and asset efficiency, resilience, and sustainability. |
| Procurement | Build a supply base with a highly competitive cost and service profile, and turn procurement into a strategic, commercially-focused capability. |
| Manufacturing | Create competitive advantage by enhancing the global value chain and manufacturing operations within that value chain. |
| Product Development | Accelerate innovation by rethinking the development operating model, processes, and supporting tools. |
| Industry Strategy & Operations | Enhance operations strategy by working on the top issues within each sector to translate operational strategy into execution. |
| Capital Programme | Develop capital asset strategy, execute mission-critical capital projects on time and on budget, and improve asset performance. |

#### Section 4 — Technology Alliance note (single-column callout)

> Our alliance relationships combine powerful technology with Krontiva's industry and business know-how to create leading solutions — with minimal disruption and maximum impact. Our operations consulting team weaves in tax considerations and risk management controls, leveraging our network of innovative alliance partners and industry-leading operations technology vendors.

Three bullet points below the callout:
- Considering taxes can help minimise taxes owed, maximise credits, optimise business structure, and simplify payments.
- Global trade and tariff strategies help you navigate trade uncertainty and improve trade costs and compliance.
- Embedded risk management controls provide early warning systems to lower supplier and operational risk.

---

### Page C — `workforce-transformation/index.html`

**`<title>`:** Workforce Transformation — Krontiva Africa  
**Hero page-tag:** What We Do  
**Hero h1:** Workforce Transformation  
**Hero subtitle:** The power of people, reimagined.

#### Section 1 — Overview

**Heading:** People power your business

**Body copy:**

> Simply having technology is insufficient to bring about a transformation in business and improve outcomes. Achieving success requires individuals with the appropriate skills, supported by the right technology.
>
> The pandemic disrupted the business landscape, making it imperative for corporate leaders to reconsider how they allocate and develop their workforce. Approximately 60% of workers are confident in their ability to thrive in a future workplace and adapt to new technologies — but it is astute managers who ensure their staff receive adequate training and access to the right tools. Empower your employees, and they will enhance your business.

#### Section 2 — Stat row (3 statistics, light-green background)

| Stat | Label |
|---|---|
| 48% | Process changes to lower dependence on institutional knowledge |
| 41% | Strategic planning based on changing business conditions |
| 30% | Major operating model reorganisation |

#### Section 3 — Four service cards (2×2 grid)

**Section heading:** Create a future-ready workforce

| Card | Heading | Body |
|---|---|---|
| 1 | HR Transformation | Is your HR department contributing strategically to the growth and success of your company? Our HR Transformation solution aids your company's current success while readying it for the future — fostering a technologically competent workforce and encouraging originality. |
| 2 | People in the Deal | HR must be an integral part of M&A, not an afterthought. This requires a lean, adaptable HR team that can recruit and retain exceptional talent while providing suitable technology, data, and up-to-date planning to support wise leadership decisions. |
| 3 | Talent Change & Behaviour | The pandemic has led companies to reconsider how they attract and keep talented employees and generate value. Workers' location and manner of working are no longer presumed. Enterprises must support their staff in adjusting to different work styles by nurturing the skills and habits needed for success. |
| 4 | Rewards & Well-being | The key to a motivated and innovative workforce is straightforward: provide employees with the compensation and benefits they deserve. We help your company create a powerful reward programme that attracts and retains top talent while enhancing employee well-being. |

---

### Page D — `financial-transformation/index.html`

**`<title>`:** Financial Transformation — Krontiva Africa  
**Hero page-tag:** What We Do  
**Hero h1:** Financial Transformation  
**Hero subtitle:** Enhance your finance infrastructure to deliver insight, create transparency, and control risk.

#### Section 1 — Overview

**Heading:** The pathway to transform your finance function

**Body copy:**

> The convergence of global events and technological advancements is creating an opportunity for the finance function to shine. To reduce costs and enhance the skill set of finance professionals, organisations need a comprehensive understanding of their requirements and the options available to address them.
>
> Finance Transformation provides a framework to continuously adapt to changing demands and unforeseen challenges. It is time to reevaluate the finance function and strategically design processes that incorporate automation for transactional and reporting tasks, while incorporating advanced analytics to uncover opportunities and deliver lasting value.

#### Section 2 — Three-column journey steps

**Section heading:** An end-to-end journey to sustain your success

| Column | Heading | Body |
|---|---|---|
| 1 | Clarity of Vision | We help you create a vision for your future state and develop your operating model, processes, and technology to achieve it — with continuous learning, building confidence, and delivering value throughout the process. |
| 2 | Transparency & Insight | Advanced technology has made it possible to position your finance function to add more value by delivering insight, transparency, and risk controls cost-effectively. The same data needed for regulatory compliance is highly valuable when applied through new lenses across the organisation. |
| 3 | Sustained Outcomes | Our trusted professionals help you design a strategy for modernising your finance function and stay with you throughout the entire process — designing, building, and refining the solution over time for long-term value. |

#### Section 3 — Four service tiles (2×2 grid)

**Section heading:** Where we can help you along the journey

| Tile | Heading | Body |
|---|---|---|
| 1 | Enterprise Performance Management | Extensive experience helping clients with projects ranging from targeted improvements through full-scale EPM transformations. |
| 2 | Finance Operations | Create a blueprint of your modern finance vision and strategy that will transform your people, processes, and technology — cutting costs and improving operational effectiveness. |
| 3 | Finance Strategy | Explore the possibilities of a modern finance function through interactive, hands-on workshops designed to help your team rapidly innovate and transform. |
| 4 | Transaction Support | Identify divestiture transition costs, post-separation target operating models, functional separation plans, and Day One Readiness plans. |

#### Section 4 — Two product feature cards (side by side)

**Section heading:** Products to help accelerate decision-making

| Card | Product | Body | Bullet points |
|---|---|---|---|
| 1 | Finance Process Intelligence | Gain insights into the operational effectiveness of Finance processes to drive greater efficiencies while improving cost and controls. | • Unify separated data in a centralised hub. • Drive business decisions and quantify cost savings through scenario planning. • Leverage pre-configured dashboards with leading KPIs and metrics. |
| 2 | Cash Intelligence | Improve cash positioning, forecasting, and working capital with real-time visibility of cash flows and agile scenario modelling. | • Connect multiple, disparate data sources into a centralised hub. • Leverage flexible "what-if" capabilities to simulate future cash flow scenarios. • Access dynamic dashboards and pre-built analytics. |

---

## Shared Components for New Pages

### Nav (copy from `about/index.html`, update Solutions link)

```html
<nav>
  <a href="/" class="logo"> … </a>
  <ul class="nav-links">
    <li><a href="/about/">Company</a></li>
    <li><a href="/customer-transformation/">Solutions</a></li>
    <li><a href="/resources/">Resources</a></li>
  </ul>
  <a href="/contact/" class="btn-nav">
    Get Started
    <svg …></svg>
  </a>
</nav>
```

### Footer (copy from `index.html`, update Solutions column)

Replace the footer's Solutions `<ul>` with the four transformation page links as listed in Change 3 above.

### CSS `:root` block (copy from `about/index.html`, apply colour change)

```css
:root {
  --dark:        #1b5e37;   /* was #0d1f13 */
  --dark-mid:    #267a4a;   /* was #193326 */
  --accent:      #3a7230;
  --accent-lt:   #6db84a;
  --green-bg:    #f2f8ef;
  --text:        #111827;
  --body:        #4b5563;
  --muted:       #6b7280;
  --border:      #e5e7eb;
  --white:       #ffffff;
  --radius:      10px;
  --grad:        linear-gradient(135deg, #2a5c1a 0%, #6ebb06 65%, #9ceb32 100%);
  --grad-text:   linear-gradient(135deg, #6ebb06 0%, #9ceb32 100%);
}
```

---

## Implementation Checklist

### Colour update (all existing pages)

- [ ] `index.html` — update `:root` tokens + hardcoded hex values in gradients
- [ ] `about/index.html` — update `:root` tokens
- [ ] `contact/index.html` — update `:root` tokens
- [ ] `faq/index.html` — update `:root` tokens
- [ ] `privacy-policy/index.html` — update `:root` tokens
- [ ] `resources/index.html` — update `:root` tokens
- [ ] `terms-of-service/index.html` — update `:root` tokens

### Homepage "Our Solutions" section

- [ ] Change eyebrow from `Our Products` to `What We Do`
- [ ] Add `.transform-grid` CSS to `<style>` block
- [ ] Replace `.cards-2` grid HTML with four `.transform-card` elements
- [ ] Confirm CTA links resolve to the four new folder URLs

### New pages (create each from `about/index.html` template)

- [ ] `customer-transformation/index.html`
- [ ] `operational-transformation/index.html`
- [ ] `workforce-transformation/index.html`
- [ ] `financial-transformation/index.html`

### Navigation & footer (all pages)

- [ ] Add Solutions nav link in all eight existing pages
- [ ] Update footer Solutions column in all eight existing pages
- [ ] Ensure new pages include updated nav and footer

---

## Notes

- **No JS changes needed.** The FAQ accordion in `index.html` and other interactive elements remain untouched.
- **No new assets.** Use inline SVG icons (matching the style already present in `index.html`) throughout the new pages.
- **URL convention.** All new pages use folder-style routing (`/customer-transformation/`) matching the existing pattern — no changes to `vercel.json` required as Vercel auto-serves `index.html` from subdirectories.
- **Gradient hardcodes.** The `who-strip`, `cta-strip`, and `hero` sections on `index.html` reference raw hex values in addition to CSS variables. Do a global find-and-replace (`#0d1f13` → `#1b5e37`, `#193326` → `#267a4a`) across all files to catch these.
- **Content fidelity.** All transformation page copy is sourced and paraphrased from the canonical Krontiva Africa content at `krontiva.africa`. Keep the brand voice consistent: confident, professional, Africa-forward.
