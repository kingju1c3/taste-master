# Taste Master v3

**v3.0 - 2026-09-09. Supersedes taste-master v2 (2026-08-24).**

Synthesized from four sources read in full and rated this session: Anthropic `frontend-design` v1.1.0 (judgment process, subject-matter grounding, hero-as-thesis, structure-is-information, copywriting), `anti-slop-frontend` (dials, density bands, fix-action checklist format, trigger-phrase description), Leonxlnx `taste-skill` v2 experimental at commit `ccbc156` (design-system map, motion engineering, redesign protocol, palette bans, mechanical checks, pattern vocabulary), and taste-master v2 (the skeleton, brief supremacy, distilled pre-flight).

**What v3 adds over v2:** a named pattern vocabulary (Section 11) so the skill expands the solution space instead of only restricting it; inlined motion skeletons so no rule points at an external repo; one assertion per pre-flight box so failures are attributable; a full interactive-state cycle; and a rotation log that makes the "differ from your last project" rules actually enforceable.

Rules below are strong defaults, not law.

**BRIEF SUPREMACY - the one meta-rule.** The user's explicit words override every rule in this file, including every ban. If the brief asks for Inter, three cards, beige-and-brass, or an em-dash aesthetic, the brief wins, silently. Rules only govern the axes the brief leaves free.

Approach every build as the design lead at a small studio known for giving every client a visual identity that could not be mistaken for anyone else's. Make deliberate, opinionated choices specific to this brief, and take one real aesthetic risk you can justify. Not taking a risk is itself a risk.

---

## 0. Read the brief (judgment gate - before any rule fires)

Extract before designing:

1. **Page kind** - landing (SaaS / consumer / agency / event), portfolio (dev / designer / studio), redesign (preserve vs overhaul), editorial.
2. **Vibe words** - "minimal", "Linear-style", "Awwwards", "premium", "playful", "brutalist", "serious B2B", "glassy", "Apple-y".
3. **References** - URLs, screenshots, named competitors, existing brand assets. For redesigns these are material, not suggestions.
4. **Audience** - the audience picks the aesthetic, not your taste.
5. **Quiet constraints** - accessibility-first, public-sector, regulated, kids. These OVERRIDE aesthetic preference.

If the brief doesn't pin the subject, pin it yourself: name one concrete subject, its audience, and the page's single job. The subject's own world - its materials, instruments, vernacular - is where distinctive choices come from. A toy site for girls aged 8 to 11 and a dashboard for financial analysts should share almost nothing. Build with the brief's real content throughout.

**Declare a one-line Design Read before code:** "Reading this as: `<page kind>` for `<audience>`, `<vibe>` language, leaning `<aesthetic family / design system>`."

Ambiguous brief: ask exactly ONE question, and only if the read genuinely diverges. Otherwise declare and proceed.

---

## 1. Route: is this the right skill? (and the design-system map)

This skill improves marketing-grade surfaces. It will NOT improve, and must not be force-applied to:

| Surface | Route to instead |
|---|---|
| Dashboards / admin panels | Fluent, Carbon, Atlassian, Polaris |
| Data tables | TanStack Table, AG Grid |
| Multi-step forms / wizards | form-flow patterns |
| Code editors | Monaco / CodeMirror official theming |
| Native mobile | Apple HIG / Material directly |
| Realtime collab UI | different problem class |

If the brief is one of these: say so plainly, name the better tool, apply only the parts that transfer.

**When the brief reads as a real design system, use the OFFICIAL package.** Never recreate its CSS by hand; never import its tokens then override 90 percent.

| Brief reads as | Reach for | Install |
|---|---|---|
| Microsoft / enterprise SaaS | Fluent UI React v9 | `npm i @fluentui/react-components` |
| Google-ish / Material product | Material Web + M3 tokens | `npm i @material/web` |
| IBM-style enterprise analytics | Carbon | `npm i @carbon/react` |
| Shopify app surfaces | Polaris (required for Shopify admin) | Polaris web components via Shopify CDN |
| GitHub-style devtool | Primer | `npm i @primer/css` or `@primer/react-brand` |
| UK public sector | GOV.UK Frontend | `npm i govuk-frontend` |
| US public sector | USWDS | `npm i @uswds/uswds` |
| Modern SaaS, owned components | shadcn/ui | `npx shadcn@latest init` - never ship default state |
| Indie / small-team default | Tailwind v4 utilities | `npm i tailwindcss@latest` |

