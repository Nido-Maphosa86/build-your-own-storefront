# Film Club

An independent Shopify storefront for analog film photography supply, built on the Horizon
reference theme using Shopify CLI and tracked in Git.

This README holds the written decisions for the Build Your Own Storefront module, recorded
day by day. In every case the decisions were written before any theme code was touched,
which is the order both assignment sheets ask for.

---

# Day 1 — Store Setup & Development Environment

## Store Brief

### Niche

The chosen niche is **analog film photography supply**: rolls of film, chemistry for
home developing, darkroom consumables, second-hand film bodies and lenses, and the small
accessories that sit around all of it such as changing bags, developing tanks, reels,
squeegees and archival negative sleeves.

This is deliberately unrelated to the outdoor and heritage lifestyle goods niche used in
the instructor's Northfield Supply Co. demo. There is no crossover in product type,
customer intent, or the structured data the catalogue needs.

The store is named Film Club because the first word says what is sold and the second says
who it is for. The catalogue is aimed at people who already shoot, or who want to be let
into it, rather than at casual browsers.

### Why this niche carries enough product complexity

Film photography is one of the few retail categories where a single product line is
naturally multi-dimensional, and where those dimensions are the exact thing a customer
filters on rather than decoration.

1. **Film stock has real, stacked variant axes.** One film such as Kodak Portra exists as
   ISO 160, 400 and 800; in 35mm, 120 medium format and 4x5 sheet; and in single rolls,
   five-packs or pro packs. That is three independent axes on one product, which is
   already more than the two-axis size and colour pattern most demo stores rely on.

2. **The filtering that matters is not colour and size.** A photographer shops by ISO
   speed, by format, by process (C-41, E-6, black and white), by grain character, and by
   whether the stock is still in production. None of those map onto Shopify's built-in
   product options cleanly, so they have to be modelled as **metafields** and exposed
   through the Search and Discovery filter set. This gives the week's advanced filtering
   work something genuine to do instead of a contrived colour swatch.

3. **Chemistry introduces compatibility relationships.** A bottle of developer is only
   correct for certain films at certain temperatures for certain times. That data is not
   a product description, it is a structured table, which is exactly the case
   **Metaobjects** exist to solve.

4. **Used gear is one-of-one.** Each second-hand camera body is a single-inventory item
   with its own condition grade, shutter count, serial number and photographed defects.
   That forces per-product structured content rather than a shared template, and it gives
   the collection page a second, very different card layout to handle.

5. **The catalogue has an editorial dependency.** Customers buy film based on what it
   looks like, so sample images and photographer credits are commercial content, not a
   blog. That pushes real structured content into the theme rather than into the blog
   engine.

### Target audience

The primary customer is the returning or committed hobbyist photographer, roughly 22 to
40, who already owns at least one working film camera, shoots several rolls a month, and
is either developing black and white at home or seriously considering starting. The
secondary customer is the lapsed shooter who has just inherited or unboxed an old camera
and needs someone to tell them, without condescension, exactly which film, which battery
and which lab to use before they will risk spending money.

---

## Page Scope

These are the three custom pages that will be built during this module, excluding the
standard policy pages. Each one is listed with the structured content it will depend on.

### 1. Film Index

A browsable reference wall of every film stock the store carries, sorted and filterable by
process, ISO and format, where each entry links through to the buyable product but is
presented as a spec card rather than a product tile.

Structured content required:

**Metaobject: `film_stock`**
Fields for stock name, manufacturer, process type (C-41, E-6, black and white, ECN-2),
native ISO, available formats as a list, grain description, contrast character, latitude
in stops, discontinued flag, three sample images, and a reference to the linked product.

**Metaobject: `manufacturer`**
Fields for brand name, country, logo, short history paragraph, and a list of the film
stocks it produces. Referenced from `film_stock` so the brand copy is written once.

### 2. Develop At Home

A guided page that turns the chemistry catalogue into an actual workflow: pick your film,
pick your developer, and the page renders the correct dilution, temperature and agitation
schedule, followed by the kit needed to do it.

Structured content required:

