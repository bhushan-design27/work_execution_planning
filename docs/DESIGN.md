# RTW Dashboard Design Specification

## 1. Overview

The RTW Dashboard gives area managers a single read-only view of work-request readiness across their management areas. It shows the status of seven operational barriers, identifies the most important blocker, and provides enough source detail to decide who needs a follow-up. Production leads and grid operations managers are secondary monitoring users.

The dashboard never changes data in source systems. Its only write action is sending a notification from the signed-in user's address; that action is logged against the work request.

## 2. Design tokens

The prototype source is the authority for visual values. The token names below are semantic aliases for those exact values.

### Colour

| Token | Value | Use |
| --- | --- | --- |
| `--color-ink` | `#06303f` | Primary text, headings, links in dark contexts |
| `--color-link` | `#016da4` | Links, active controls, WR numbers |
| `--color-brand` | `#045c86` | Header, selected chips, secondary links |
| `--color-action` | `#0096db` | Open/selected focus border |
| `--color-page` | `#f6f8fa` | Main page background and quiet panels |
| `--color-white` | `#ffffff` | Cards, controls, drawer |
| `--color-muted` | `#6b7279` | Secondary text, labels, metadata |
| `--color-body-muted` | `#4d545b` | Descriptions and body copy |
| `--color-border` | `#c8d4df` | Control borders and drawer border |
| `--color-rule` | `#e9eaeb` | Row separators and section rules |
| `--color-soft-rule` | `#dfe6ed` | Table and card borders |
| `--color-ready` | `#1c8d4f` | Cleared barriers and ready status |
| `--color-ready-bg` | `#d7f3e2` | Ready pill background |
| `--color-ready-border` | `#ade6c4` | Ready pill border |
| `--color-ready-text` | `#125c34` | Ready pill text |
| `--color-progress` | `#c17d06` | In-progress barriers |
| `--color-progress-text` | `#956000` | In-progress card status text |
| `--color-blocked` | `#d6342a` | Blocked barriers and not-ready status |
| `--color-blocked-bg` | `#fde1df` | Not-ready pill background |
| `--color-blocked-border` | `#fbc5c1` | Not-ready pill border |
| `--color-blocked-text` | `#81211d` | Not-ready pill text |
| `--color-na` | `#c8d0d8` | N/A barrier dot |
| `--color-hold-bg` | `#eef1f4` | On-hold pill background |
| `--color-hold-border` | `#d3dbe3` | On-hold pill border |
| `--color-overlay` | `#06303f40` | Drawer scrim |

```css
:root {
  --color-ink: #06303f;
  --color-link: #016da4;
  --color-brand: #045c86;
  --color-action: #0096db;
  --color-page: #f6f8fa;
  --color-white: #ffffff;
  --color-muted: #6b7279;
  --color-body-muted: #4d545b;
  --color-border: #c8d4df;
  --color-rule: #e9eaeb;
  --color-soft-rule: #dfe6ed;
  --color-ready: #1c8d4f;
  --color-ready-bg: #d7f3e2;
  --color-ready-border: #ade6c4;
  --color-ready-text: #125c34;
  --color-progress: #c17d06;
  --color-progress-text: #956000;
  --color-blocked: #d6342a;
  --color-blocked-bg: #fde1df;
  --color-blocked-border: #fbc5c1;
  --color-blocked-text: #81211d;
  --color-na: #c8d0d8;
  --color-hold-bg: #eef1f4;
  --color-hold-border: #d3dbe3;
  --color-overlay: #06303f40;
}
```

### Typography

Load `Barlow` for body text and `Barlow Condensed` where the prototype uses condensed display text. Use `-apple-system, BlinkMacSystemFont, sans-serif` only as the loading-shell fallback. The prototype's practical scale is 11px uppercase labels, 13px controls and metadata, 14px body text, 16px row text, 21px drawer heading, and 32px page heading. Use `font-variant-numeric: tabular-nums` for counts, dates, and IDs. Uppercase labels use `font-weight: 700` and `letter-spacing: 0.1em`; chips use `letter-spacing: 0.04em`.

### Spacing, radii, and borders

Use the values present in the prototype: 2px, 3px, 4px, 5px, 6px, 8px, 10px, 12px, 14px, 16px, 18px, 20px, 24px, 30px, 36px, and 38px. Use `4px` for inputs and compact controls, `6px` for barrier cards and toasts, `8px` for metric cards, and `999px` for pills, chips, and circular controls. Standard borders are `1px`; selected metric cards use `2px`; expanded rows have a `3px` left accent.

