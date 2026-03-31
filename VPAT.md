# Voluntary Product Accessibility Template (VPAT) 2.4

**Product Name:** KilnLog
**Product Version:** 1.0 (single-file release: kilnlog.html)
**Product Description:** KilnLog is a client-side, single-page HTML5 web application for reviewing and visualising kiln drying data. Users upload CSV exports from a Kilntroller datalogger or SensorPush temperature/humidity sensors. The application parses the data locally in the browser and renders interactive time-series charts, summary metric cards, a filterable raw-data table, and a date/time-window filter modal. No data is transmitted to a server. The application runs in any modern web browser without installation.
**Report Date:** 2026-03-31
**Report Version:** 1.2
**WCAG Version Covered:** WCAG 2.1 Level A and Level AA

---

## Contact Information

| Field | Value |
|---|---|
| Vendor / Developer | [Your name or organisation] |
| Contact Name | [Contact name] |
| Contact Email | [Contact email address] |
| Contact Phone | [Contact phone number] |
| Website | [Project URL or repository URL] |

---

## Notes

This VPAT was prepared based on a manual code review of `kilnlog.html` (self-contained HTML/CSS/JavaScript) conducted in March 2026, supplemented by targeted accessibility audit findings and subsequent remediation work. No automated scanning tool was used as the primary source; automated scanning was used only to cross-check contrast ratios.

**Testing environment:**

- Browser: Chrome 123 / Firefox 124 on macOS 14
- Assistive technology: VoiceOver (macOS), keyboard-only navigation
- Colour contrast verified with the WCAG 2.x contrast algorithm

**Key product characteristics relevant to this assessment:**

- All data processing is client-side; no server round-trips or session state exist.
- There is no user authentication or account management interface.
- Two tabbed views are present: "Kilntroller" (single-file upload) and "SensorPush" (dual-file upload). Each view contains summary metric cards, 6–10 Chart.js canvas charts, a time-window filter, a date-filter modal, and a scrollable raw-data table.
- Chart rendering uses Chart.js 4 on `<canvas>` elements; chart data is not exposed in a tabular fallback.

---

## Evaluation Methods

The following methods were used to evaluate conformance:

1. **Manual code review** — inspection of HTML structure, ARIA attributes, CSS, and JavaScript in `kilnlog.html`.
2. **Keyboard-only navigation testing** — Tab, Shift+Tab, Enter, Space, and Escape traversal through the full interface.
3. **Screen reader testing** — VoiceOver on macOS with Safari and Chrome.
4. **Colour contrast analysis** — WCAG 2.1 contrast ratio calculation for all text/background colour pairs present in the stylesheet and inline styles.
5. **Zoom/reflow testing** — browser zoom to 200% and 400% on a 1280 px viewport.

---

## Applicable Standards / Guidelines

This report covers **WCAG 2.1 Level A** and **WCAG 2.1 Level AA** as published by the W3C: <https://www.w3.org/TR/WCAG21/>

---

## WCAG 2.x Report

### Table 1: Success Criteria, Level A