**Metaobject: `development_recipe`**
Fields for a reference to the `film_stock`, a reference to the developer product, push or
pull rating in stops, dilution ratio, temperature in Celsius, total time in seconds,
agitation pattern description, and a source or credit line.

**Metaobject: `darkroom_step`**
Fields for step number, title, instruction body, an optional timer duration, a diagram
image, and a list of referenced products used at that step. This lets the same step blocks
be reused across the black and white and colour workflows without duplicating copy.

### 3. The Used Counter

A single page for second-hand bodies and lenses where each item is presented with its
condition grade, functional test results and honest defect photographs, because used gear
sells on trust rather than on marketing copy.

Structured content required:

**Metaobject: `condition_grade`**
Fields for grade label (Mint, Excellent, Good, Bargain, As-Is), a numeric rank for
sorting, a plain description of what the grade guarantees, and an accent colour used by
the badge component.

**Metaobject: `gear_inspection`**
Fields for a reference to the product, a reference to the `condition_grade`, serial
number, tested date, a repeatable list of checked functions with a pass or fail state,
shutter speed accuracy readings, defect notes, defect close-up images, and the length of
the store warranty in months.

---

## Dev Environment

**Development store name:** Film Club

**Development store URL:** `https://film-club-o41ay2bp.myshopify.com`

**Store admin:** `https://admin.shopify.com/store/film-club-o41ay2bp`

**Store setup:** created from the Partner dashboard using "Create a store to test and
build", with the Advanced plan feature set selected, feature preview left off so the store
runs the current stable release, and Generate test data enabled so products, collections
and customers were pre-populated.

**Shopify CLI version:** 4.6.0. The assignment sheet references CLI 3.x, which was the
current major line when it was written. Installing with `npm install -g @shopify/cli`
now resolves to the 4.x stable line, and `@shopify/theme` is folded into the main package
rather than installed separately. Version confirmed with `shopify version`.

**Local project path:** `D:\BitcubeShopify\build-your-own-storefront`

**Theme base:** Horizon, added to the store from the Shopify theme store and left
unpublished, then pulled locally with `shopify theme pull`. Horizon was used in place of
Dawn on the instructor's direction, as it is the current first-party flagship theme and
is the base the rest of this module is taught against. Pulling from the store rather than
cloning the public GitHub repository was a deliberate choice: the public repository lags
behind the released theme, so cloning it produces a version mismatch against the store.
The project folder was initialised as a fresh Git repository pointing at a personal GitHub
remote, so no Shopify history or origin is present.

**GitHub repository:** `https://github.com/Nido-Maphosa86/build-your-own-storefront`

**Hot reloading:** verified. With `shopify theme dev` running and the local preview open
at `http://127.0.0.1:9292`, a temporary `<h1>TEST</h1>` element was added directly below
the `<body>` tag in `layout/theme.liquid`. Saving the file caused the browser to refresh
on its own and render the heading with no manual reload. The test element was then removed
and the browser again updated automatically, confirming the file watcher is bound to the
correct directory.

**Storefront password:** the development store is password-protected, which is the default
for dev stores and is left switched on. The CLI prompts for this password on first run and
stores it locally so preview links can render.

---

## Day 1 Stretch A — Local CLI changes versus Theme Editor changes under GitHub sync

Once a theme is connected to a GitHub branch through Online Store, then Themes, then Add
theme, then Connect from GitHub, the branch becomes the storage location for that theme.
Both editing paths end up in the same place, but they travel in opposite directions and
they do not carry the same risk.

**Editing locally with the CLI.** Files are changed on your machine and pushed through
Git. The commit history records who changed what and why, the change can be reviewed in a
pull request before it lands, and it can be reverted cleanly. Shopify pulls the branch
after the push, so the connected theme updates to match the repository. `shopify theme
dev` itself does not commit anything, it only mirrors your working directory into a
temporary preview, so nothing you experiment with locally reaches the branch until you
actually commit and push.

