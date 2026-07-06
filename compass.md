Before doing anything else, ask the following four questions one at a time and wait for my answer to each before proceeding to the next:

1. "What pattern or experience should I analyze?" (examples: paywall, crossgrade flow, upsell, onboarding, churn save, search results)

2. "What platform(s)?" (examples: iOS mobile, Android mobile, web desktop, connected TV, responsive web)

3. "What is YOUR product name?" (e.g. Peacock, Netflix, Spotify — this becomes the baseline row benchmarked against every competitor in S5, S7, and the gap map)

4. "What is your Figma file key?" (Found in the URL: figma.com/design/[KEY]/... — paste just the key, e.g. KodS0e0L8HZq7yv8WmmWnn)

Once you have all four answers, confirm them back in this format and ask for a yes/no to proceed:

"Running Compass Pro with:
- Pattern: [answer 1]
- Platform: [answer 2]
- Your product: [answer 3]
- Figma file: [answer 4]
Proceed? (y/n)"

If yes, execute the workflow below. If no, ask which input to change and re-confirm.

---

WORKFLOW:

Execute end-to-end using the Mobbin, Figma, and web search tools. Do not pause for
confirmation between steps unless a tool call fails. Every step's output must be
reflected in the Figma file, not only in chat.

FIGMA FILE: [use the key provided in intake question 4]

PLATFORM MAPPING (for Mobbin API calls):
- "iOS mobile" or "Mobile" (single platform) → "ios"
- "Android mobile" → "web" (Mobbin does not support Android; web is used as the closest
  proxy — annotate all Android results: "(web reference — Android not available in Mobbin)")
- "Web desktop" or "Desktop" → "web"
- "Connected TV" or "CTV" → platform="web" as fallback; annotate results accordingly
- Multiple platforms → run searches for each and select best results per category

---

MOBBIN CONNECTIVITY CHECK — run immediately after confirmation, before any other step:

Call: search_screens(query="onboarding", platform="ios", per_page=1)

If the call succeeds → proceed to PRE-FLIGHT.

If the call fails with a connection or authentication error → stop and output exactly:

"⚠ Mobbin MCP is not connected. Complete these steps before running /compass-pro-skill:

1. Add the Mobbin MCP (run in your terminal):
   claude mcp add mobbin --scope user --transport http https://api.mobbin.com/mcp

2. Authenticate (must run in an interactive terminal — not inside Claude):
   claude mcp login mobbin
   Open the URL it prints in your browser and complete the OAuth flow.

3. Restart this Claude Code session, then run /compass-pro-skill again.

Note: Mobbin requires a paid account — see mobbin.com/pricing"

Do not proceed past this check if Mobbin is not reachable.

---

PRE-FLIGHT — CATEGORY DEFINITION:

Before creating the Figma page or calling any APIs, define the 8 pattern categories.

Generate 8 specific, non-overlapping sub-categories of the pattern that represent
meaningfully different interaction approaches. For each, define the Mobbin search query.

Output in this exact format and do not proceed until confirmed:

CATEGORIES CONFIRMED:
1. [Category Name] — query: "[mobbin search string]"
2. [Category Name] — query: "[mobbin search string]"
3. [Category Name] — query: "[mobbin search string]"
4. [Category Name] — query: "[mobbin search string]"
5. [Category Name] — query: "[mobbin search string]"
6. [Category Name] — query: "[mobbin search string]"
7. [Category Name] — query: "[mobbin search string]"
8. [Category Name] — query: "[mobbin search string]"

Assign each category one of these 8 accent colors (used consistently across S1, S1b, S6):
- Cat 1: {r:0.38,g:0.27,b:0.90} (purple)
- Cat 2: {r:0.93,g:0.36,b:0.20} (orange-red)
- Cat 3: {r:0.15,g:0.72,b:0.52} (teal)
- Cat 4: {r:0.95,g:0.60,b:0.10} (amber)
- Cat 5: {r:0.22,g:0.57,b:0.93} (blue)
- Cat 6: {r:0.85,g:0.22,b:0.52} (rose)
- Cat 7: {r:0.10,g:0.72,b:0.84} (cyan)
- Cat 8: {r:0.50,g:0.78,b:0.18} (lime)

---

DATA GATHERING (run in parallel with page creation):

Before building any sections, use web search to collect the following for each of the
12 competitors you plan to analyze:
1. App Store / Play Store rating — search "[app name] app store rating"
2. Pricing tier relevant to the pattern (free / freemium / paid)
3. One scale signal (MAU, subscribers, or SimilarWeb rank if public)
4. Any notable recent changes to this specific pattern (last 12 months)