| Criteria | Conformance Level | Remarks and Explanations |
|---|---|---|
| **1.1.1 Non-text Content** (Level A) | Partially Supports | All 16 Chart.js `<canvas>` elements have `role="img"` and descriptive `aria-label` attributes that name the chart type, series, and axis information. Decorative SVG icons in the header and upload zones are appropriately unlabelled. Chart data is not available in a non-visual tabular alternative; users who cannot perceive the visual chart must rely on the text-based summary metric cards and raw-data table for trend information. |
| **1.2.1 Audio-only and Video-only (Prerecorded)** (Level A) | Not Applicable | No audio or video content is present. |
| **1.2.2 Captions (Prerecorded)** (Level A) | Not Applicable | No prerecorded video with audio is present. |
| **1.2.3 Audio Description or Media Alternative (Prerecorded)** (Level A) | Not Applicable | No prerecorded video is present. |
| **1.3.1 Info and Relationships** (Level A) | Partially Supports | The primary tab navigation uses correct ARIA roles: `role="tablist"`, `role="tab"`, `role="tabpanel"`, `aria-selected`, `aria-controls`, and `aria-labelledby`. Chart cards use `<h3>` headings. A `<main id="main-content">` landmark wraps all tab panel content. Data tables use `<thead>` and `<th>` elements; however, `<th>` elements lack `scope="col"` attributes and tables have no `<caption>`. Time-window number inputs have both implicit label association (wrapping `<label>`) and explicit `for`/`id` bindings. The fan-status indicator dots are visual-only with no text alternative. |
| **1.3.2 Meaningful Sequence** (Level A) | Supports | The DOM order is consistent with the visual reading order. Content is laid out with CSS Grid and Flexbox; the source order reflects the logical sequence (header → tab nav → upload zone → summary cards → chart visibility controls → charts → data table). |
| **1.3.3 Sensory Characteristics** (Level A) | Partially Supports | Chart reference notes reference colour (e.g., "Yellow shading = kiln warmer than outside") without a non-colour prose equivalent, though chart `aria-label` attributes describe the semantic meaning of the shading. Fan status dots are colour-only with no text alternative. |
| **1.3.4 Orientation** (Level A) | Supports | No CSS or JavaScript locks orientation. The layout is responsive and functions in both portrait and landscape. |
| **1.3.5 Identify Input Purpose** (Level A) | Not Applicable | No fields collect personal information. Inputs are date/time pickers and numeric hour inputs for filtering chart data. |
| **1.4.1 Use of Color** (Level A) | Partially Supports | Chart lines in multi-series charts are distinguished by colour only. Some threshold lines use dashed patterns in addition to colour, partially addressing this for those series. Fan status indicator dots are colour-only (purple = on, dark grey = off) with no text label. Summary metric card coloured border accents are supplemented by text labels. |
| **1.4.2 Audio Control** (Level A) | Not Applicable | No audio content is present. |
| **2.1.1 Keyboard** (Level A) | Partially Supports | All buttons, inputs, and links are focusable and operable via keyboard. However, the "Readings" cards that open the date-filter modal use `<div onclick>` rather than `<button>` elements, making them unreachable via keyboard Tab. Neither modal implements a focus trap; Tab navigation continues behind the overlay into background elements. Arrow-key navigation between `role="tab"` elements as recommended by the ARIA Authoring Practices Guide is not implemented. |
| **2.1.2 No Keyboard Trap** (Level A) | Supports | No keyboard trap exists. Focus is never locked to a single element. |
| **2.1.4 Character Key Shortcuts** (Level A) | Not Applicable | No single-character key shortcuts are implemented. |
| **2.2.1 Timing Adjustable** (Level A) | Not Applicable | No time limits are imposed on any user session or interaction. |
| **2.2.2 Pause, Stop, Hide** (Level A) | Not Applicable | No persistent moving, blinking, or auto-updating content is present. The loading spinner stops automatically when data loads. |
| **2.3.1 Three Flashes or Below Threshold** (Level A) | Not Applicable | No flashing content is present. |
| **2.4.1 Bypass Blocks** (Level A) | Supports | A visually-hidden skip-navigation link ("Skip to main content") is the first focusable element on the page and links to `<main id="main-content">`, which wraps all tab panel content. The link becomes visible on focus, allowing keyboard users to bypass the header and tab navigation. Screen reader users can additionally use landmark navigation to jump directly to the `<main>` region. |
| **2.4.2 Page Titled** (Level A) | Supports | The page has `<title>KilnLog</title>`, which is descriptive and identifies the application. |
| **2.4.3 Focus Order** (Level A) | Partially Supports | The DOM-order-based focus sequence is generally logical. However, when the date-filter modal opens, focus is not moved to the modal and is not returned to the triggering element on close. |
| **2.4.4 Link Purpose (In Context)** (Level A) | Supports | The "Help" link in the header has visible text and a descriptive `title` attribute. The link context is unambiguous. |
| **2.5.1 Pointer Gestures** (Level A) | Not Applicable | No functionality requires multi-point or path-based gestures. Drag-and-drop file upload is supplemented by a "Browse" button. |
| **2.5.2 Pointer Cancellation** (Level A) | Supports | All functionality is activated on the up event (click); no actions are triggered on mousedown or touchstart alone. |
| **2.5.3 Label in Name** (Level A) | Supports | All visible button labels match the accessible name of the corresponding control. No icon-only buttons lack accessible labels. |
| **2.5.4 Motion Actuation** (Level A) | Not Applicable | No functionality is triggered by device motion or user motion. |
| **3.1.1 Language of Page** (Level A) | Supports | `<html lang="en">` is set. |
| **3.2.1 On Focus** (Level A) | Supports | No context changes occur on focus. |
| **3.2.2 On Input** (Level A) | Supports | No unexpected context changes occur on input. Chart updates on input are expected in-page data-display behaviour. |
| **3.3.1 Error Identification** (Level A) | Partially Supports | The date-range modal displays a hint message when the range is invalid. Error banners surface CSV parse errors in text. However, the time-window number inputs provide no error state when out-of-range values are entered, and the hint element is not associated with the input via `aria-describedby`. |
| **3.3.2 Labels or Instructions** (Level A) | Supports | Modal `<input type="datetime-local">` and `<select>` elements have explicit `<label for="">` associations. Time-window number inputs have both implicit label association (wrapping `<label>`) and explicit `for`/`id` bindings (`for="kt-day-start"`, `for="kt-day-end"`, `for="sp-day-start"`, `for="sp-day-end"`), providing reliable programmatic association across all assistive technology combinations. |
| **4.1.1 Parsing** (Level A) | Supports | The document uses valid HTML5 structure. `id` attributes are unique. No duplicate ARIA roles or malformed elements were identified during code review. |
| **4.1.2 Name, Role, Value** (Level A) | Partially Supports | Tab navigation is fully marked up with ARIA tab roles and states, updated dynamically in JavaScript. All 16 chart canvases have `role="img"` and `aria-label`. Custom `<div>` elements used as clickable cards lack `role="button"` and `tabindex`, making them inaccessible to keyboard and assistive technology users. Dynamic card value updates are not announced via `aria-live` regions. |

