# Software Requirements Specification

## Standalone Slide Deck Web Application — "SlideDown"

**Document ID:** SRS-SLIDEDOWN-001  
**Version:** 0.1 DRAFT  
**Date:** 2026-04-17  
**Status:** For Review  
**Standard Reference:** Structure informed by ISO/IEC/IEEE 29148:2018

---

## 1. Introduction

### 1.1 Purpose

This document specifies the requirements for a standalone, offline-capable
slide-deck presentation application delivered as a single HTML file. The
application is intended for professional presentations where network access
cannot be guaranteed.

### 1.2 Scope

The product — working name **"SlideDown"** — is a zero-dependency,
self-contained web application. It shall be usable by opening a single `.html`
file in any modern browser. No build step, web server, or internet connection
is required at runtime.

A proof-of-concept (POC) deck of approximately 10 slides shall be delivered
alongside the application to demonstrate all features. The POC content argues
the thesis: *"We should have AI build purpose-built applications rather than
training AI to operate existing frameworks like PowerPoint or Google Slides."*

### 1.3 Definitions & Abbreviations

| Term | Definition |
|------|-----------|
| Slide | A single full-viewport page of presentation content |
| Fragment | A sub-element of a slide revealed incrementally (e.g., a bullet point) |
| Presenter View | A secondary window showing speaker notes + controls |
| Overview Mode | A thumbnail grid of all slides for non-linear navigation |
| Print Stylesheet | A CSS `@media print` ruleset that formats slides for PDF output |

### 1.4 References

- ISO/IEC/IEEE 29148:2018 — Systems and software engineering — Life cycle processes — Requirements engineering
- ISO/IEC 25010:2011 — Systems and software quality models
- W3C Fullscreen API specification
- W3C CSS Paged Media Module Level 3

---

## 2. Overall Description

### 2.1 Product Perspective

SlideDown is a standalone replacement for cloud-based presentation tools
(Google Slides, PowerPoint Online) in situations where offline delivery is
required. It has no external dependencies and runs entirely client-side.

### 2.2 Product Functions (Summary)

| ID | Function |
|----|----------|
| F1 | Present slides fullscreen in a web browser |
| F2 | Navigate slides via keyboard, mouse, and touch |
| F3 | Display incremental fragment reveals within slides |
| F4 | Apply configurable per-slide transitions |
| F5 | Open a presenter view with speaker notes in a secondary window |
| F6 | Display a slide overview grid for non-linear navigation |
| F7 | Export the deck to PDF via the browser's print dialog |
| F8 | Operate fully offline from a single HTML file |

### 2.3 User Classes

| User Class | Description |
|------------|-------------|
| Presenter | Delivers the presentation; needs keyboard/clicker nav, fullscreen, presenter notes |
| Author | Edits slide content in the HTML source; needs a clear, maintainable content format |
| Audience | Views the projected output; no direct interaction |

### 2.4 Operating Environment

- **Browsers:** Latest two versions of Chrome, Firefox, Safari, Edge
- **Platforms:** Desktop (Windows, macOS, Linux), Tablet (iOS, Android)
- **Delivery:** Single `.html` file; file:// or http(s):// protocol

### 2.5 Constraints

| ID | Constraint |
|----|-----------|
| C1 | The entire application (markup, styles, scripts, slide content) SHALL be contained in one `.html` file |
| C2 | No external CDN, font, or script references; everything is inlined |
| C3 | No build tools, transpilers, or bundlers required to use the output |
| C4 | Total file size SHOULD remain under 200 KB for the POC deck (excluding embedded images) |

---

## 3. Specific Requirements

### 3.1 Functional Requirements

#### 3.1.1 Slide Rendering

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| FR-RENDER-01 | Each slide SHALL occupy the full viewport at a **16:9** aspect ratio, centered and letterboxed if the viewport differs | MUST |
| FR-RENDER-02 | Slides SHALL use a **dark background with light text** default theme | MUST |
| FR-RENDER-03 | Slide content SHALL be authored as semantic HTML `<section>` elements within the file, each section representing one slide | MUST |
| FR-RENDER-04 | The application SHALL support the following content elements within a slide: headings (h1–h3), paragraphs, unordered/ordered lists, inline code, code blocks, images (base64-embedded), blockquotes, and emphasis/strong text | MUST |
| FR-RENDER-05 | A slide number indicator (e.g., "3 / 10") SHALL be displayed in presentation mode | SHOULD |