Verify the package actually resolves before importing it. If you cannot verify, print the install command and say so rather than assuming.

**One system per project.** Never Material plus shadcn mixed. When the brief is an *aesthetic* (glassmorphism, bento, brutalism, editorial, dark tech, aurora, kinetic type) there is no official package: build with native CSS plus Tailwind and label borrowed inspiration honestly. Apple Liquid Glass on the web is always an approximation - say so in a comment.

---

## 2. The three dials

Set after the design read. Every later decision is gated by them.

- **VARIANCE** (1 = perfect symmetry ... 10 = asymmetric / editorial / off-grid)
- **MOTION** (1 = hover-only static ... 10 = cinematic, scroll-choreographed)
- **DENSITY** (1 = gallery-airy ... 10 = cockpit-packed)

**Baseline when the brief gives no direction: 7 / 5 / 4.** Infer, don't ask.

| Signal | V | M | D |
|---|---|---|---|
| minimal / calm / editorial / Linear-style | 5 | 3 | 3 |
| premium consumer / luxury / Apple-y | 7 | 6 | 3 |
| playful / Awwwards / agency / experimental | 9 | 8 | 3 |
| trust-first / public-sector / regulated | 3 | 2 | 5 |
| developer portfolio | 6 | 5 | 4 |
| redesign - preserve | match existing | +1 | match |
| redesign - overhaul | +2 | +2 | match |

**Density bands:** 1-3 gallery (huge negative space, `py-32`+ section gaps, few elements per view) · 4-7 daily app (`py-16` to `py-24`) · 8-10 cockpit (tight padding, hairline `1px` dividers instead of cards, `font-mono` numbers). A brief landing in 8-10 is usually a signal you should be routing to a real design system instead.

**State your three values in one line at the top of the response** so the choice is visible and correctable.

---

## 3. Plan, critique, then build (two-pass discipline)

**Pass 1 - plan a compact token system before any markup:**

- **Color:** 4 to 6 named hex values. One dominant, near-black foreground, toned background, 1 to 2 rare sharp accents.
- **Type:** 2+ roles. One characterful display face used with restraint, one quiet legible body face.
- **Layout:** one-sentence concept, plus alignment guidance (left, centered, justified). ASCII wireframes are useful for comparing two options against each other.
- **Signature:** the ONE element this page is remembered by. Spend your boldness here; keep everything around it quiet.

**Pass 2 - genericity critique before building.** Ask: "would I produce this same plan for any similar brief?" Work through a neighboring prompt mentally and see whether you land in the same place. Check against the known AI-design clusters:

1. Warm cream background near `#F4F1EA`, high-contrast serif display, terracotta or warm-clay accent near `#D97757`. That accent is Anthropic's own interaction color, so on a user's brief it reads as a tell.
2. Near-black background with a single bright acid-green or vermilion accent.
3. Broadsheet layout: hairline rules, zero border-radius, dense newspaper columns.
4. The SaaS-card kit: content chopped into identical rounded cards, one radius on everything regardless of hierarchy, the same soft grey `rgba(0,0,0,.1)` shadow under each, gradient washes as decoration.
5. Template chrome that appears whatever the subject: tracked-out ALL-CAPS eyebrow above every heading, meta strings joined by middle dots, `WORD - fragment` labels, tinted near-black standing in for black, a mono face for small data labels, a `→` appended to every link and button.

All five are legitimate when *chosen*; none is legitimate as a *default*. If any part of the plan is a default, revise it and note what changed and why. Only then write code, following the revised plan exactly and deriving every color and type decision from it.

Do the planning in thinking. Show the user the result, not the deliberation.

---

## 4. Typography

