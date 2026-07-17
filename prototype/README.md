# TagBin — NFC Storage Tracker · Mobile UI Prototype

A high-fidelity, **clickable** mobile UI prototype for the NFC Storage Tracker app,
designed as a starting point for the product. It's a single, self-contained HTML file —
no build step, no dependencies, no network requests. Just open it.

> **Status:** design prototype only. This is a visual/interaction mock to align on the
> product experience before any engineering. No real NFC, backend, or persistence.

## How to open

Open `prototype/index.html` in any modern browser (or drag it onto a browser window).
It renders inside a phone frame.

- **Tap around** — it's fully interactive: tap a bin to open it, tap the center **Scan**
  button to run the scan flow, use the bottom tab bar to move between sections.
- **Screens** button (top right) — jump directly to any screen for review.
- **Dark / Light** toggle (top left, and inside Profile → Appearance).

For the intended feel, narrow your browser to a phone width (~390px) or use device
emulation in DevTools.

## The concept

TagBin helps you track what's stored in physical bins and boxes around your home.
Each bin has a cheap **NFC sticker**. Tap your phone to a bin to instantly see what's
inside, and search *"where is my…?"* to find which box (and room) any item lives in.

## Product model the UI is designed around

A simple, legible hierarchy:

| Entity | What it is | Key fields |
| --- | --- | --- |
| **Location** | A room / area | name, icon, color, bin count |
| **Container** (bin) | A physical box carrying **one NFC tag** | name, cover, location, `nfcTagUid`, item count, last-scanned |
| **Item** | A thing inside a bin | name, photo, quantity, category, container, notes, date added |
| **Activity** | Recent events for the home feed | scanned / added / moved / paired / edited |

> The **NFC tag identifies the container**, not individual items — you tag boxes, then
> catalog their contents inside the app. (Nesting is intentionally kept to one level:
> Location → Container → Item.)

## Screens included

1. **Home / Dashboard** — greeting, prominent *Scan a bin* CTA, search, stat tiles
   (items / bins / locations), recently-scanned bins, recent activity feed.
2. **Scan (NFC)** — full-screen scanning state with an animated pulse, plus two
   simulate buttons: a successful scan and an *unrecognized tag* branch.
3. **Scan · success** — confirmation with the matched bin, then *Open bin →*.
4. **Container detail** — hero cover, location + tag chips, stats, searchable contents
   list, add-item FAB, re-scan / share actions.
5. **Item detail** — photo, quantity stepper, category, *located in* breadcrumb,
   notes, move / delete actions.
6. **Add item** — sheet: photo, name, category picker, quantity, bin picker, notes.
7. **Search** — search all items with suggestions and category browse; results answer
   *"where is it"* (item → *Power Tools · Garage*).
8. **Browse** — locations list + a grid of all bins.
9. **Location detail** — a room and the bins in it.
10. **Pair a new tag** — reached from an unrecognized scan: name a bin, choose a
    location + cover, write & pair the tag.
11. **Profile / Settings** — user, stats, appearance (dark mode), storage & general.

Every screen is styled for both **light and dark** themes.

## Design system

Defined once as CSS tokens at the top of `index.html` and reused across all screens:

- **Accent:** indigo → violet primary; neutral gray ramp for surfaces and text.
- **Type:** native system font stack (SF-like on iOS) with a clear scale.
- **Shape/space:** 12–28px radii, 16–20px gutters, soft shadows / elevated dark surfaces.
- **Components:** bottom tab bar (raised center Scan), cards, list rows, chips, FAB,
  sheets, search field, stat tiles, segmented control, toggles.
- **Accessibility:** legible contrast in both themes; tap targets ≥ 44px.

Retune the whole look by editing the `:root` (and `:root[data-theme="dark"]`) token
blocks — accent, radii, and surface colors flow through every screen.

## Accessibility (built to WCAG 2.2 AA)

The prototype is designed to conform to WCAG 2.2 Level AA and to model accessible
patterns for the real app:

- **Perceivable** — every text/UI color pair meets **≥ 4.5:1** contrast in *both* light
  and dark themes (verified programmatically against the computed tokens). Meaningful
  emoji tiles expose a text alternative (`role="img"` + `aria-label`); decorative emoji
  and all inline SVG icons are `aria-hidden`. Category identity is carried by a colored
  dot **plus** a text label, never color alone (SC 1.4.1).
- **Operable** — the whole UI is **keyboard-operable**: rows, cards, and the scan CTA are
  real controls (native `<button>` or `role="button"` + Enter/Space handling), the tab
  order is logical, and a visible **`:focus-visible`** ring appears for keyboard users
  (a white + color double-ring over gradients). All targets are **≥ 44×44px**
  (SC 2.5.8). On navigation, focus moves to the new screen and its name is announced.
- **Understandable** — one real `<h1>` per screen with a proper heading outline;
  landmarks (`<main>`, `<nav aria-label="Primary">`, `<header>`); form fields have
  programmatic labels; the category picker is a keyboard **`radiogroup`** (arrow keys);
  quantity steppers announce their value via `aria-live`; the modal sheets use
  `role="dialog"`/`aria-modal` with a real **focus trap** (Tab wraps inside the dialog,
  the background stage is `inert`) and **Escape** closes them.
- **Robust** — icon-only buttons all have `aria-label`s; the dark-mode control is a real
  `role="switch"` with `aria-checked`; the active tab is marked `aria-current="page"`;
  toasts and result counts announce through polite/assertive live regions.
- **Motion & scaling** — `prefers-reduced-motion` disables the scan pulse, sheet slide,
  and fades. Text uses **rem** units and reflows without clipping or horizontal scroll at
  **200% zoom**.

## Interaction / UX simplifications

- **One primary action per screen** — the container header's duplicate "+ Add" link was
  removed; the thumb-reachable FAB is the single "Add item" action.
- **First-run guidance** — a dismissible tip on Home points to the Scan action.
- **Richer empty states** — an empty bin (*Seasonal Storage*) and an empty location
  (*Garden Shed*) show friendly, actionable empty states.
- **Clearer search** — Home's search is a labelled button that opens Search, which now
  announces a live result count.

## Design assumptions (open for discussion)

- Single household / personal storage (a light "share with household" is shown in Profile).
- Tag = container; one level of nesting (Location → Container → Item).
- Indigo/violet brand accent and the name "TagBin" are placeholders.
- Item "photos" are represented by emoji/color tiles so the prototype stays offline and
  self-contained; a real app would use captured photos.

## Not included (intentionally)

Real NFC read/write, authentication, storage, and networking are out of scope for this
prototype — it exists to validate the **experience and visual language**. Adding items
and pairing bins *do* update the in-session data (contents, counts, activity feed) so
the flows feel real end-to-end, but nothing persists across a page reload.