## 3. Layout

The page is a full-width application surface with a fixed-height brand header, a filter band, a pale dashboard content band, and a wide work-request table.

1. **Header:** brand teal `#045c86`, approximately 72px high in the reference. Place the grid/app icon and `RTW Dashboard` at left. Place notification bell and user initials at right.
2. **Filters:** white band below the header. The first row is `MANAGEMENT AREA` followed by horizontally wrapping multi-select chips. The second row is `PROJECT LEAD` followed by the independent dropdown. Filter controls use 32px height and 999px radius.
3. **Dashboard heading:** page title reflects selected areas. Place request count and source refresh range beside it. Use 32px heading text and 21px supporting text.
4. **Metrics:** three cards in one row on wide screens: Work Requests, Ready, Not Ready. Each card is 1px bordered, 8px radius, white, with an optional selected state of `#dff3fe` and a 2px `#0096db` border. The prototype cards are approximately 504px wide in the desktop capture; allow the three cards to shrink as a group while preserving readable content.
5. **Table surface:** white bordered container with 8px radius. The toolbar contains the request count at left and Export plus Sort controls at right. The header uses a pale `#f6f8fa` background.
6. **Rows:** use a fixed seven-dot barrier region and flexible text columns. The visual order is expand affordance, WR#, address/project, description, status, barrier dots, blocker, last notified. Keep barrier abbreviations `LOC`, `STK`, `PRM`, `GT`, `OUT`, `SWO`, `MAT` in the header and show an information affordance beside them.
7. **Expanded row:** retain the selected summary row above a pale detail region. Detail uses a four-column barrier-card grid on wide screens plus a right-hand context column separated by `1px solid #dfe6ed`. The seven cards are always in fixed barrier order and wrap to a second row. The context column contains Description, SSD, Remarks, and Audit trail.
8. **Footer:** show range, Rows selector, and pagination. The prototype uses a 10-row page size and compact circular page controls.

The supported responsive scope is desktop and tablet, with a minimum supported viewport width of `768px`; phone widths below `768px` are out of scope for v1. Use `1024px` as the tablet breakpoint: at widths below `1024px`, keep every table column at its desktop width and enable horizontal scrolling. Freeze the `WR#` and `ADDRESS / PROJECT` columns while the remaining columns scroll horizontally; frozen cells retain an opaque white or selected-row background and a right-edge separator so content cannot show through. Never stack or reflow rows. At tablet width, the expanded barrier grid changes from four columns to two columns; cards continue in fixed barrier order, and the context column moves below the barrier grid at full width.

## 4. Components

### Area filter chips

Purpose: scope all metrics and rows by one or more management areas. `All` is first and selected by default. The default selected chip is brand blue fill with white text; unselected chips are white with `#c8d4df` border and `#06303f` text. Multiple specific area chips may be selected. `All` is exclusive: selecting any specific area deselects `All`; clicking `All` clears all specific selections. The title reflects the selection.

### Project lead dropdown

Purpose: independently filter by project lead. The closed control is a 32px pill; open/active state uses `#dff3fe` and `#0096db`. The menu is white, has a 1px `#d6d8da` border, `0 8px 24px #06303f1f` shadow, and 6px vertical padding. Each option has a 16px checkbox, label, and count. Include `All project leads` and a footer stating that the filter is independent of area.

### Metric card

Three clickable cards: Work Requests, Ready, Not Ready. Each has a 9px colored dot, uppercase 13px label, large count, and 13px/18px subline. Work Requests subline includes on-hold count; Ready and Not Ready use the denominator excluding on hold. Default is white. Active is pale blue with a 2px blue border. Zero remains clickable and filters to the empty state.

### Table row

Collapsed rows show a chevron, linked WR number, address/project with area, description, status pill, seven barrier dots, blocker, and last-notified metadata. Use approximately 16px row copy and 13px secondary lines. Selected/expanded rows use a pale blue highlight, a 3px brand-blue left border, and a downward chevron. On-hold rows use the On Hold pill and show `On hold · [reason] · since [date]` in the blocker cell. Rows sort Not Ready, Ready, On Hold, then WR#.

