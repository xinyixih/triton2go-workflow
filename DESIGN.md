# Triton Dining — UI Design System

> This file defines the visual language, components, and conventions for all screens in the Triton Dining app. Every new HTML mockup or showcase page should follow these rules.

---

## 1 · Color Palette

| Token | Hex | Usage |
|---|---|---|
| `--navy` | `#182B49` | Primary background, status bar, nav bar, headers |
| `--navy-dark` | `#1a1a2e` | Page/canvas background |
| `--orange` | `#E67A46` | Primary CTA buttons, active accents, icon backgrounds |
| `--orange-light` | `#FECBB7` | Icon container background (soft tint) |
| `--yellow` | `#FFCD00` | Center FAB button, highlight |
| `--cream` | `#FAF8F5` | Screen/card background |
| `--card-bg` | `#FFFFFF` | Card surface |
| `--gray-text` | `#747678` | Secondary/subtitle text |
| `--border` | `#EAE6E1` | Card borders, dividers |

```css
:root {
  --navy:        #182B49;
  --navy-dark:   #1a1a2e;
  --orange:      #E67A46;
  --orange-light:#FECBB7;
  --yellow:      #FFCD00;
  --cream:       #FAF8F5;
  --gray-text:   #747678;
  --border:      #EAE6E1;
}
```

---

## 2 · Typography

| Role | Font | Weight | Size (full px) |
|---|---|---|---|
| Screen title | Source Serif Pro | 600 | 85px (in 1499px canvas) |
| Section heading | Source Sans Pro | 700 | 55px |
| Body / card title | Source Sans Pro | 600 | 55px |
| Subtitle | Source Sans Pro | 400 | 45px |
| Nav label | Source Sans Pro | 600 | 35px |
| Status bar time | -apple-system | 600 | 15px (real px) |
| Overlay title | Source Sans Pro | 700 | 13px (real px) |

> All profile-canvas text uses original Figma px values (1499px coordinate space). They are visually scaled via `transform: scale()`.

**Google Fonts import:**
```html
<link href="https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@400;600;700&family=Source+Serif+Pro:wght@600&display=swap" rel="stylesheet">
```

---

## 3 · iPhone 16 Pro Max Mockup Frame

### Dimensions
| Property | Value |
|---|---|
| Frame width | `260px` |
| Frame height | `607px` (fixed, not auto) |
| Frame ratio | `9 : 19.5` (width : height) — matches real device |
| Padding (bezel) | `6px` |
| Screen width | `248px` (260 − 2×6) |
| Screen height | `595px` (607 − 12) |
| Border radius (frame) | `46px` |
| Border radius (screen) | `40px` |

### CSS
```css
.iphone {
  position: relative;
  width: 260px;
  background: #1c1c1e;
  border-radius: 46px;
  box-shadow:
    0 0 0 1px #6a6a6c,
    0 0 0 2px #3a3a3c,
    0 0 0 8px #242424,
    0 0 0 9.5px #4a4a4c,
    0 24px 70px rgba(0,0,0,0.75),
    0 6px 20px rgba(0,0,0,0.5);
  padding: 6px;
  flex-shrink: 0;
}

/* Left side buttons (mute + volume) */
.iphone::before {
  content: '';
  position: absolute;
  left: -11px; top: 93px;
  width: 3px; height: 25px;
  background: #3a3a3c;
  border-radius: 2px 0 0 2px;
  box-shadow: 0 34px 0 #3a3a3c, 0 68px 0 #3a3a3c;
}
/* Right side button (power) */
.iphone::after {
  content: '';
  position: absolute;
  right: -11px; top: 123px;
  width: 3px; height: 52px;
  background: #3a3a3c;
  border-radius: 0 2px 2px 0;
}

.screen-bezel {
  width: 100%;
  border-radius: 40px;
  overflow: hidden;
  background: #182B49; /* navy so scroll-reveal has no black gap */
  position: relative;
}
```

### Profile content scaling
The profile canvas is **1499px wide** (Figma export). It is scaled into the 248px screen:

```
scale = 248 / 1499 = 0.1655
```

```css
.profile-wrapper {
  width: 1499px;
  transform-origin: top left;
  transform: scale(0.1655);
  height: 3258px; /* original Figma height */
}

.profile-scale-container {
  width: 248px;
  height: 485px; /* 2929 × 0.1655 — content height minus tab bar */
  position: relative;
}
```

### Scroll area
```css
.phone-scroll {
  overflow-y: scroll;
  overflow-x: hidden;
  height: 505px; /* 595 screen − 36 status − 54 tab */
  scrollbar-width: none;
}
```
> On load, set `phoneScroll.scrollTop = 28` to trim the top navy gap.

---

## 4 · Component: Status Bar

Full reference in `_components.html → ① Status Bar`.

### Layout
3-column flex: **[time zone · flex:1]** | **[island spacer · 82px]** | **[icons zone · flex:1]**

```html
<div class="status-bar">
  <div class="dynamic-island"></div>

  <div style="flex:1; display:flex; justify-content:center; align-items:center;">
    <span class="status-time">9:41</span>
  </div>

  <div style="width:82px; flex-shrink:0;"></div><!-- spacer = island width -->

  <div style="flex:1; display:flex; justify-content:center; align-items:center;">
    <div class="status-icons">
      <!-- Signal SVG · WiFi SVG · Battery SVG (see _components.html) -->
    </div>
  </div>
</div>
```