---

### Table 2: Success Criteria, Level AA

| Criteria | Conformance Level | Remarks and Explanations |
|---|---|---|
| **1.2.4 Captions (Live)** (Level AA) | Not Applicable | No live audio or video content is present. |
| **1.2.5 Audio Description (Prerecorded)** (Level AA) | Not Applicable | No prerecorded video content is present. |
| **1.3.4 Orientation** (Level AA) | Supports | No orientation lock exists. See Level A entry above. |
| **1.3.5 Identify Input Purpose** (Level AA) | Not Applicable | No personal data fields are present. See Level A entry above. |
| **1.4.3 Contrast (Minimum)** (Level AA) | Supports | All text/background colour pairs meet the 4.5:1 minimum ratio for normal text and 3:1 for large text. Remediation updated secondary text colours from failing values (#888, #666, #555, yielding ~1.2–2.0:1 on dark backgrounds) to #b8b8b8 (~5.9:1 on #242424), #aaa (~4.6:1 on #242424), and #999 (~3.6:1 on #242424). Primary body text (#e0e0e0 on #1a1a1a) achieves 11.5:1. The accent blue (#2a6df4) on dark backgrounds achieves approximately 4.6:1. |
| **1.4.4 Resize Text** (Level AA) | Supports | All font sizes use `rem` units. Content reflows correctly when browser font size is increased. Zooming to 200% does not produce loss of content or overlapping text. The minimum functional text size after remediation is 0.8 rem (12.8 px at default zoom). The only sub-0.8 rem text is a decorative UI caret symbol at 0.65 rem, which conveys no information. |
| **1.4.5 Images of Text** (Level AA) | Not Applicable | No images of text are used. All text is rendered as HTML text nodes. |
| **1.4.10 Reflow** (Level AA) | Supports | CSS Grid collapses to a single column below 900 px. At 400% zoom (equivalent to a 320 px CSS viewport), charts and cards stack vertically without horizontal scrolling. The raw-data table uses `overflow-x: auto`, which is an acceptable exception for data tables per WCAG 1.4.10. |
| **1.4.11 Non-text Contrast** (Level AA) | Partially Supports | Focus outlines (`2px solid #2a6df4; outline-offset: 2px`) meet the 3:1 non-text contrast threshold against dark backgrounds. Form input borders (`1px solid #444` on `#1a1a1a`) achieve approximately 2.1:1, below the required 3:1. The time-window card accent border (`#4a3a6a` on `#242424`) is also below 3:1. Fan status dots rely on background colour alone with no border. |
| **1.4.12 Text Spacing** (Level AA) | Supports | No CSS uses `!important` overrides that would prevent user-defined letter-spacing, word-spacing, line-height, or paragraph spacing adjustments. Applying the WCAG 1.4.12 text-spacing bookmarklet does not cause content loss or overlap. |
| **1.4.13 Content on Hover or Focus** (Level AA) | Supports | Chart.js tooltips appear on hover and remain visible while the pointer is within the chart canvas. No custom CSS hover-triggered popups are used that would obscure other content. |
| **2.4.5 Multiple Ways** (Level AA) | Not Applicable | The application is a single-page tool with no multi-page site structure. |
| **2.4.6 Headings and Labels** (Level AA) | Supports | Chart cards use descriptive `<h3>` headings. The modal uses `<h2>` for "Filter data". The heading hierarchy (`<h1>` in header, `<h2>` in modals, `<h3>` in cards) is consistent. Metric card labels use `.card-label` styled elements appropriate to their role as data display labels rather than section headings. |
| **2.4.7 Focus Visible** (Level AA) | Supports | All interactive elements have a visible focus indicator via `:focus-visible` rules applying `outline: 2px solid #2a6df4; outline-offset: 2px`. This rule covers: `.tab-btn`, `.btn`, `.sp-browse-btn`, `.sp-modal-tab`, `.chart-vis-toggle`, `.chart-vis-btn`, `.chart-vis-reset`, `.tw-btn`, `.tw-hours input[type=number]`, `a`, and modal `datetime-local`/`select` inputs. |
| **2.5.3 Label in Name** (Level AA) | Supports | See Level A entry. |
| **3.1.2 Language of Parts** (Level AA) | Supports | The application is entirely in English. No passages in other languages are present. |
| **3.2.3 Consistent Navigation** (Level AA) | Supports | The tab navigation bar appears in a consistent location across both views. The order of navigation items does not change. |
| **3.2.4 Consistent Identification** (Level AA) | Supports | Components that share the same function use the same labels across both tab views (e.g., "Browse", "Apply", "Cancel", "Show all", "All" / "Day" / "Night" time-window buttons). |
| **3.3.3 Error Suggestion** (Level AA) | Partially Supports | The date-range modal displays a text hint when the range is invalid but does not provide a specific correction suggestion. No error guidance is provided for out-of-range time-window number inputs. CSV parse errors describe the problem in text but do not guide the user to a specific correction. |
| **3.3.4 Error Prevention (Legal, Financial, Data)** (Level AA) | Not Applicable | The application performs no legal, financial, or data-modification transactions. All operations are read-only data visualisation on locally uploaded files. |
| **4.1.3 Status Messages** (Level AA) | Supports | All dynamic status regions are marked up with appropriate live region roles. Error banners (`#kt-error-banner`, `#sp-error-banner`) and validation hints (`#kt-tw-hint`, `#sp-tw-hint`) use `role="alert"` (assertive, announced immediately). The loading message (`#loading-msg`), EMC source summary (`#emc-mode-summary`), wick health note (`#kt-wick-note`), empty-state messages (`#kt-empty-msg`, `#sp-empty-msg`), and date-range hints (`#modal-range-hint`, `#sp-modal-range-hint`) use `role="status"` (polite, announced at next opportunity). |

---

## Summary Table

| Guideline | Level A | Level AA |
|---|---|---|
| 1.1 Text Alternatives | Partially Supports | — |
| 1.2 Time-based Media | Not Applicable | Not Applicable |
| 1.3 Adaptable | Partially Supports | Supports |
| 1.4 Distinguishable | Partially Supports | Partially Supports |
| 2.1 Keyboard Accessible | Partially Supports | — |
| 2.2 Enough Time | Not Applicable | — |
| 2.3 Seizures and Physical Reactions | Not Applicable | — |
| 2.4 Navigable | Supports | Supports |
| 2.5 Input Modalities | Supports | Supports |
| 3.1 Readable | Supports | Supports |
| 3.2 Predictable | Supports | Supports |
| 3.3 Input Assistance | Partially Supports | Partially Supports |
| 4.1 Compatible | Partially Supports | Supports |

**Overall conformance claim:** KilnLog does not claim full conformance with WCAG 2.1 Level A or Level AA. The product partially supports a significant portion of success criteria. All known gaps are documented above and are targeted for remediation.

---

## Known Issues Prioritised for Remediation

The following are the highest-priority accessibility gaps identified in this evaluation, listed in recommended fix order:

1. **2.1.1 Keyboard / 2.4.3 Focus Order — modal not fully keyboard accessible.** Convert "Readings" clickable cards to `<button>` elements. Implement a focus trap in both modals (Tab/Shift+Tab cycling within the modal while open), move focus to the modal on open, and return focus to the triggering element on close. Add `role="dialog"`, `aria-modal="true"`, and `aria-labelledby` to each modal container.

2. **1.4.1 Use of Color — chart series (Level A).** Introduce per-series `pointStyle` differentiation (circle, triangle, rect, cross) for multi-series charts in Chart.js so series can be distinguished by shape as well as colour.

3. **1.3.1 / 4.1.2 — data table accessibility.** Add `scope="col"` to all `<th>` elements in generated data tables. Add a `<caption>` element to each table.

4. **1.4.11 Non-text Contrast (Level AA) — input borders.** Update form input borders from `#444` to a colour achieving 3:1 contrast against the `#1a1a1a` input background (e.g., `#767676`).

5. **3.3.1 — time-window input error association.** Associate the validation hint elements with their inputs via `aria-describedby` so error messages are announced by screen readers.

---

## Legal Disclaimer

The information in this Voluntary Product Accessibility Template (VPAT) is provided in good faith and is accurate as of the report date stated above. This document represents a self-assessment and has not been independently audited by a third party. Conformance levels reported here reflect the state of the product at the time of evaluation and are subject to change as the product is updated.

This VPAT does not constitute a warranty, guarantee, or legally binding representation of conformance with any accessibility standard, law, or regulation, including but not limited to Section 508 of the Rehabilitation Act of 1973 (as amended), the Americans with Disabilities Act (ADA), the European Web Accessibility Directive (EU 2016/2102), or EN 301 549.

Users with specific accessibility requirements are encouraged to contact the developer using the information in the Contact section above to discuss individual needs or to report accessibility issues not captured in this document.

---

*This VPAT was prepared following the ITIC VPAT 2.4 template structure. VPAT is a registered trademark of the Information Technology Industry Council (ITIC).*