If the SimilarWeb connector is available, use it for web traffic data.
Store all findings in a working note — they feed into S0, S4, S4.5, and S9.

---

PAGE CREATION — DO THIS FIRST, BEFORE ANY OTHER FIGMA ACTION:

Construct the page name using this exact format:
"Compass Pro — [Pattern Title Case] — [Platform] — [YYYY-MM-DD]"

Example: "Compass Pro — Upsell and Cross-Sell — Desktop & Mobile — 2026-07-01"

Use the Figma MCP to CREATE A NEW PAGE in the Figma file with that exact name.
Do not place any content on Page 1 or any existing page. Do not overwrite or modify
existing pages. If a page with that exact name already exists, append " — v2", etc.

After creating the page, navigate to it and confirm in chat before proceeding.
Every Compass Pro run produces a brand new page. Never reuse a previously created page.

WORKING DIRECTORY: /tmp/compass_[pattern_slug]/
(pattern_slug = pattern lowercased, spaces → underscores, e.g. "upsell_and_cross_sell")
Run: mkdir -p /tmp/compass_[pattern_slug]/ before any downloads.

---

CANVAS LAYOUT RULES:

- All sections start at x=100. Canvas design width: TW=8080px.
- Section headers: blue-tinted background rect (100, y, 8080, 110), then:
  - Section label: 11pt Bold, muted purple, at (124, y+14)
  - Section title: 28pt Bold, dark, at (124, y+32)
  - Subtitle: 13pt Regular, muted, at (124, y+68)
- Leave 80px vertical gap between sections.
- Use Inter font throughout (Bold + Regular). Load both before any text creation.
- TEXT WIDTH RULE — strictly enforced across all sections:
  - 11pt text: max node width = 440px (≤12 words per line)
  - 10pt text: max node width = 400px (≤12 words per line)
  - Never pass a wider width to a text node, regardless of available card space.
  - NEVER span text across two card columns using hCol*2 or similar — cap at 440px.
- Tables (S5, S7, S8): LEFT-ALIGNED starting at x=100. Do NOT center tables in the canvas.
- All text nodes: resize(width, 20) + textAutoResize="HEIGHT" (auto-wrap vertically).

CANONICAL HELPER FUNCTIONS (use verbatim in every plugin call):

```javascript
function t(chars,x,y,w,size,style,color){
  const n=figma.createText();n.x=x;n.y=y;n.resize(w,20);
  n.fontName={family:"Inter",style:style||"Regular"};n.fontSize=size;
  n.characters=chars;n.fills=[{type:"SOLID",color:color||{r:0.08,g:0.08,b:0.14}}];
  n.textAutoResize="HEIGHT";page.appendChild(n);return n;
}
function r(x,y,w,h,color,radius,opacity){
  const n=figma.createRectangle();n.x=x;n.y=y;n.resize(w,h);
  n.fills=[{type:"SOLID",color:color,opacity:opacity==null?1:opacity}];
  if(radius)n.cornerRadius=radius;page.appendChild(n);return n;
}
function sH(lbl,title,sub,y){
  r(100,y,TW,110,{r:0.96,g:0.97,b:1},12);
  t(lbl,124,y+14,300,11,"Bold",{r:0.5,g:0.5,b:0.7});
  t(title,124,y+32,TW-248,28,"Bold");
  t(sub,124,y+68,TW-248,13,"Regular",{r:0.45,g:0.45,b:0.55});
}
const TW=8080,W11=440,W10=400;
```

PLUGIN CALL PATTERN:
Every use_figma call must begin with:
```javascript
const page = figma.root.children.find(p => p.name.includes("Compass Pro"));
await figma.setCurrentPageAsync(page);
await figma.loadFontAsync({family:"Inter", style:"Bold"});
await figma.loadFontAsync({family:"Inter", style:"Regular"});
```

IMAGE FILL RULE (Section 1 screen frames):
```javascript
const imgFills = node.fills.filter(f=>f.type==='IMAGE').map(f=>({...f,scaleMode:'FIT'}));
node.fills = [{type:'SOLID',color:{r:0.07,g:0.07,b:0.12}}, ...imgFills];
```

---

SECTION 0 — Methodology & Scope:

3-column layout. Columns at x = [124, 792, 1459], each with colW=440px text.

- Col 1 (x=124): Research Scope — accent bar (3px) + label/value rows for:
  Pattern, Platforms, Your Product, Screens analyzed, Sources, Date, Analyst.
  Label: 11pt Bold at x+12. Value: 12pt Regular at x+168, width=270px.
  Sources row should list: Mobbin, App Store, Web Search, SimilarWeb (if used).