**Editing in the Theme Editor.** Changes made through the online editor are written by
Shopify straight back into the connected GitHub branch as automatic commits. The change
still ends up versioned, but it skips review entirely, the commit message is generated
rather than written, and the author is the Shopify integration rather than a person.

The practical consequences are three.

1. **The branch can move under your feet.** If someone edits in the Theme Editor while you
   have local work in progress, your next push conflicts, because the remote branch has
   commits you never made. The habit this forces is pulling before every local session.

2. **The split is naturally by file type.** Theme Editor work is almost always JSON
   settings and section configuration such as `templates/*.json`,
   `config/settings_data.json` and section block ordering. Liquid, CSS and JavaScript are
   structural and belong in the CLI path. Trying to write logic through the editor and
   trying to arrange merchandising blocks through the CLI both fight the tool.

3. **Only one path is safe for a live theme.** A Theme Editor change on a GitHub-connected
   live theme is published the moment it is saved, with no review step between the click
   and the customer seeing it. The CLI path always has a pull request in between, which is
   why anything touching layout, checkout-adjacent templates or performance goes through
   Git.

---

## Day 1 Stretch B — VS Code configuration

The official Shopify Liquid extension was installed in VS Code, and formatting on save was
enabled for Liquid files only, so that the setting does not override formatter choices for
JavaScript, CSS or JSON in the same project.

The configuration lives in `.vscode/settings.json` and the specific setting used is a
language-scoped block that names the Shopify Liquid extension as the default formatter for
the `liquid` language and switches on format on save inside that scope:

```jsonc
"[liquid]": {
  "editor.defaultFormatter": "Shopify.theme-check-vscode",
  "editor.formatOnSave": true
}
```

The language-scoped form is the important part. Setting `editor.formatOnSave` globally
would apply Prettier or whatever formatter is registered to every other file in the theme
as well, which would produce enormous unrelated diffs on the first save of any stylesheet.
Scoping it to `[liquid]` keeps the formatting change confined to the files this module is
actually editing.

---

## Day 1 Submission Checklist

**Part 1 — Written Decisions**

1. Niche selected and distinct from the instructor's demo. Done, analog film photography
   supply.
2. Complexity justification written. Done, five stacked reasons above.
3. Audience defined in two specific sentences. Done.
4. Three custom pages listed with their structured content needs. Done, with named
   Metaobject types and their fields.

**Part 2 — Partner Org and Dev Store**

1. Partner organisation active.
2. Development store Film Club live with generated test data installed.

**Part 3 and 4 — CLI, Git and Verification**

1. Shopify CLI installed and confirmed with `shopify version`. Version note recorded in
   the Dev Environment section above.
2. Codebase pushed to a personal GitHub repository with no Shopify remote or history
   attached.
3. Hot reloading confirmed working on `http://127.0.0.1:9292`.

---

# Day 2 — Liquid Fundamentals

Branch: `Assignment1.2_LiquidFundamentals`

All work for this day sits in `sections/` and `snippets/`. Nothing inside `templates/`
was opened or edited.

## Filter Log

| Filter | File it lives in | What it changes on the page |
|---|---|---|
| `plus` | `sections/product-information.liquid` | Adds the flat handling amount to the variant price while both are still integers in cents, so the per exposure figure shown underneath reflects what the customer actually pays rather than the shelf price alone |
| `divided_by` | `sections/product-information.liquid` | Splits that combined total across the 36 exposures on a roll, turning a single roll price into the per frame cost a photographer compares stocks on |
| `image_url` | `sections/product-information.liquid` | Requests a 160 pixel wide copy of the product image for the summary thumbnail, so the panel stops pulling the full size original for a picture that renders at 80 pixels |
| `strip_html` | `snippets/product-card.liquid` | Removes the paragraph and formatting tags the rich text editor wraps around the product description, so the card shows readable words instead of markup |
| `truncate` | `snippets/product-card.liquid` | Cuts the stripped description to 90 characters with an ellipsis, so every card in a row ends at the same height no matter how long the original description runs |

The price display uses `money` as the terminal formatter on both the per exposure figure
and the combined total. It is not counted among the five, because Horizon already applies
`money` throughout `snippets/price.liquid` and claiming it would be claiming existing theme
code. The five listed above are all filters added by this assignment.