- Display face with character: Geist, Satoshi, Cabinet Grotesk, Outfit, General Sans, Clash Display. Pairings worth knowing: Geist + Geist Mono, Satoshi + JetBrains Mono, Cabinet Grotesk + Inter Tight.
- One family or two. If two, make them clearly distinct - a near-identical pair reads as an accident.
- **Serif discipline.** Serif is discouraged as a default reach; "creative brief equals serif" is a tested AI tell. Justified only when the brief names one, or the direction is genuinely editorial / luxury / publication / heritage AND you can say why this serif fits this brand. Fraunces and Instrument Serif are the two LLM-favorite display serifs - treat them as tells unless briefed. When a serif IS justified, rotate between projects (Section 15); never ride one.
- **Emphasis rule.** To emphasize a word inside a headline, use italic or bold of the SAME family. Never inject a serif word into a sans headline for visual interest. Better still, avoid single-word accenting entirely - it is one of the commonest generated-page tells, alongside all-caps labels and unnecessary typographic labels above content.
- Real scale, not flat: display `clamp(2.5rem, 5vw, 5rem)`, body around `1.0625rem`, dramatic jump between.
- **A 4-line hero headline is a font-size error, never a copy-length error.** Sensible hero default `text-4xl md:text-5xl lg:text-6xl`; reserve `text-7xl` for 3 to 5 word headlines. Plan headline size and hero asset size together.
- Line length under 80 characters. Serif body text gets slightly longer measure and slightly more line-height than sans.
- Tracking `-0.02em` on large display; body line-height around 1.6. Italic descenders (`y g j p q`) get line-height at least 1.1 plus bottom padding reserve. Audit every italic display word before shipping.

## 5. Color & theme

- Token system (CSS vars or Tailwind theme), never ad-hoc hex. Everything references tokens.
- Near-black (`#0A0A0B` class) on toned off-white. Never pure `#000` on `#FFF`.
- Max 1 accent, saturation under 80 percent by default. Neutral base (zinc / slate / stone) plus one high-contrast accent. AI-purple glow is a tell unless briefed.
- **Premium-consumer palette rotation.** For cookware / wellness / artisan / luxury / DTC briefs the LLM default is warm beige-cream plus brass/clay/oxblood plus espresso text. Banned as the automatic reach - every AI premium site uses it and the brand disappears. Rotate real alternatives: cold luxury (silver / chrome / smoke), forest (deep green + bone + amber), black-and-tan, cobalt + cream, terracotta + slate, monochrome + one saturated pop. Check the rotation log (Section 15) before choosing. Brief naming those colors overrides, as always.
- **Theme lock.** ONE theme (light, dark, or auto) for the whole page. No section flips mid-page. Same-family background tints are fine (`bg-zinc-950` next to `bg-zinc-900`); `bg-amber-50` mid-dark-page is broken.
- **Consistency locks.** One accent used identically across all sections. One corner-radius system page-wide; mixed radii only under a documented rule followed everywhere. One design system per project.
- Dark mode is a design, not an inversion. Define tokens per mode, pick ONE strategy (`dark:` variants OR CSS variables, not both), keep hierarchy parity and brand fidelity, and open the page in both modes before declaring done.

## 6. Layout

- CSS Grid over flex-percentage math. Contain in `max-w-7xl mx-auto` plus consistent padding; full-bleed is a per-section choice.
- **`min-h-[100dvh]`, never `h-screen`** (iOS toolbar jump).
- **Hero is a thesis.** Open with the most characteristic thing in the subject's world - headline, image, animation, live demo, interactive moment. Be deliberate: big-number-with-label plus stats plus gradient accent is the default treatment, so use it only if it is genuinely best. Discipline: at most 4 text elements (eyebrow OR brand strip, headline at most 2 lines, subtext at most 20 words, CTA group of 1 primary plus at most 1 secondary); top padding at most `pt-24` desktop; CTA visible without scrolling. **Banned inside the hero:** tiny tagline below CTAs, trust micro-strip, pricing teaser, feature bullets, avatar rows. All move to sections below. "Trusted by" logo walls live UNDER the hero, never in it. More presence means bigger font or asset, never more padding. Text plus a gradient blob is not a hero.
- **Structure is information.** Numbering, eyebrows, dividers, and labels must encode something true about the content, not decorate it. Numbered markers (01 / 02 / 03) only when the content genuinely is a sequence whose order the reader needs. Check the content really is a sequence before adding them.
- Vary section rhythm. No uniform heights or gaps. No 3+ consecutive sections sharing the same split layout. Each layout family appears at most once; aim for 4+ families across roughly 8 sections.
- **Split-header ban.** "Left big headline plus right small floating explainer" as a section header is a default tell. Stack vertically (headline, then body at `max-w-[65ch]`) unless the right column carries a real visual or interactive element.
- **Bento cell count equals content count.** 3 items, 3 cells; 5 items, 5 cells. An empty filler tile means the grid shape is wrong - reshape it. At least 2 to 3 cells in any multi-cell grid need real visual variation (image, brand-appropriate gradient, pattern). All-white text cards read as default.
- **Navigation:** one line at desktop, height at most 80px (default 64 to 72). Two-line desktop nav is broken.
- Icons: one real set (Phosphor, Lucide, Radix, Tabler), one stroke width. Never emoji-as-icons, never hand-rolled SVG paths.
- Mobile is mandatory. Every wide or asymmetric block collapses to clean single column below 768px, declared per section. Horizontal scroll on mobile is a failure.