#### 3.1.2 Navigation

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| FR-NAV-01 | Pressing **→** or **↓** or **Space** or **Page Down** SHALL advance to the next slide or fragment | MUST |
| FR-NAV-02 | Pressing **←** or **↑** or **Page Up** SHALL go to the previous slide or fragment | MUST |
| FR-NAV-03 | Pressing **Home** SHALL jump to the first slide | MUST |
| FR-NAV-04 | Pressing **End** SHALL jump to the last slide | MUST |
| FR-NAV-05 | Clicking/tapping the right third of the slide SHALL advance; clicking the left third SHALL go back | SHOULD |
| FR-NAV-06 | **Swipe left** on a touch device SHALL advance; **swipe right** SHALL go back | MUST |
| FR-NAV-07 | The current slide index SHALL be reflected in the URL hash (e.g., `#/3`) to allow bookmarking and reload-persistence | SHOULD |
| FR-NAV-08 | Pressing a number key sequence followed by **Enter** SHALL jump to that slide number | SHOULD |

#### 3.1.3 Fullscreen

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| FR-FS-01 | Pressing **F** or **F11** SHALL toggle the browser Fullscreen API | MUST |
| FR-FS-02 | Pressing **Escape** SHALL exit fullscreen (native browser behavior) | MUST |
| FR-FS-03 | A visible "Enter Fullscreen" button SHALL be present on the initial/title slide for discoverability | SHOULD |

#### 3.1.4 Fragments (Incremental Reveals)

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| FR-FRAG-01 | Elements with a designated CSS class (e.g., `class="fragment"`) SHALL be hidden initially and revealed one-at-a-time on forward navigation | MUST |
| FR-FRAG-02 | Reverse navigation SHALL hide the most recently revealed fragment before going to the previous slide | MUST |
| FR-FRAG-03 | Fragment reveal animation SHALL default to a fade-in (opacity 0 → 1 over ~300ms) | SHOULD |
| FR-FRAG-04 | Fragments SHALL support optional ordering via a `data-index` attribute | SHOULD |

#### 3.1.5 Slide Transitions

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| FR-TRANS-01 | The application SHALL support at least three transition types: **none** (instant), **fade**, and **slide** (horizontal push) | MUST |
| FR-TRANS-02 | The default transition SHALL be **fade** | SHOULD |
| FR-TRANS-03 | Individual slides SHALL be able to override the default transition via a `data-transition` attribute on the `<section>` element | MUST |
| FR-TRANS-04 | Transition duration SHALL be configurable (default: 400ms) | SHOULD |

#### 3.1.6 Presenter View

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| FR-PRES-01 | Pressing **S** SHALL open a **presenter view** in a new browser window | MUST |
| FR-PRES-02 | The presenter view SHALL display: (a) the current slide, (b) the next slide preview, (c) speaker notes for the current slide, and (d) an elapsed-time clock | MUST |
| FR-PRES-03 | Speaker notes SHALL be authored in `<aside class="notes">` elements within each slide `<section>` | MUST |
| FR-PRES-04 | Navigation in either the main window or the presenter window SHALL synchronize both windows via `BroadcastChannel` or equivalent | MUST |
| FR-PRES-05 | The presenter view SHALL remain functional even if the main window enters fullscreen | SHOULD |

#### 3.1.7 Overview Mode

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| FR-OV-01 | Pressing **O** or **G** SHALL toggle a thumbnail grid overview of all slides | MUST |
| FR-OV-02 | Clicking a thumbnail SHALL navigate to that slide and exit overview mode | MUST |
| FR-OV-03 | The current slide SHALL be visually highlighted in the overview grid | SHOULD |
| FR-OV-04 | Arrow keys SHALL navigate between thumbnails while in overview mode | SHOULD |

#### 3.1.8 PDF Export

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| FR-PDF-01 | The file SHALL include a CSS `@media print` stylesheet that renders each slide as a separate page | MUST |
| FR-PDF-02 | Printed slides SHALL use the **same 16:9** aspect ratio, sized for landscape A4/Letter | MUST |
| FR-PDF-03 | All fragments SHALL be fully revealed in print output | MUST |
| FR-PDF-04 | Speaker notes SHALL be excluded from default print output | MUST |
| FR-PDF-05 | A keyboard shortcut hint (e.g., "Ctrl+P for PDF") SHALL be documented in a help overlay | SHOULD |
| FR-PDF-06 | Background colors and graphics SHALL print correctly (using `-webkit-print-color-adjust: exact` and equivalent) | MUST |

#### 3.1.9 Help Overlay

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| FR-HELP-01 | Pressing **?** or **H** SHALL toggle a help overlay listing all keyboard shortcuts | SHOULD |
| FR-HELP-02 | The help overlay SHALL be dismissible with **Escape** or clicking outside it | SHOULD |

---

### 3.2 Non-Functional Requirements

#### 3.2.1 Performance

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| NFR-PERF-01 | Initial render to first slide visible SHALL complete in < 500ms on a mid-range 2024 laptop | MUST |
| NFR-PERF-02 | Slide transitions SHALL maintain 60 fps | SHOULD |