`plus` and `divided_by` are applied before `money` on purpose. Money formatting returns a
string with a currency symbol in it, and arithmetic filters cannot operate on that, so all
the maths has to finish while the value is still a plain integer.

`strip_html` is applied before `truncate` for the same ordering reason. Stripping first
means the 90 character limit counts words a customer can read. Truncating first would count
the tags, so a description opening with a long tag could be cut down to almost no visible
text, and the cut could land in the middle of a tag and leave it unclosed.

## Conditional Logic

**Driving property:** `product.selected_or_first_available_variant.inventory_quantity`,
assigned to the variable `fc_stock` at the top of the panel.

**File:** `sections/product-information.liquid`, inside the Shooter's Summary panel.

**Branches:**

1. `fc_stock <= 0` renders a struck through out of stock line offering to hold one from the
   next delivery.
2. `fc_stock <= 5` renders a low stock warning that prints the exact remaining count, with a
   dot marker before the text.
3. Anything above 5 renders a plain in stock and shipping today line.

Three branches rather than the two the brief requires, because the middle one is the
commercially useful case. A customer who sees a number is being told something a plain in
stock badge cannot tell them.

**Requirement worth noting:** the variant must have inventory tracking switched on in the
admin. Shopify reports an inventory quantity of zero for untracked variants regardless of
real stock, which would pin the output to the sold out branch permanently.

## Verification Notes

Both pages were opened in the local preview at `http://127.0.0.1:9292` with
`shopify theme dev` running, and each change was confirmed on screen rather than only read
in the editor.

**Product page.** The Shooter's Summary panel renders above the main product layout with
the thumbnail, the per exposure figure, and the combined total line. The per exposure
figure was checked by hand against the variant price to confirm the handling amount was
included before the division rather than after.

**Collection page.** Product cards render the shortened description underneath the existing
card content. Cards whose descriptions run past the limit end in an ellipsis, and cards
with short descriptions show the whole thing with no ellipsis, which confirms `truncate` is
cutting rather than padding. No HTML tags appear as visible text on any card, which
confirms `strip_html` runs first.

**Conditional branches, each triggered in the admin by editing the inventory quantity on
the selected variant and reloading the preview:**

1. Inventory set to 0 produced the struck through out of stock line.
2. Inventory set to 3 produced the low stock line reading three left on the shelf, with the
   number matching what was entered.
3. Inventory set to 25 produced the in stock and shipping today line.

The middle branch was checked twice with different numbers to confirm the count in the
sentence is being read from the variant rather than hardcoded.

## Day 2 Stretch B — Metafield-Shaped Values

Two values in `sections/product-information.liquid` are hardcoded placeholders standing in
for structured data that does not exist yet.

`fc_exposures` is fixed at 36, the frame count on a standard 35mm roll. This genuinely
varies: 120 medium format gives 12 or 16 frames depending on the camera back, short rolls
come in 24, and bulk loaded rolls are whatever the loader decided. Once Day 4 covers
metafields this becomes a product level integer, and the assign line changes to read from
it while every use of the variable underneath stays as written.

`fc_handling` is fixed at 1500 cents. That is a shop wide figure rather than a per product
one, so it belongs in theme settings rather than a metafield, and would be swapped for a
`settings.` reference.

Both were written as single assigns at the top of the block specifically so that the swap
is a one line change in each case, rather than a hunt through the markup for every place
the number appears.

## Day 2 Submission Checklist

**Part 1 — Written Decisions**

1. Five distinct filters listed with target file and effect. Done, table above.
2. Conditional plan names a real object property and every branch's output. Done.

**Parts 2 and 3 — Section Edits**

1. Product page carries three filters and the custom conditional, all branches verified.
2. Collection page brings the combined filter count to five, all distinct.
3. No edits made inside `templates/`.

**Part 4 — Verification and Git**

1. Local preview confirms all edits render correctly.
2. Changes committed and pushed to the `Assignment1.2_LiquidFundamentals` branch.