## 7. Motion (banded by the MOTION dial)

- **1-3:** hover / active / focus transitions only. Calm reads expensive.
- **4-7:** transform and opacity only, easing like `cubic-bezier(0.16,1,0.3,1)`, staggered load-in delays.
- **8-10:** scroll choreography via a real library (Motion, GSAP ScrollTrigger) or `IntersectionObserver`.

**Engineering floor for motion.** Continuous input-driven values (mouse position, scroll progress, magnetic hover) use Motion's `useMotionValue` / `useTransform` / `useScroll`. NEVER `useState` - it re-renders the tree every frame and collapses on mobile. Motion lives in isolated `'use client'` leaf components with `useEffect` cleanup; Server Components render static layout only. Never mix GSAP or Three.js with Motion in the same component tree; they fight over the same frames. Never attach a raw `window.addEventListener('scroll')` for animation.

**Canonical skeletons (inlined - this skill has no external dependencies).**

Scroll-reveal stagger, the lightest correct pattern and the right default for MOTION 4-7:

```jsx
'use client'
import { motion } from 'motion/react'

const stagger = { hidden: {}, show: { transition: { staggerChildren: 0.08 } } }
const item = {
  hidden: { opacity: 0, y: 16 },
  show: { opacity: 1, y: 0, transition: { duration: 0.6, ease: [0.16, 1, 0.3, 1] } },
}

export function Reveal({ children }) {
  return (
    <motion.div
      variants={stagger}
      initial="hidden"
      whileInView="show"
      viewport={{ once: true, margin: '-10% 0px' }}
    >
      {Array.isArray(children)
        ? children.map((c, i) => <motion.div key={i} variants={item}>{c}</motion.div>)
        : <motion.div variants={item}>{children}</motion.div>}
    </motion.div>
  )
}
```

Sticky-stack and horizontal-pan, MOTION 8-10 only. The classic failure is pinning at `top center`, which fires mid-scroll:

```jsx
'use client'
import { useEffect, useRef } from 'react'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
gsap.registerPlugin(ScrollTrigger)

export function HorizontalPan({ children }) {
  const wrap = useRef(null)
  const track = useRef(null)

  useEffect(() => {
    if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) return
    const ctx = gsap.context(() => {
      const distance = track.current.scrollWidth - window.innerWidth
      gsap.to(track.current, {
        x: -distance,
        ease: 'none',
        scrollTrigger: {
          trigger: wrap.current,
          start: 'top top',        // never 'top center'
          end: () => `+=${distance}`,
          pin: true,
          scrub: 1,
          invalidateOnRefresh: true,
        },
      })
    }, wrap)
    return () => ctx.revert()   // cleanup is not optional
  }, [])

  return (
    <section ref={wrap} className="overflow-hidden">
      <div ref={track} className="flex w-max">{children}</div>
    </section>
  )
}
```

- Motion claimed equals motion shown. If MOTION is above 4, the page actually animates. If you cannot ship working motion in scope, drop the dial to 3 and ship clean static. Never half-built motion.
- Non-user-triggered motion is used sparingly and only to draw attention. One orchestrated moment beats scattered effects. Fade-and-slide-up on every section plus hover transitions on every card is the generic default and reads as AI-generated. Motion that answers a person's action (opening, expanding, confirming) is welcome because it shows what changed.
- Every animation justified in one sentence (hierarchy, feedback, storytelling, state transition) or cut. Marquees: max one per page. Sometimes none is the strongest choice; extra animation is itself a tell.
- `prefers-reduced-motion` respected for everything above MOTION 3. Infinite loops, parallax, scroll-hijack, and magnetic physics all collapse to static under it.

## 8. Images & assets

Landing pages are visual products. Text-only pages with fake-screenshot divs are slop. Priority order:

1. **Image-generation tool first** if any is available. Section-specific assets at the right aspect ratio.
2. **Real images second:** `https://picsum.photos/seed/{descriptive-seed}/{w}/{h}` for placeholders, or actual brand/stock URLs the brief provides.
3. **Last resort:** clearly-labeled placeholder slots (`<!-- TODO: hero product photo, 1600x1200 -->`) plus a closing line listing what is needed. Never hand-rolled SVG illustrations or div-built fake product UI as filler.