### Status pill

Use 20px high, 8px horizontal padding, 999px radius, 11px bold uppercase text, and 0.1em tracking. Ready uses ready tokens; Not Ready uses blocked tokens; On Hold uses hold tokens. Text is exactly `READY`, `NOT READY`, and `ON HOLD`.

### Barrier dot

The table dot is 11px, circular, with a 1.5px darker border. Cleared is green, in progress is amber, blocked is red, and N/A is an empty grey-outlined circle. The dot must have an accessible text label such as `Locates: cleared`; color is never the only meaning. Expanded cards use the same state color as a 3px left border and a larger header dot.

### Blocker cell

For a NOT READY row, show one blocker: `[Barrier] · [operator status word]` and the owner on the next line. Blocked beats in progress; ties use fixed barrier order. System-updated ownership is `via [system]`. READY shows an em dash. ON HOLD shows the hold reason and date. Do not replace operator wording with generic status wording.

### Barrier card

Expanded rows always show Locates, Staking, Permits, Gold Tape, Outage, SWO, and Material in that order. Cards are white, 6px radius, `#e4e8eb` top/right/bottom borders, and `10px 12px 8px` padding. In-progress and blocked cards use a 3px state-colored left border. Cleared cards keep the same size and position but use a muted grey border, so they recede through colour only. Header contains the barrier name and operator status word. The body is a two-column label/value list using 13px text, with a footer rule and a named source-system link followed by an external-link arrow: Locates → Sunshine 811; Staking → Design Manager; Permits → FDOT ePermits; Gold Tape → EO Registry; Outage → Outage Scheduler; SWO → Switching Manager; Material → SAP Material. All barrier cards within the same row share equal height, sized to the tallest card in that row. Cards do not carry a fixed minimum height, and the card grid does not equalise height across rows. The source link is the system name plus the external-link arrow, with no label or other text before it.

Cards use barrier-specific body fields and do not repeat the header's operator status word as a body field:

- **Locates:** ticket #, provider, valid until.
- **Staking:** staker, design file.
- **Permits:** permit #, agency, contact.
- **Gold Tape:** data source, last checked.
- **Outage:** window, crew.
- **SWO:** status, written, source, last checked.
- **Material:** items, needed by, hold date.

In-progress and blocked cards may show Notify when a reachable person exists. Notify sends to one owner per barrier; multiple recipients are out of scope for v1. Cleared cards, N/A cards, Gold Tape, and on-hold barriers do not show Notify. N/A cards are greyed, show exactly two body fields, `Reason` and `Normally`; `Normally` names what that barrier usually tracks, such as `Permit #, agency, contact`. N/A cards disable the source link and have no notification action. Gold Tape links to the EO Registry and is never N/A. After notification, show `Notified just now · [name]` and a muted `Notify again` action.

### Audit trail

The expanded context column shows Description, SSD, Remarks, and Audit trail. Audit entries use an 8px dot, 13px/18px text, a primary event and muted metadata, with a `Show earlier` link. System activity is explicitly identified as system/source-feed activity; person activity includes the person's name.

### Notify flow and confirmation

Clicking Notify opens a right-side drawer over a `#06303f40` scrim. The drawer is 520px wide at desktop, full-width at smaller widths, white, full height, with a left border and `-24px 0 60px #06303f33` shadow. Header shows `Send notification`, work request context, and a 32px circular close button. Body contains To owner chips, owner choices, a fixed Subject field, prior-notification context, a fixed work-request summary, and an editable message. Footer states `Sends from your address · logged on the work request`, with Cancel and a primary `Send to [count]` button. After sending, update Last Notified, replace Notify with Notify again, and show a confirmation toast for 2600ms. Toast styling is dark navy, white 14px text, 6px radius, 11px/16px padding, and a dark shadow.

### Pagination, sort, and export

Default sort is `Status · not ready first`; secondary order is WR#. Sort is an explicit menu control. Export is a button with a download icon that downloads a CSV of the currently filtered view using the filename `rtw-export-YYYY-MM-DD.csv`. Pagination shows range, page size, previous/next, and numbered pages. The prototype default is 10 rows per page.

### Empty and loading states

Empty state appears inside the table surface and reads `No work requests match these filters.` centered with 36px top and 38px bottom padding. A zero metric still leads here. Loading preserves the page shell and controls while data is unavailable and uses skeleton rows matching the table layout, including the column structure and row height.