### CSS
```css
.status-bar {
  height: 36px;
  background: #182B49;
  display: flex;
  align-items: center;
  padding: 6px 0 0;
  position: relative;
  flex-shrink: 0;
}

.dynamic-island {
  position: absolute;
  top: 8px; left: 50%;
  transform: translateX(-50%);
  width: 82px; height: 24px;
  background: #000;
  border-radius: 12px;
  z-index: 10;
}

.status-time {
  color: #fff;
  font-size: 10px;
  font-weight: 600;
  font-family: -apple-system, sans-serif;
}

.status-icons { display: flex; align-items: center; gap: 4px; }
.status-icons svg { fill: white; opacity: 0.9; }
```

### Icons (SVG)
All icons use `fill="white"` on a navy background. See `_components.html` for full SVG paths for:
- **Signal bars** — 4 rects with graduated opacity (0.4 → 1.0)
- **WiFi** — 2 arcs + filled dot
- **Battery** — outlined rect + filled rect + nub rect

---

## 5 · Component: Bottom Nav Bar

Full reference in `_components.html → ② Bottom Nav Bar`.

### Structure
5 items in a horizontal flex row. Center item has a **yellow circle FAB** that lifts above the bar.

```
[ Home ]  [ Map ]  [ ● FAB ]  [ Rewards ]  [ Profile ]
```

### Icons (SVG · viewBox 0 0 110 110)
All icons are **stroke-based**, `stroke-linecap="round"`, `stroke-linejoin="round"`, `stroke-width="6"` (in 110px space).

| Tab | Icon shape | Stroke color |
|---|---|---|
| Home | House outline + roof path | `rgba(255,255,255,0.75)` |
| Map | Teardrop pin + inner circle | `rgba(255,255,255,0.75)` |
| Center FAB | Chat bubble + 3 filled dots | `#182B49` (on yellow) |
| Rewards | 5-point star outline | `rgba(255,255,255,0.75)` |
| Profile | Circle head + arc body | `white` (active tab) |

### CSS variables
```css
:root {
  --nav-bg:       #182B49;
  --nav-active:   #ffffff;
  --nav-inactive: rgba(255,255,255,0.55);
  --nav-accent:   #FFCD00;
}
```

### Scaling in mockup
The nav bar inner canvas is `1499 × 329px`, scaled at `0.1655` to render at `248 × 54px`.

```css
.tab-bar-fixed {
  position: absolute;
  bottom: 0; left: 0;
  width: 100%; height: 54px;
  overflow: hidden;
  z-index: 15;
}
.tab-bar-fixed .tab-bar-inner {
  width: 1499px; height: 329px;
  transform-origin: top left;
  transform: scale(0.1655);
}
```

---

## 6 · Overlay System

Three overlay types used on top of the profile screen:

| ID | Type | Trigger |
|---|---|---|
| `#overlay1` | Bottom sheet (slide up) | Tap Upload button |
| `#overlay2` | Center modal (pop in) | Tap upload option |
| `#overlay3` | Center modal — Edit form | Tap Edit in review |

### CSS pattern
```css
.overlay-backdrop {
  display: none;
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.45);
  z-index: 20;
  border-radius: 32px; /* match screen-bezel radius */
}
.overlay-backdrop.active { display: flex; align-items: flex-end; }
.overlay-backdrop.center { align-items: center; justify-content: center; }
```

### JS flow
```js
function openUpload() { closeAll(); overlay1.classList.add('active'); setStep(2); }
function openReview() { overlay1.classList.remove('active'); overlay2.classList.add('active','center'); setStep(3); }
function openEdit()   { overlay2.classList.remove('active','center'); overlay3.classList.add('active','center'); setStep(4); }
function closeAll()   { ['overlay1','overlay2','overlay3'].forEach(id => document.getElementById(id).classList.remove('active','center')); setStep(1); }
```

---

## 7 · Page Layout (Showcase)

```
body { display: flex; align-items: center; justify-content: center; height: 100vh; overflow: hidden; }

.left-panel   → position: absolute; left: 80px; — never displaces iPhone
.iphone       → sole flex child → always centered
```

Background gradient:
```css
body {
  background: #1a1a2e;
  background-image: radial-gradient(ellipse at 30% 20%, #16213e 0%, #0f3460 50%, #1a1a2e 100%);
}
```

---

## 8 · Icons on Profile Cards

Cards use **Apple emoji** at `font-size: 80px` in the Figma coordinate space (visible at ~13px after scale).

| Card | Emoji |
|---|---|
| Class Schedule | 📅 |
| Payment Method | 💳 |
| Dinning Support | 🎧 |
| Nutrition & Allergens | 🥗 |
| Dinning Service Hiring | 💼 |

---

## 9 · File Structure

```
profile workflow/
├── SKILL.md              ← this file (design system)
├── _components.html      ← copy-paste reference for Status Bar + Nav Bar
├── showcase.html         ← main flow showcase (iPhone mockup + overlays)
├── profile.html          ← original Figma export (1499px canvas)
└── screens.html          ← (other screens reference)
```

---

## 10 · Quick Checklist for New HTML Mockups

- [ ] Import Source Sans Pro + Source Serif Pro from Google Fonts
- [ ] Use color tokens from §1
- [ ] iPhone frame: 260×607px, padding 6px, border-radius 46px
- [ ] Screen bezel background: `#182B49` (not black — prevents scroll gap)
- [ ] Status bar: 3-column flex layout with Dynamic Island spacer
- [ ] Nav bar: SVG icons 110×110 viewBox, stroke-width 6, round caps
- [ ] Center FAB: yellow `#FFCD00` circle, navy chat-bubble SVG
- [ ] Overlays: `border-radius: 32px` on backdrop to clip to screen corners
- [ ] `scrollTop = 28` on phone-scroll to trim top navy gap on load
- [ ] Profile canvas scale: `scale = screen_width / 1499`