Even minimalist sites need 2 to 3 real images; pure-text is incomplete work, not minimalism. Logo walls use real SVG logos (`https://cdn.simpleicons.org/{slug}` or devicon) or, for invented brands, a simple generated monogram mark. Never plain text wordmarks, and logos only, no industry labels beneath. Div-based fake screenshots, fake terminals, and fake dashboards are banned: real screenshot, generated image, real component preview, or nothing.

## 9. Copy & content

Words are design material, not decoration. Before writing anything, ask what the design needs to say and how it best helps the person navigate.

- Write from the user's side of the screen. Name what people control ("notifications"), never how it's built ("webhook config"). Describe what something does in plain terms rather than selling it. Specific and legible beats clever.
- Active voice. Controls say what happens ("Save changes", not "Submit"). An action keeps its name through the flow: the button that says "Publish" produces a toast that says "Published." One job per element - a label labels, an example demonstrates, nothing quietly does double duty.
- Errors explain what went wrong and how to fix it, in the interface's voice. Never apologetic, never vague. Empty states invite action.
- Concrete verbs only. Banned filler: "Elevate", "Seamless", "Unleash", "Next-Gen", "Revolutionize".
- Realistic data. No "John Doe" / "Acme" / "SmartFlow" / `99.99%` / `1234567`. Locale-appropriate names, believable brands, organic numbers (`47.2%`). Fake-precise specs (`5.8 mm`, `4.1x`) come from real data, are labeled mock, or don't ship.
- Default section shape: headline at most 8 words, sub at most 25 words, one visual or CTA. Long lists (more than 5 items) get a real component - grouped chunks, card grid, tabs, scroll-snap pills, carousel. Never a default `<ul>` with a hairline under every row. Quotes at most 3 lines with clean attribution (name plus role).
- Em-dash rationed: zero in UI chrome (headlines, labels, buttons, eyebrows, pills, captions); avoid in body copy. Restructure with periods, commas, colons, parentheses. The brief's editorial voice can override.
- One copy register per page. **Copy self-audit before shipping:** re-read every visible string. Anything grammatically broken, unclear-referent, or LLM-cute ("free on its past") gets rewritten plain. AI-cute copy is worse than boring copy.

---

## 10. Interactive states (the full cycle, not just success)

LLMs default to "static successful state only." Ship all of it:

- **Loading:** skeleton loaders matching the final layout's shape. Not generic circular spinners.
- **Empty:** composed deliberately, and it says how to populate itself.
- **Error:** inline for forms, contextual for actions. Toasts only for transient confirmations.
- **Tactile feedback:** `:active` gets `scale-[0.98]` or `-translate-y-[1px]` to simulate a physical push.
- **Focus:** visible keyboard focus on every interactive element. Never `outline: none` without a replacement.
- **Button contrast (mandatory, a11y):** every CTA's text is readable against its own background. WCAG AA, 4.5:1 for body text and 3:1 for large text at 18px+. White-on-white, `bg-white` with `text-white`, and transparent buttons over the page background with no border are all banned. Ghost buttons over photographs need a scrim, backdrop, or stroke.
- **CTA wrap ban (mandatory):** button text fits on one line at desktop. Fix by shortening the label (3 words max for primary CTAs, ideally 1 to 2) or widening the button. Do not artificially constrain `max-width` on CTAs.
- **No duplicate CTA intent (mandatory):** "Get in touch" plus "Let's talk" plus "Start a project" are one intent. Pick ONE label and use it everywhere - nav, hero, footer. Same for "Try free" / "Get started" / "Sign up free", and for "View work" / "See selected work" / "Browse projects".
- **Form contrast (mandatory, a11y):** inputs, placeholder text, focus rings, helper text, and error text all pass AA against the section background.
- **Forms:** label above input, helper text present in markup, error below. Never placeholder-as-label.

---

## 11. Pattern vocabulary (expand the space, don't just restrict it)

Every other section of this file tells you what *not* to do. This one gives you somewhere to go. Know these names so you can reach for them deliberately when the design read calls for one, and so you can describe what you built. This is a vocabulary, not a mandate; most pages need one or two of these, not ten.

**Hero paradigms** - Asymmetric Split (text one side, asset the other) · Editorial Manifesto (large type, no asset, near-poster) · Media Mask (type cut as mask over video) · Kinetic Type (animated typography as the primary visual) · Curtain Reveal (hero parts on scroll) · Scroll-Pinned (hero holds while content scrolls behind).