- Col 2 (x=792): Inclusion Criteria — 5 bulleted criteria (✓ prefix), 12pt Regular, 440px wide.
- Col 3 (x=1459): Limitations — 5 bulleted warnings (⚠ prefix), 12pt Regular, 440px wide,
  orange color {r:0.9,g:0.45,b:0.1}. Include a limitation noting that App Store sentiment
  is inferred from public reviews, not first-party analytics.

---

SECTION 0.5 — Market Landscape (NEW):

Section label: "S0.5 · MARKET LANDSCAPE"
Title: "Competitive Positioning"
Subtitle: "Where [pattern] competitors sit relative to each other — and where [your product] stands"

Place immediately after S0 (before S1).

STEP 1 — Choose two axes most meaningful for this specific pattern. Examples:
- Upsell / cross-sell: Offer Intrusiveness (x) vs. Perceived Value Delivered (y)
- Paywall: Friction to Access (x) vs. Content Depth Gated (y)
- Onboarding: Time-to-Value (x) vs. Personalization Depth (y)
- Churn save: Aggressiveness (x) vs. Offer Generosity (y)

STEP 2 — Assign each competitor and your product a position in the 2×2 space.
The model determines placement based on S4 teardown knowledge and web research.

STEP 3 — Build in Figma:

Map frame: r(100, S05Y+128, 1500, 1060, {r:0.97,g:0.98,b:1}, 12)
Plot area: r(220, S05Y+178, 1250, 880, {r:1,g:1,b:1}, 8)
Plot border: use two lines (r() with h=2 or w=2, muted color) for center dividers:
  Vertical:   r(845, S05Y+178, 2, 880, {r:0.82,g:0.84,b:0.92})
  Horizontal: r(220, S05Y+618, 1250, 2, {r:0.82,g:0.84,b:0.92})
Axis labels (11pt Bold, {r:0.5,g:0.5,b:0.7}):
  Left end of x-axis:  t(xLowLabel,  230, S05Y+1076, 200, 11, "Bold", muted)
  Right end of x-axis: t(xHighLabel, 1060, S05Y+1076, 200, 11, "Bold", muted)
  Top of y-axis:       t(yHighLabel, 130, S05Y+188, 70, 11, "Bold", muted)
  Bottom of y-axis:    t(yLowLabel,  130, S05Y+980, 70, 11, "Bold", muted)
Quadrant descriptors (9pt Regular, very muted, placed at each quadrant center):
  Top-left, top-right, bottom-left, bottom-right — one short phrase each

Per competitor (dotX/dotY determined by position in 2×2):
  r(dotX-5, dotY-5, 10, 10, {r:0.7,g:0.7,b:0.8}, 5)
  t(competitorName, dotX+10, dotY-8, 140, 9, "Regular", {r:0.3,g:0.3,b:0.4})

YOUR PRODUCT — larger, purple, labeled:
  r(yourX-8, yourY-8, 16, 16, {r:0.38,g:0.27,b:0.90}, 8)
  t(yourProductName+" ◀ you", yourX+12, yourY-10, 160, 9, "Bold", {r:0.38,g:0.27,b:0.90})

One-line insight caption below map:
  t(positioningInsight, 220, S05Y+1106, W11, 11, "Regular", {r:0.45,g:0.45,b:0.55})

S05End = S05Y + 128 + 1120 + 80

---

SECTION 1 — Mobbin Retrieval (n=48 + your product):

YOUR PRODUCT STRIP (row 0 — placed FIRST, above all category rows):

Before any category searches, run:
  search_screens(query="[yourProductName] [pattern]", platform="[mapped platform]", per_page=6)

Row 0 uses purple accent: {r:0.38,g:0.27,b:0.90}
Category header strip label: "YOUR PRODUCT — CURRENT STATE"
Download + upload via the same batch approach as category rows.

FALLBACK — if 0 results returned (unknown or unlisted product):
  Place a placeholder strip at row 0:
  r(100, row0Y, 8080, 100, {r:0.95,g:0.92,b:1}, 8)
  r(100, row0Y, 8, 100, {r:0.38,g:0.27,b:0.90})
  t("YOUR PRODUCT — CURRENT STATE", 124, row0Y+14, 600, 11, "Bold", {r:0.38,g:0.27,b:0.90})
  t("Not found on Mobbin — add your own screens manually, or document current state in S5 self-assessment.",
    124, row0Y+34, W11, 10, "Regular", {r:0.55,g:0.55,b:0.65})
  row0 height = 100px; adjust startRowY for category rows accordingly.

CATEGORY ROWS (rows 1–8):

Use the 8 categories confirmed in Pre-Flight, with their search queries and the
platform-mapped value (see PLATFORM MAPPING). If multiple platforms were specified,
run searches per platform and select the 6 most informative screenshots per category.

