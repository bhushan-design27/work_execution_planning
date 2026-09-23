You are acting as the UX Expert in a BMAD-style workflow. Produce 
docs/DESIGN.md: the front-end specification developers and AI dev agents 
will build the RTW (Ready to Work) Dashboard from.

INPUTS
· prototype/RTW Dashboard - standalone.html — the approved prototype. This 
  is the source of truth for every visual value.
· prototype/screens/ — screenshots of the prototype's states.
· The PRODUCT RULES section below — the source of truth for behaviour. The 
  prototype uses hardcoded sample data and does not encode these rules.

NOTE ON THE PROTOTYPE FILE
This is a bundled export. The outer shell is loader scaffolding — ignore it. 
The real application source is embedded, escaped, in the script block near 
the end of the file. Unescape and read that to extract markup, styles, and 
component logic. Ignore anything named __bundler_*, and ignore 
<title>Bundled Page</title> — that is scaffolding, not the app's title.

HOW TO WORK
· Extract tokens from the code: colours, type scale, spacing, radii, borders. 
  Use the actual values. Never invent a value the code does not contain.
· Where the code and a screenshot disagree, trust the code and note the 
  discrepancy.
· Where something is needed but unspecified, write [OPEN QUESTION: …] rather 
  than deciding it.
· Do not document the sample data as if it were a requirement. Data shapes 
  yes; specific work requests no.
· Write for a developer who has never seen the prototype. Every component 
  must be buildable from this document alone.

═══ DESIGN.md STRUCTURE ═══

1. Overview
   What the dashboard is for, in three sentences. Primary users: area 
   managers. Secondary: production leads and grid ops managers who monitor. 
   Read-and-notify only — it never changes data in source systems.

2. Design tokens
   Colour (semantic names: status-ready, status-not-ready, barrier-cleared, 
   etc.), typography scale, spacing scale, radii, borders. As a table and as 
   CSS custom properties.

3. Layout
   Page structure top to bottom, grid, breakpoints, max widths, table column 
   widths (which are fixed, which is flexible).

4. Components
   For each: purpose, anatomy, every state, exact tokens used, content rules.
     · Area filter chips (default, selected, All)
     · Project lead dropdown
     · Metric card (default, active, zero)
     · Table row (collapsed, expanded, on hold)
     · Status pill (Ready, Not Ready, On Hold)
     · Barrier dot (cleared, in progress, blocked, N/A)
     · Blocker cell (populated, empty)
     · Barrier card (in progress, blocked, cleared, N/A, after notify)
     · Audit trail
     · Notify flow and confirmation
     · Pagination, sort, export
     · Empty state, loading state

5. Product rules — reproduce the section below, organised and complete

6. Content and vocabulary
   Operator status wording is never normalised. Terminology list.

7. Accessibility
   Contrast, colour-not-alone, keyboard, focus, screen-reader labels for dots 
   and pills.

8. Out of scope for v1 — with the reason for each.

9. Open questions

═══ PRODUCT RULES ═══

Work request status
· Every WR has exactly one status: READY, NOT READY, or ON HOLD.
· READY: every required barrier is cleared.
· NOT READY: one or more required barriers not cleared.
· ON HOLD: deliberately paused. Excluded from Ready and Not Ready counts.

Barriers — seven, always in this order
  Locates · Staking · Permits · Gold Tape · Outage · SWO · Material
· All seven cards always show in the expanded row, fixed order, never 
  reordered by urgency.
· Each barrier maps to one of: cleared, in progress, blocked, N/A.
· Status wording on cards is the operators' own — Complete, Scheduled, 
  Pending, Written, Approved, Src Down, Not in EO, Hold, Returned, Rejected. 
  Never replace it with a generic word.
· "Src Down" is an operational SWO step (source feeder switched out). It 
  counts as cleared.

N/A
· Only Locates, Staking, Permits, Outage, SWO and Material can be N/A.
· Gold Tape is never N/A — every WR is checked against the EO registry.
· If Outage is N/A, SWO is normally N/A.
· N/A comes from work-type rules.
· N/A cards show Reason and Normally (what the barrier usually tracks), 
  greyed, no Notify, source link inactive.
· N/A barriers are excluded from the count: "5 of 5 cleared · 2 N/A".

Blocker
· One per NOT READY row: barrier name, its status word, and owner.
· Blocked beats in progress; ties go to fixed barrier order.
· System-updated barriers show "via [system]", never a person or team.
· READY rows: em dash.
· ON HOLD rows: "On hold · [reason] · since [date]".

Notify
· Shown only where a person can be reached.
· Never on cleared barriers, N/A barriers, or Gold Tape (updates 
  automatically from the EO registry).
· After sending: card shows "Notified just now · [name]" with a muted 
  "Notify again". Row's LAST NOTIFIED updates. Toast confirms.
· Sends from the user's own address and is logged on the WR.

Metric cards
· Three: Work Requests, Ready, Not Ready. All clickable status filters.
· Counts recompute against selected areas and project lead.
· Work Requests subline shows the on-hold count. Ready and Not Ready 
  sublines state their denominator, excluding on hold.
· A card showing 0 stays clickable; clicking shows the empty state.

Filters
· Area chips: multi-select, "All" first and selected by default. Multi-select 
  exists so a manager can cover a colleague's area alongside their own.
· Project lead: independent dropdown, not dependent on area.
· Page title reflects the selected areas.

Sort
· Default: Not Ready first, then Ready, then On Hold. Within each, by WR#.

Deliberately out of scope for v1
· Age / days idle — source systems do not reliably record when a barrier 
  last changed.
· Scheduled dates and expiry warnings — crew dispatch is decided elsewhere.
· Unknown / feed-down state — no evidence in source systems.
· Filters panel, barrier breakdowns, analytics — deferred to a future 
  analytics page.
· Permissions — read-and-notify tool, no role model needed.