**Navigation** - Dock Magnification · Magnetic Button · Gooey Menu · Dynamic Island (morphing status pill) · Contextual Radial Menu · Floating Speed Dial · Mega Menu Reveal.

**Layout and grids** - Bento Grid · Masonry · Chroma Grid (tiles with subtly animating gradient borders) · Split-Screen Scroll (halves moving opposite directions) · Sticky-Stack Sections.

**Cards and containers** - Parallax Tilt Card · Spotlight Border Card (border illuminates under cursor) · Glassmorphism Panel · Holographic Foil Card · Swipe Stack · Morphing Modal (button expands into its own dialog).

**Scroll behavior** - Sticky Scroll Stack · Horizontal Scroll Hijack · Sequence Scroll (video or 3D tied to scrollbar) · Zoom Parallax · Scroll Progress Path (SVG line drawing) · Liquid Swipe Transition.

**Galleries and media** - Dome Gallery · Coverflow Carousel · Drag-to-Pan Grid · Accordion Image Slider · Hover Image Trail · Glitch Shift.

**Typography as motion** - Kinetic Marquee (bands reversing on scroll) · Text Mask Reveal · Text Scramble · Circular Text Path · Gradient Stroke Animation · Kinetic Type Grid.

**Micro-interactions** - Particle Burst Button · Skeleton Shimmer · Directional Hover-Aware Button (fill enters from the cursor's actual side) · Ripple Click · Animated SVG Line Drawing · Mesh Gradient Background · Lens Blur Depth.

**Library choice** - Motion (`motion/react`) for UI and state-change motion. GSAP plus ScrollTrigger for full-page scrolltelling and hijacks. Three.js / WebGL for canvas backgrounds and 3D scenes. Isolate each in dedicated leaf components with cleanup; never mix them in one tree.

Anything from this list that lands above MOTION 7 still obeys Section 7's engineering floor and reduced-motion rule. A named pattern is not an exemption.

---

## 12. AI-tells (banned as defaults; the brief can override any)

**Hero and labels** - version badges (`BETA`, `v2.0`, `EARLY ACCESS`) unless the brief is a launch · "Brand · No. 01" micro-meta · decoration strips (`DESIGN. BUILD. SHIP.`) · scroll cues ("Scroll to explore") · locale/weather strips ("LIS 14:23 · 18°C") unless the brief is place-centric.

**Section furniture** - numbered eyebrows (`001 · Capabilities`): eyebrows name topics in plain language, budget at most `ceil(sections ÷ 3)` per page, and if one section has an eyebrow the next two don't · generic step labels ("Step 1", "Phase 02"): the verb-noun is the label ("Install", "Configure", "Ship") · poetic section labels ("From the field", "On our desks"): use plain ones · micro-meta sentences under headings · `01 / 4` pagination on images or tiles (if the user can count, skip the label) · middle-dot rationed to one per metadata line.

**Visual clichés** - purple-to-blue gradient hero on white · three identical rounded cards as the only features idea · glassmorphism on everything · neon outer glows · custom cursors · decorative colored status dots (semantic state only, sparingly) · crosshair or hairline grids as pure decoration · filled-track progress bars as comparison visuals · `border-t` plus `border-b` on every row of long lists · vertical rotated text · `<br>`-broken italicized headlines as a default move · one soft grey shadow under everything regardless of hierarchy.

**Fakery** - div-built fake product screenshots · fake version footers (`v1.4.2 · last sync 4s ago`) on marketing pages · pills or tags overlaid on photos · photo-credit captions as decoration (`Field study no. 12 · Ines Caetano`) · "Quietly trusted by" (say "Trusted by" or let the logos speak) · fake-scarce counters ("Reservation 412 of 800") without real data.

---

## 13. Engineering floor (quality without announcing it)

- Verify every import and library actually exists in the project (`package.json`) before using it. If unconfirmed, output the install command or state the assumption and prefer dependency-free. Hallucinated imports are the fastest way to break a build.
- Default stack when unspecified: React plus Tailwind, Server Components with client islands only where motion needs them. Fonts via `next/font` or self-hosted `@font-face` with `font-display: swap`. Never a Google Fonts `<link>` in production. The user's named stack always wins.
- WCAG AA contrast on every CTA and form element against its actual background. Visible keyboard focus. No CTA label wrapping at desktop. One label per CTA intent.
- Animate only `transform` and `opacity`. `will-change` sparingly. Grain and noise filters only on fixed `pointer-events-none` overlays, never on scrolling containers - continuous GPU repaints destroy mobile framerate.
- Z-index is systemic, not sprinkled. Reserve it for real layer contexts (sticky nav, modal, overlay, grain) and document the scale in a constants file. Never spam arbitrary `z-50`.
- Bundle awareness: Motion is not tiny, Three.js is large. Lazy-load anything below the fold.
- Core Web Vitals plausible: LCP under 2.5s (hero image prioritized and preloaded), INP under 200ms, CLS under 0.1.
- Watch CSS specificity collisions between type-based and element-based selectors (`.section` vs `.cta`). A classic silent padding/margin canceller.

---

## 14. Redesigns: audit first, change nothing silently

1. **Detect the mode:** greenfield · preserve (modernize without breaking the brand) · overhaul (new visuals, same content and IA). If ambiguous, ask once. Misclassifying the mode is the single biggest source of bad redesign output.
2. **Audit before touching:** brand tokens, information architecture, content blocks, patterns to preserve vs retire, a dial reading of the existing site (that is the starting point, not the baseline), and the SEO baseline. SEO migration is the number-one redesign risk.
3. Existing brand assets are starting material, not optional input. A brand that is already purple stays purple. Preserve copy voice and existing accessibility wins unless asked. State what you are keeping and replacing before touching code.
4. **Modernization levers, in order - stop when the brief is satisfied:** typography refresh, then spacing and rhythm, then color recalibration, then motion layer, then hero/key-section recomposition, then full block replacement (only when unsalvageable). Sound IA plus content plus SEO means targeted evolution beats full redesign.
5. **Never change silently:** URL slugs, primary nav labels, form field names and order, the logo or wordmark, legal and consent copy.

---

## 15. Rotation log (makes the "differ from last time" rules real)

Several rules in this file say "different from your last project." A stateless skill file cannot know that, which made those rules unenforceable in v2. Fix: keep a log.

At the end of every build, append one line to `.taste-log` at the project root (create it if absent; add it to `.gitignore` if the user prefers):

```
2026-09-09 | acme-cookware | premium-consumer | palette: forest+bone+amber | display: Cabinet Grotesk | signature: sticky-stack process section | V7 M6 D3
```

At the **start** of every build, read `.taste-log` if it exists. If the last entry in the same brief category used a palette family, display face, or signature pattern, pick a different one this time unless the brief pins it. If no log exists, say so in one line and start one. Human designers have memory and always try to do something new; this is that memory.

If the environment has no writable filesystem, note the palette family, display face, and signature in the response text so the user can carry it forward.

---

## 16. Pre-flight - the last gate before outputting code

One assertion per box, so a failure names itself. Every box is checkable mechanically against the rendered output, not against your intentions. **One failure means not done.**

**Setup**
- [ ] Design Read declared in one line?
- [ ] Dial values stated and reasoned from the brief, not silently baseline?
- [ ] `.taste-log` read (or its absence noted) and rotation applied?
- [ ] Design system chosen from the Section 1 map, or the aesthetic labeled honestly?
- [ ] Plan critiqued for genericity (Section 3 Pass 2), with revisions noted?
- [ ] Redesign mode detected and audit performed, if applicable?

**Type**
- [ ] Display face is not Inter, Roboto, Arial, or a system default (unless briefed)?
- [ ] If serif: justified in one sentence, and not Fraunces or Instrument Serif by default?
- [ ] Serif differs from the last serif in `.taste-log`?
- [ ] No single-word accenting inside a headline for decoration?
- [ ] No mixed-family emphasis (serif word inside a sans headline)?
- [ ] Every italic word with `y g j p q` has descender clearance?
- [ ] Body measure under 80 characters?

**Color**
- [ ] No pure `#000` or `#FFF`?
- [ ] Palette is tokens, not ad-hoc hex?
- [ ] Exactly one accent, used identically across all sections?
- [ ] One corner-radius system page-wide?
- [ ] Premium-consumer brief: palette is NOT default beige plus brass plus espresso?
- [ ] Palette family differs from the last same-category entry in `.taste-log`?
- [ ] Theme lock holds - no mid-page inversion?
- [ ] If dual-mode shipped: both modes actually opened and checked?

**Hero**
- [ ] At most 4 text elements?
- [ ] Headline at most 2 lines at desktop?
- [ ] Subtext at most 20 words?
- [ ] Top padding at most `pt-24`?
- [ ] CTA visible without scrolling?
- [ ] No trust strip, tagline, pricing teaser, or feature bullets inside the hero?
- [ ] A real visual is present (not text plus a gradient blob)?

**Structure**
- [ ] Logo wall sits below the hero?
- [ ] Logo wall uses real SVG logos or generated marks, logos only, no category labels?
- [ ] Eyebrow count at most `ceil(sections ÷ 3)`?
- [ ] No numbered eyebrows?
- [ ] No two eyebrows back to back?
- [ ] No 3+ consecutive sections sharing one split layout?
- [ ] At least 4 layout families present, each used once?
- [ ] No split-header pattern (big headline left, small explainer right)?
- [ ] Bento cell count equals content count, no filler tiles?
- [ ] At least 2 to 3 bento cells carry real visual variation?
- [ ] Three identical cards is not the only features idea?
- [ ] Navigation is one line at desktop, at most 80px tall?
- [ ] Long lists over 5 items use a real component, not a hairline `<ul>`?
- [ ] Numbered markers appear only where the content is genuinely a sequence?
- [ ] Signature element exists - boldness spent in exactly one place?

**Build**
- [ ] Icons from one real set, one stroke width, zero emoji, zero hand-rolled paths?
- [ ] Grid used, not flex percentage math?
- [ ] `min-h-[100dvh]`, not `h-screen`?
- [ ] Every block collapses clean below 768px?
- [ ] Zero horizontal scroll on mobile?
- [ ] Every import verified to exist?
- [ ] Fonts loaded via `next/font` or self-hosted, not a Google Fonts link?
- [ ] Exactly one design system in play?

**Motion**
- [ ] Zero raw `window.addEventListener('scroll')` handlers?
- [ ] Zero scroll or pointer values held in `useState`?
- [ ] All pins start at `top top`?
- [ ] Every `useEffect` animation has cleanup?
- [ ] Motion libraries isolated - no GSAP or Three mixed with Motion in one tree?
- [ ] Claimed MOTION level is actually shown in the output?
- [ ] Every animation justified in one sentence, or cut?
- [ ] At most one marquee?
- [ ] `prefers-reduced-motion` handled everywhere above MOTION 3?

**States and access**
- [ ] Loading state present, and it is a skeleton matching final layout?
- [ ] Empty state present, and it invites action?
- [ ] Error state present, and it says how to fix?
- [ ] `:active` gives tactile feedback?
- [ ] Keyboard focus visible on every interactive element?
- [ ] Every CTA passes AA against its real background?
- [ ] Every form element, placeholder, and error passes AA?
- [ ] Zero CTA labels wrapping at desktop?
- [ ] Zero duplicate CTA intents across the page?
- [ ] Forms use label-above-input, never placeholder-as-label?

**Content**
- [ ] Zero filler verbs ("Elevate", "Seamless", "Unleash", "Next-Gen", "Revolutionize")?
- [ ] Zero "John Doe" or "Acme" placeholder data?
- [ ] Zero fake-precise specs that aren't real or labeled mock?
- [ ] Every visible string re-read in the copy self-audit?
- [ ] One copy register throughout?
- [ ] Zero em-dashes in UI chrome?
- [ ] Quotes at most 3 lines with clean attribution?

**Images and tells**
- [ ] Image priority order followed (gen tool, then real, then labeled TODO)?
- [ ] Zero div-built fake screenshots, terminals, or dashboards?
- [ ] Zero decorative hand-rolled SVG illustrations?
- [ ] Zero pills or captions overlaid on photos as decoration?
- [ ] Section 12 scanned against the rendered output, zero tells present?
- [ ] Redesign: never-change-silently list untouched?
- [ ] Chanel pass: looked at the whole thing and removed one accessory?

---

## 17. Prove it, then log it

The pre-flight checks intent against output. This step checks output against reality.

1. **Render it.** If the environment can build, serve, or screenshot, do that and look at the result before declaring done. A picture is worth 1000 tokens. Do not report a visual outcome you have not seen.
2. **Look at three things specifically:** the hero at desktop width, the same page at 375px, and one interactive state (hover or focus) on the primary CTA. These are where the failures cluster.
3. **Say what you verified and what you didn't.** "Rendered and screenshotted" and "written but not run" are different claims. Never blur them.
4. **Append the `.taste-log` line** (Section 15) so the next build can rotate away from this one.

Then ship.