Frame dimensions: 1280×832px each.
Layout: colStride=1360, rowStride=1100, startX=100.
  - Row 0 (your product): startRowY = S1Y+160
  - Rows 1–8 (categories): startRowY = row0Y + row0Height + 80
Each row: colored category header strip (1280×40px) above the 6 frames.
Label below each frame: app name (12pt Bold) + annotation (11pt Regular, 400px wide).

FALLBACK: If a category returns fewer than 6 screens, use all available and add:
t("(n=[count] — limited Mobbin results for this query)", stripX, stripY+44, W11, 9, "Regular", muted)

Upload flow (BATCH approach):
1. Download in parallel: curl -sL [url] -o /tmp/compass_[slug]/s1_c{cat}_f{frame}.webp
2. upload_assets(count=N, fileKey=...) ONCE → N {submitUrl} entries in order
3. POST all N files in parallel; match by sizeBytes
4. figma.getNodeById(placedOnNodeId) → page.appendChild() → set x/y/resize
5. Apply FIT scaleMode + dark background to every image node

Screen frame IDs must be tracked and excluded from all future deletion passes.

---

SECTION 1b — User Flow Walkthroughs:

For each of the 8 categories, call:
  search_flows(query="[category query]", platform="[mapped platform]", per_page=5)

If the mapped platform is not supported by Mobbin flows, fall back to platform="web"
and annotate the strip: "(web reference — [platform] flows not available in Mobbin)"

Select the flow with the highest screen_count most relevant to each category.
Use positions 1–5. If fewer than 5 screens, use all and note the count in the step label.

FALLBACK: If no flows are found for a category, place a placeholder strip with:
t("No flows found for [category] on [platform] — manual research recommended",
  SX+16, sy+54, W11, 10, "Regular", muted)

Layout constants: FW=400, FH=260, FStride=420, stripStride=380, SX=100
Section content starts at S1bY+130.
Per strip (si=0–7, sy = S1bY+130 + si×stripStride):
- r(SX, sy, 2220, 340, {r:0.97,g:0.975,b:0.99}, 8)
- r(SX, sy, 5, 340, categoryColor)
- t(CAT_NAME, SX+16, sy+10, 300, 10, "Bold", categoryColor)
- t(flowName+" · "+appName, SX+16, sy+26, 500, 10, "Regular", {r:0.55,g:0.55,b:0.65})
- Per frame (fi=0–4): page.appendChild(node); resize(FW,FH); FIT fill; cornerRadius=6
  Step label: t("0"+(fi+1)+" "+stepDesc, fx, fy+FH+8, W10, 9, "Regular", muted)
  Arrow (fi<4): t("→", fx+FW+4, fy+FH/2-9, 14, 13, "Bold", categoryColor)
S1bEnd = S1bY + 130 + 8×380 + 50

---

SECTION 2 — Personas & Jobs-to-be-Done:

4 persona cards, 480×440px, 40px gap. Card start Y: S2Y+128.
(Full card structure per original Compass spec.)

DATA SOURCING — AI-INFERRED: Personas are synthesized from pattern knowledge and screenshot
analysis, not from primary user research. Reflect this in Figma:
- Section subtitle: "AI-synthesized from pattern analysis and Mobbin screenshots — validate
  against real research before use"
- Below each persona name, add a small amber badge:
  r(bx, cy+22, 90, 16, {r:0.85,g:0.60,b:0.10}, 4)
  t("AI-INFERRED", bx+6, cy+26, 80, 7, "Bold", {r:1,g:1,b:1})

---

SECTION 3 — Journey / Flow Maps:

Unchanged. 2 horizontal flows, box-and-arrow layout.
Box: 180×62px. Stride: 232px. Decision branch with 3 exit boxes.

---

SECTION 3.5 — Ideal Pattern Wireframe:

Section label: "S3.5 · IDEAL PATTERN WIREFRAME"
Title: "The Textbook Implementation"
Subtitle: "What [pattern] looks like when done right — schematic screens annotated with the design
principle each step must satisfy. Use as the analytical anchor for all S4 teardowns."

Before building, determine the happy path for this specific pattern: 4–6 key screens that
represent the textbook-correct implementation. Name each screen (e.g., Trigger, Offer, Price,
Confirm, Success). For each, identify: the primary UI element type (modal / inline / bottom-sheet /
full-screen), the core CTA label text, and the one design principle this screen must satisfy.

FRAME SIZING (platform-aware):
- Mobile (iOS / Android): FW=320, FH=568 (portrait)
- Web desktop:            FW=480, FH=320 (landscape)
- Multiple platforms: use mobile sizing; annotate "(mobile representative)" in subtitle