## 5. Product rules

### Work request status

Every work request has exactly one status: READY, NOT READY, or ON HOLD. READY means every required barrier is cleared. NOT READY means one or more required barriers are not cleared. ON HOLD means deliberately paused and is excluded from Ready and Not Ready counts.

### Barriers

There are always seven barriers, in this order: Locates, Staking, Permits, Gold Tape, Outage, SWO, Material. All seven cards appear in every expanded row and are never reordered by urgency. Each maps to cleared, in progress, blocked, or N/A.

The operator's status word is preserved exactly: Complete, Scheduled, Pending, Written, Approved, Src Down, Not in EO, Hold, Returned, or Rejected. `Src Down` is an operational SWO step and counts as cleared.

### N/A

Only Locates, Staking, Permits, Outage, SWO, and Material can be N/A. Gold Tape is never N/A because every WR is checked against the EO registry. If Outage is N/A, SWO is normally N/A. N/A comes from work-type rules. N/A cards show Reason and Normally, are greyed, have no Notify action, and have an inactive source link. N/A barriers are excluded from the count, for example `5 of 5 cleared · 2 N/A`.

### Blocker and notification rules

There is one blocker per NOT READY row: barrier name, its status word, and owner. Blocked beats in progress; ties use fixed barrier order. System-updated barriers show `via [system]`, never a person or team. READY rows show an em dash. ON HOLD rows show `On hold · [reason] · since [date]`.

Notify appears only when a person can be reached and sends to one owner per barrier. It never appears on cleared barriers, N/A barriers, Gold Tape, or barriers that are on hold. Multiple recipients are out of scope for v1. After sending, show `Notified just now · [name]` with muted `Notify again`; update Last Notified, confirm with a toast, send from the user's own address, and log the action on the WR.

### Filters, metrics, and sort

There are three clickable metric cards: Work Requests, Ready, and Not Ready. Counts recompute against selected areas and project lead. Work Requests includes the on-hold count; Ready and Not Ready sublines state their denominator excluding on hold. Area chips are multi-select with All first and selected by default, allowing a manager to cover another colleague's area. All is exclusive: selecting an area deselects All, and clicking All clears specific selections. Project lead filtering is independent of area. Page title reflects selected areas. Default sort is Not Ready first, then Ready, then On Hold; within each group sort by WR#.

## 6. Content and vocabulary

Never normalize operator status wording. Preserve capitalization and punctuation from source data. Use `work request`, `WR`, `area`, `project lead`, `barrier`, `blocker`, `Last Notified`, `N/A`, `EO Registry`, `source feed`, `via [system]`, and `On hold` consistently.

Barrier display names are full names in detail cards and abbreviated only in the table header: `LOC`, `STK`, `PRM`, `GT`, `OUT`, `SWO`, `MAT`. Use an em dash for intentionally empty values. Sample addresses, descriptions, WR numbers, people, and dates in the prototype are demonstration data, not requirements.

## 7. Accessibility

Use semantic headings, landmarks, buttons, links, table headers, and dialog semantics. Every chip, metric card, row expander, sort control, export control, and Notify action must be keyboard reachable. Menus and the notification drawer must support Escape, visible focus, and logical focus return.

Do not rely on dots or pill color alone. Provide accessible labels that combine barrier name and state, and expose the exact status wording in text. Status pills need sufficient text contrast against their tinted backgrounds; body text, links, and controls must meet WCAG AA contrast. The drawer scrim must not remove access to the open dialog, and background content must be inert while it is open. Announce filter results, expanded/collapsed state, notification success, and empty state to assistive technology.

## 8. Out of scope for v1

- **Age / days idle:** source systems do not reliably record when a barrier last changed.
- **Scheduled dates and expiry warnings:** crew dispatch is decided elsewhere.
- **Unknown / feed-down state:** there is no evidence in source systems.
- **Filters panel, barrier breakdowns, and analytics:** deferred to a future analytics page.
- **Permissions:** this is a read-and-notify tool; no role model is needed.

## 9. Open questions

- What is the source-of-truth date/time format and timezone for Last Notified, SSD, and audit entries? **Answer:** product owner with the data integration/source-system owners.
- Which contact should be used when a barrier has several possible owners? **Answer:** operations product owner with the responsible barrier/system owners.