#### 3.2.2 Portability

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| NFR-PORT-01 | The application SHALL function identically when loaded via `file://` protocol (no CORS/CSP issues) | MUST |
| NFR-PORT-02 | The application SHALL function without JavaScript disabled, degrading gracefully to a scrollable document | SHOULD |

#### 3.2.3 Accessibility

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| NFR-A11Y-01 | Slide content SHALL be navigable via screen reader using ARIA landmarks and roles | SHOULD |
| NFR-A11Y-02 | Color contrast ratios SHALL meet WCAG 2.1 AA (≥ 4.5:1 for body text) | SHOULD |

#### 3.2.4 Maintainability

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| NFR-MAINT-01 | Slide content SHALL be clearly separated from application logic within the HTML file (content section vs. script/style sections) | MUST |
| NFR-MAINT-02 | Adding a new slide SHALL require only adding a new `<section>` element — no changes to JavaScript | MUST |
| NFR-MAINT-03 | The CSS theme SHALL be defined in CSS custom properties (variables) at the `:root` level, allowing easy re-theming | SHOULD |

#### 3.2.5 Security

| Req ID | Requirement | Priority |
|--------|-------------|----------|
| NFR-SEC-01 | The file SHALL make no network requests of any kind | MUST |
| NFR-SEC-02 | No inline `eval()` or dynamic script loading SHALL be used | MUST |

---

## 4. Content Specification — POC Deck

### 4.1 Thesis

> *"Rather than training AI to operate complex existing frameworks (PowerPoint,
> Google Slides, Keynote), we should have AI generate purpose-built,
> self-contained applications that do exactly what's needed — nothing more,
> nothing less."*

### 4.2 Slide Outline (≈10 slides)

| # | Title | Key Points | Notes |
|---|-------|------------|-------|
| 1 | **Title Slide** | "Build the App, Not the Adapter" — subtitle, author, date | Fullscreen button visible |
| 2 | **The Status Quo** | AI tools wrap around PowerPoint/Slides APIs; fragile, version-dependent, limited | Fragment reveals for each pain point |
| 3 | **The Abstraction Tax** | Every framework imposes its own object model, file format, plugin ecosystem; AI must learn and re-learn each one | Diagram or visual metaphor |
| 4 | **What We Actually Need** | Enumerate real requirements: show slides, navigate, export PDF — that's it | Minimal requirements list |
| 5 | **The Alternative: AI as App Builder** | AI generates a single HTML file; the "framework" is the browser, the most universal runtime on earth | Key insight slide |
| 6 | **Benefits: Zero Dependencies** | No install, no version conflicts, no license fees, works offline, works forever (HTML is eternal) | Fragment list |
| 7 | **Benefits: Total Control** | Every pixel, transition, and interaction is yours; no fighting the framework's opinions | Contrast with PowerPoint limitations |
| 8 | **Benefits: Composability** | Need a live chart? Embed `<canvas>`. Need a quiz? Add 20 lines of JS. Need 3D? Import a WebGL snippet. | Show code examples or interactive elements |
| 9 | **But What About…?** | Address objections: collaboration, templates, non-technical users, brand guidelines | Fragment reveals for Q&A style |
| 10 | **Conclusion / Call to Action** | "Stop adapting AI to tools. Start adapting tools to the task." — next steps | Strong closing statement |

---

## 5. Delivery Artifacts

| Artifact | Description |
|----------|-------------|
| `slideshow.html` | The complete application + POC content in a single file |
| `REQUIREMENTS.md` | This document |
| `README.md` | Brief usage instructions (keyboard shortcuts, how to edit, how to export PDF) |

---

## 6. Acceptance Criteria

The delivery is accepted when:

1. `slideshow.html` opens in Chrome, Firefox, and Safari (latest) via `file://` and renders correctly
2. All FR-\* MUST requirements are demonstrably met
3. PDF export via `Ctrl+P` → "Save as PDF" produces a correctly paginated landscape document
4. The POC deck contains ≈10 content slides as specified in §4.2
5. Speaker notes are visible in presenter view for at least 5 slides
6. The file is ≤ 200 KB (no embedded images in the POC)

---

## 7. Open Questions

| # | Question | Status |
|---|----------|--------|
| OQ-1 | Should the app support embedded video (base64 or blob)? | Deferred |
| OQ-2 | Should we support a "notes-only" print mode for handouts? | Deferred |
| OQ-3 | Should there be a built-in slide-editing UI, or is source-editing sufficient? | Source-editing for v1 |
| OQ-4 | Do we want a configurable light theme option in addition to dark? | Deferred — CSS vars make it easy to add later |
| OQ-5 | Should we support syntax-highlighted code blocks? | TBD — can inline a small highlighter at cost of file size |

---

*End of document.*