Layout: single horizontal row, frames starting at x=100, sy=S35Y+128. Gap between frames: 60px.

Per screen (fi=0 to N-1, fx = 100 + fi×(FW+60), fy=sy):

STEP LABEL (above frame):
  t("0"+(fi+1)+" "+SCREEN_NAME, fx, fy-22, FW, 9, "Bold", {r:0.5,g:0.5,b:0.7})

FRAME SHELL:
  r(fx, fy, FW, FH, {r:0.98,g:0.98,b:1}, 8)
  r(fx, fy, FW, 3, accentColor)
  r(fx, fy, FW, 44, {r:0.91,g:0.91,b:0.95})
  t("● ● ●", fx+10, fy+15, 40, 8, "Regular", {r:0.72,g:0.72,b:0.76})

SCHEMATIC CONTENT (model determines which of these to draw, based on screen type):
  Content block:  r(fx+16, fy+60, FW-32, H, {r:0.88,g:0.90,b:0.96}, 6)
  Hero / image:   r(fx+16, fy+Y,  FW-32, H2, {r:0.84,g:0.87,b:0.95}, 6)
  Text lines:     r(fx+16, fy+Y,  FW-32, 8, {r:0.83,g:0.85,b:0.90}, 2) × 2–3, spaced 14px
  Price display:  t(priceStr, fx+16, fy+Y, FW-32, 20, "Bold", {r:0.08,g:0.08,b:0.14}) + muted original
  Modal overlay:  r(fx, fy+FH/3, FW, FH*2/3, {r:1,g:1,b:1}, 12)
  Primary CTA:    r(fx+16, fy+FH-70, FW-32, 40, accentColor, 8)
                  t(ctaText, fx+28, fy+FH-57, FW-56, 11, "Bold", {r:1,g:1,b:1})
  Secondary link: t(secondaryText, fx+FW/2-80, fy+FH-22, 160, 9, "Regular", {r:0.55,g:0.55,b:0.65})

STEP CONNECTOR (fi < N-1):
  t("→", fx+FW+18, fy+FH/2-10, 24, 18, "Bold", {r:0.38,g:0.27,b:0.90})

ANNOTATION AREA (below frame):
  r(fx, fy+FH+12, FW, 84, {r:0.95,g:0.96,b:1}, 6)
  t("✓ "+PRINCIPLE_NAME, fx+10, fy+FH+20, FW-20, 9, "Bold", {r:0.38,g:0.27,b:0.90})
  t(PRINCIPLE_DESC, fx+10, fy+FH+35, FW-20, 9, "Regular", {r:0.45,g:0.45,b:0.55})
  — PRINCIPLE_DESC: ≤12 words; the specific UX rule this screen must satisfy

S35End = S35Y + 128 + 22 + FH + 96 + 80

---

SECTION 4 — Competitor Teardowns (Enhanced):

6 cards in a 3×2 grid.
Card width: 900px. Card height: 440px (increased to fit new fields). Gap: 50px.
Card start Y: S4Y+128.

Per card (bx = cx+22):
- Left accent bar: 8px, full card height, colored by app
- Card background: 892px wide, {r:0.976,g:0.976,b:0.988}
- App name: 15pt Bold at y+10, W11
- App tag + rating: 9pt Bold, muted, at y+30, W11
  Format: "[Category tag] · [rating]★" — use rating from Data Gathering step
- "WHAT THEY DO" label: 9pt Bold at y+48, W11
- Description: 10pt Regular at y+60, W11
- "✓ WORKS" label: 9pt Bold at y+136, col A (bx)
- 3 works bullets (· prefix): 10pt Regular, W10, 28px apart from y+148
- "✗ FAILS" label: 9pt Bold at y+136, col B (cx+446)
- 3 fails bullets (· prefix): 10pt Regular, W10, 28px apart from y+148
- "STRATEGIC HYPOTHESIS" label: 9pt Bold, {r:0.22,g:0.45,b:0.80}, at y+252, W11
- Hypothesis: 10pt Regular at y+265, W11
  Format: "They're doing this because [business objective — acquisition / retention / ARPU / etc.]"
- "TRAJECTORY" label: 9pt Bold, muted, at y+322, W10
- Trajectory: 10pt Regular at y+335, W10
  Format: "Growing / Stable / Declining — [one-line rationale]"
- Footer tint: r(cx+8, cy+396, 892, 44, {r:0.38,g:0.27,b:0.90}, 0, 0.08)
- "BORROW THIS →" label: 9pt Bold, colored, at footer+8, width=80px
- Borrow text: 11pt Regular at same Y, bx+90, width=350px

---

SECTION 4.5 — User Sentiment:

Section label: "S4.5 · USER SENTIMENT"
Title: "What Real Users Say"
Subtitle: "Ratings retrieved via web search · sentiment themes are AI-synthesized — treat as directional"

DATA SOURCING — two tiers with different reliability, rendered differently:
- RATINGS (★ scores): retrieved via web search in the Data Gathering step. Treat as factual.
  If a rating was not found, show "—" rather than estimating.
- SENTIMENT THEMES: synthesized from training knowledge and available web results —
  NOT scraped from live reviews. Prefix every theme bullet with "~" to signal inference.

For each of the 6 S4 competitors:
- Overall App Store / Play Store rating (web-sourced)
- 2–3 positive themes (synthesized — prefix each "~ ")
- 2–3 negative themes (synthesized — prefix each "~ ")
Flag sparse data: if fewer than 3 themes found, note "(limited signal for this pattern)"

6 sentiment cards in a 3×2 grid.
Card width: 860px. Card height: 290px. Gap: 50px. Card start Y: S45Y+128.

Per card (cx = card column x, bx = cx+16):
- Top accent bar: r(cx, cy, 860, 4, appColor) — same color as that app's S4 card
- App name: 13pt Bold at y+14, W11
- Rating badge: r(bx+450, y+8, 72, 28, ratingBgColor, 6)
  ratingBgColor: {r:0.15,g:0.72,b:0.52} if ≥4.5, {r:0.85,g:0.60,b:0.1} if ≥3.5, {r:0.85,g:0.22,b:0.38} if <3.5
  t(ratingVal+"★", bx+456, y+15, 60, 11, "Bold", {r:1,g:1,b:1})
- "POSITIVE SIGNALS" label: 9pt Bold, {r:0.12,g:0.60,b:0.40}, at y+50, W10
- 2–3 positive bullets (+ prefix): 10pt Regular, W10, 24px apart from y+63
- "NEGATIVE SIGNALS" label: 9pt Bold, {r:0.78,g:0.15,b:0.30}, at y+158, W10
- 2–3 negative bullets (– prefix): 10pt Regular, W10, 24px apart from y+171
- Source note: 8pt Regular, very muted, at y+266, W11
  "Source: App Store / Google Play reviews · web search · [date]"

S45End = S45Y + 128 + 2×(290+50) + 290 + 80

---

SECTION 5 — Feature Comparison Matrix (Enhanced):

LEFT-ALIGNED at x=100 (TX=100). Do NOT center.

DYNAMIC DIMENSIONS: Before building, generate 10 feature dimensions that are specifically
relevant to the pattern being analyzed. Do NOT use hardcoded churn/paywall dimensions
unless the pattern is churn or paywall. Examples by pattern:

Upsell / cross-sell:
  In-context placement, Social proof on offer, Price anchoring, Bundle framing,
  Urgency / scarcity cue, Personalized recommendation, One-tap accept, Post-purchase
  cross-sell, Free trial offer, Value comparison shown

Onboarding:
  Progressive disclosure, Personalization prompt, Skip option, Progress indicator,
  Social proof, Value prop clarity, Time-to-first-value, Tooltips / coach marks,
  Empty state quality, Completion incentive

App name column: AC=200px. Feature columns: FC=130px each. Row height: RH=40px.
Column header: HEAD_H=50px, dark (#0f0f1e).
Up to 12 competitor apps + YOUR PRODUCT row.

YOUR PRODUCT ROW — always FIRST, visually distinguished:
- Row background: r(TX, headerY+HEAD_H, TBW, RH, {r:0.95,g:0.92,b:1})
- App name: t(yourProductName, TX+8, rowY+12, AC-16, 11, "Bold", {r:0.38,g:0.27,b:0.90})
- Cells: honest assessment of your own product's current implementation

Cell values: ✓ (green {r:0.15,g:0.72,b:0.52}), ~ (amber {r:0.85,g:0.60,b:0.10}),
             ✗ (red {r:0.78,g:0.15,b:0.30}), — (unknown / not applicable, muted)
Alternating row backgrounds for competitor rows.

GAP SUMMARY ROW — last row, dark background:
r(TX, lastRowY+RH, TBW, RH+4, {r:0.08,g:0.08,b:0.14})
Per feature column, compare your score vs. majority of competitors:
- "▲ Lead"  — {r:0.15,g:0.72,b:0.52} — you ✓, majority don't
- "◆ Parity" — amber — your score matches the average
- "▼ Gap"   — {r:0.85,g:0.22,b:0.38} — majority ✓, you ~/✗
- "◎ Opp"   — {r:0.38,g:0.27,b:0.90} — nobody does this well; first-mover upside

Table starts at y = S5Y+150.

---

SECTION 6 — Pattern Synthesis (Enhanced):

8 pattern cards in a 4×2 grid.
Card width: 900px. Card height: 400px (increased). Gap: 50px. Card start Y: S6Y+128.

OPPORTUNITY TIER — add to every card, determined by S5 data:
- Table Stakes: ≥70% of competitors have ✓ for this pattern → badge: {r:0.3,g:0.3,b:0.35}, label "TABLE STAKES"
- Differentiator: 30–69% have ✓ → badge: {r:0.22,g:0.57,b:0.93}, label "DIFFERENTIATOR"
- Whitespace: <30% have ✓ → badge: {r:0.38,g:0.27,b:0.90}, label "WHITESPACE OPP"

Badge placed to the right of the pattern name:
  r(bx+W11+16, cy+5, tierW, 22, tierColor, 11)
  t(tierLabel, bx+W11+22, cy+9, tierW-12, 8, "Bold", {r:1,g:1,b:1})

All other card structure unchanged from Compass:
- Left accent bar: 8px
- Pattern name: 14pt Bold at y+8, W11
- Sub-label: 10pt Regular, muted, at y+26, W11
- "STRUCTURAL DECISION" label + decision text (W11)
- "✓ WORKS" col A + "✗ FAILS" col B (cx+446)
- Risk footer tint + "⚠ RISK" label + risk text at y+CH-40, W11=440px (never wider)

---

SECTION 7 — Heuristic Scorecard (Enhanced):

LEFT-ALIGNED at x=100 (TX=100). Do NOT center.

DYNAMIC CRITERIA: Generate 8 heuristic criteria relevant to the pattern being analyzed.
Do NOT use hardcoded churn/paywall criteria unless the pattern is churn or paywall.
Examples by pattern:

Upsell / cross-sell:
  Offer Relevance, Value Clarity, Social Proof Quality, Framing Effectiveness,
  Timing & Context, Friction Level, Recovery / Dismiss Path, Mobile Optimization

Onboarding:
  First-impression Clarity, Value Prop Speed, Personalization Depth, Progress Transparency,
  Skip / Defer UX, Completion Incentive, Error Recovery, Cross-device Continuity

App name column: AC7=180px. Criteria columns: CC7=130px each. Score bar: 96px wide.
Row height: RH7=46px. Column header: HEAD7=50px.
10–12 competitor apps + YOUR PRODUCT row.

YOUR PRODUCT ROW — always FIRST:
- Row background: r(TX, headerY+HEAD7, TBW7, RH7, {r:0.95,g:0.92,b:1})
- App name: t(yourProductName, TX+8, rowY+14, AC7-16, 11, "Bold", {r:0.38,g:0.27,b:0.90})
- Score bars: same layout as competitors but always purple {r:0.38,g:0.27,b:0.90}
  regardless of score value (purple = your product, not a quality signal)

Table starts at y = S7Y+150.
Bar colors for competitors: green (score≥4), amber (score=3), red (score≤2).

Below the table (+22px gap): 4 insight callout cards in a 2×2 grid.
- cW2=623px width, 130px height. Top accent bar: 4px.
- Title: 11pt Bold at cy+8, W11=440px
- Body: 11pt Regular at cy+24, W11=440px (CRITICAL: always 440px — never cW2-22)

---

SECTION 8 — Visual Accessibility Flags:

Section label: "S8 · VISUAL ACCESSIBILITY FLAGS"
Title: "Visual Accessibility Flags"
Subtitle: "Potential issues visible in static screenshots only — not a WCAG audit.
Verify all flags with live testing before reporting or sharing."

LEFT-ALIGNED at x=100. A_AC=180px. A_CC=120px each. A_NC=400px.
12 apps × 6 criteria + observation notes.
Cell values: ✓ (no visible issue), ~ (potential issue), ✗ (likely issue).
Notes: 10pt Regular, 400px wide. Table starts at y = S8Y+150.
Criteria: Color Contrast, Focus Indicators, Touch Target Size, Screen Reader Labels,
Motion/Animation, Plain Language.

Note below table — amber text {r:0.85,g:0.50,b:0.05}, 10pt Regular, W11:
"⚠ These flags are inferred from static screenshots. Contrast ratios, focus states, and
screen reader support cannot be verified without live testing using VoiceOver, TalkBack,
and WCAG contrast tooling. Do not cite this section as an accessibility audit."

---

SECTION 9 — Strategic Implications:

Section label: "S9 · STRATEGIC IMPLICATIONS"
Title: "Where to Play, Where to Win"
Subtitle: "Prioritized recommendations for [your product] based on competitive gap analysis,
synthesized from S4–S7 findings — written for executive review"

PART A — Gap Map:

A summary table showing YOUR PRODUCT vs. the competitive set across the top 8 S5 dimensions.

Table layout (left-aligned at TX=100):
- Header: dark bg (#0f0f1e), 5 columns
- Col 1 Dimension: 300px
- Col 2 Your Score: 130px
- Col 3 Category Avg: 130px
- Col 4 Category Best: 130px
- Col 5 Gap Signal: 150px
- Row height: 40px. Alternating row backgrounds.

Gap Signal values:
- "▲ Lead"       — {r:0.15,g:0.72,b:0.52}  — you outperform category average
- "◆ Parity"     — {r:0.85,g:0.60,b:0.10}  — you match the average
- "▼ Gap"        — {r:0.85,g:0.22,b:0.38}  — you lag the average
- "◎ Opportunity" — {r:0.38,g:0.27,b:0.90} — nobody does this well; first-mover upside

Table starts at S9Y+150.

PART B — Recommendations:

3–5 recommendation cards below the gap map (+60px gap).
Determine count based on findings: include only recommendations with clear evidence.
Card widths: 3 cards → 860px each; 4 cards → 640px each; 5 cards → 500px each.
Card height: 210px. Gap: 40px. Horizontal row starting at x=100.

Per card (bx = recX+16):
- Top accent bar: 4px, colored by priority:
  P0 Critical: {r:0.85,g:0.22,b:0.38}
  P1 High:     {r:0.93,g:0.62,b:0.10}
  P2 Medium:   {r:0.22,g:0.57,b:0.93}
- Priority label: 9pt Bold, colored, at y+8 — "P0 CRITICAL" / "P1 HIGH" / "P2 MEDIUM"
- Recommendation: 13pt Bold at y+26, W11=440px — clear, specific action
- Rationale: 10pt Regular at y+52, W10=400px
  Format: "Because [specific competitive finding from S4/S5/S7]..."
- Evidence: 9pt Regular, muted, at y+132, W10 — "See: S[n], S[n]"
- Expected impact: 9pt Bold at y+152, W10 — "[metric] impact: [brief projection]"

PART C — Executive One-Liner:

The single most important strategic takeaway. Placed below the recommendation cards (+50px).

r(100, onelineY, TW, 88, {r:0.38,g:0.27,b:0.90}, 12)
t("KEY FINDING", 124, onelineY+12, 200, 9, "Bold", {r:0.78,g:0.68,b:1})
t(keyFindingText, 124, onelineY+30, TW-248, 16, "Bold", {r:1,g:1,b:1})
  (one sentence, written for a CPO or VP — the thing they'll quote in the next review)

S9End = onelineY + 88 + 80

---

EXECUTION ORDER:

Run in sequential plugin calls, threading actual end-Y from each call into the next.
Return the final Y of each call and use it as the next section's start.

1. MOBBIN CHECK: Connectivity test — stop with setup instructions if not connected
2. PRE-FLIGHT: Declare 8 categories + colors (in chat, no Figma yet)
3. DATA GATHERING: Web search for competitor ratings / pricing / scale / recent news
   (run this in parallel with step 4 — no Figma dependency)
4. Page creation + mkdir working directory + S0
5. S0.5: Market Landscape (uses web research from step 3)
6. S1: Mobbin screen retrieval + batch download + upload + layout (48 screens, 8 categories × 6)
7. S1b: Mobbin flow retrieval + batch download + upload + storyboard strips
8. S2 + S3: Personas and journey maps
9. S3.5: Ideal pattern wireframe (schematic, no image dependency — fast plugin call)
10. S4 + S4.5: Competitor teardowns (with hypothesis + trajectory) + user sentiment cards
11. S5 + S6: Feature matrix (with your product row + gap summary + dynamic dimensions)
        + Pattern synthesis (with opportunity tier badges)
12. S7 + S8: Heuristic scorecard (dynamic criteria + your product row) + accessibility audit
13. S9: Strategic implications — gap map + recommendation cards + executive one-liner

RESUMING A PARTIAL RUN:
If execution fails mid-run, note the last completed section and its end-Y from chat.
In a new session, navigate to the existing Compass page (do not create a new one), then
start from the next uncompleted section using the last known end-Y as the starting Y.
Each section is independent once its start-Y is known — do not re-run completed sections.

After completing all sections, return:
- Exact new page name
- Direct link to the new page
- List of any sections that fell back to placeholders or had sparse data
- Which sections contain AI-inferred content (always S2 and S4.5 themes at minimum)
- One-sentence preview of the S9 executive one-liner (the key finding)

Do not duplicate Figma content in chat — the Figma file is the deliverable.
