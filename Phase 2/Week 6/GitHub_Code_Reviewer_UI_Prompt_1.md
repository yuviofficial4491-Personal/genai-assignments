# GitHub Code Reviewer Agent — Phase-Wise UI Build Prompt (ICEPOT Format)

Derived from the "AI User Story Reviewer" example prompt, restructured for the GitHub Code Reviewer Agent and rewritten in ICEPOT form (Instructions, Context, Example, Persona, Output, Tone) as requested. Output is table-only, per the Output directive below.

---

## Master Directive

| Element | Specification |
|---|---|
| **Persona (P)** | You are a **Senior Technical Architect** accountable for the front-end and integration layer of an enterprise AI system. You do not produce prototypes or demos — every deliverable must meet production, audit-ready engineering standards. |
| **Context (C)** | The backend is an **n8n workflow — "GitHub Code Reviewer Agent"** — rebuilt so that a **Webhook** node is the first node (entry point, receives the PR reference from the UI) and a **Respond to Webhook** node is the last node (returns the structured AI review to the UI). The workflow fetches the diff for a given GitHub Pull Request, runs it through an LLM review step, and returns findings (summary, issues, severity, suggestions). The UI is the sole client of this webhook. |
| **Example (E)** | Structural template: the attached 4-phase "AI User Story Reviewer" prompt (HTML/CSS-only → webhook wiring → UX polish → premium branding). That sequencing is preserved below; every requirement inside it is re-scoped from a Jira Story Key review to a **GitHub Pull Request code review**, and the result panel is upgraded from plain text to a structured findings display appropriate for code review output. |
| **Output (O)** | Every phase below — and every response executing a phase — must be delivered **strictly in table format**. No narrative paragraphs outside table cells. Each phase table must resolve: Objective, Scope of Work, UI/Functional Elements, Constraints, Acceptance Criteria. |
| **Tone (T)** | Strong, professional, and directive. Instructions are issued as formal specifications, not casual requests. No hedging, no filler. |

---

## Phase 1 — Build the Complete UI (Structure & Styling Only)

| Field | Instruction |
|---|---|
| **Objective** | Produce the complete HTML structure and CSS styling for the GitHub Code Reviewer Agent interface. No JavaScript at this stage. |
| **Scope of Work** | Build a single-page interface titled **"GitHub Code Reviewer Agent"**, with a subtitle explaining that the application performs AI-driven code review of GitHub Pull Requests. |
| **Required UI Elements** | 1. Header with AI/branding icon and application title.  2. Short descriptive subtext.  3. Input field for a **GitHub Pull Request URL** (e.g. `https://github.com/org/repo/pull/42`).  4. Primary action button — **"Review Code"**.  5. **Code Review Result** panel, initially displaying **"Waiting for code review..."**, structured to later hold a summary, a findings list, and severity indicators (not plain text only). |
| **Design Constraints** | Soft gradient background; glassmorphism cards; rounded corners; smooth shadows; professional spacing; modern hover states; no Bootstrap, no Tailwind; fully responsive (desktop, tablet, mobile). Must read as a real AI/SaaS product, not a form page. |
| **Acceptance Criteria** | Static HTML/CSS only, zero functional JS; visually complete on all breakpoints; all five required elements present and clearly labeled. |

---

## Phase 2 — Connect the UI to the n8n Webhook

| Field | Instruction |
|---|---|
| **Objective** | Wire the completed UI to the n8n **Webhook** entry node and handle the **Respond to Webhook** response, with no visual changes to Phase 1. |
| **Scope of Work** | On clicking **Review Code**: read the entered PR URL → validate it → POST it to the n8n Webhook URL → await the Respond to Webhook payload → render the result. |
| **Required Behavior** | 1. **Validation** — if the field is empty or not a well-formed GitHub PR URL, show a clear inline message and stop; do not call the webhook. 2. **Loading state** — while awaiting the response, the Result panel shows an active loading state, and the button is disabled to prevent duplicate submissions. 3. **Success** — parse the JSON returned by Respond to Webhook and render it into the structured Result panel (summary / findings / severity). 4. **Failure** — network error, timeout, or non-2xx response shows a distinct, friendly error state; the raw error is never shown to the user. |
| **Constraints** | Existing UI/layout from Phase 1 remains unchanged. Only the functionality required for this request/response cycle is added. Webhook URL must be a single, clearly named, easily replaceable constant. |
| **Acceptance Criteria** | Empty/invalid input never reaches the webhook; loading, success, and error paths are all independently reachable and visually distinct. |

---

## Phase 3 — Improve the User Experience

| Field | Instruction |
|---|---|
| **Objective** | Elevate the working application from functional to polished, without altering layout or backend integration. |
| **Scope of Work** | Add motion, feedback, and messaging refinements on top of the Phase 2 build. |
| **Required Enhancements** | 1. Purpose-built loading animation while the AI review is running (not a generic spinner). 2. Refined button interaction states (hover / active / disabled). 3. Smooth transitions between waiting → loading → result states. 4. Explicit **success** confirmation once a review is returned. 5. Explicit, friendly **error** messaging on failure, with a retry path. 6. Improved spacing and visual hierarchy in the Result panel so findings are scannable, not a wall of text. |
| **Constraints** | No change to the webhook contract, request payload, or response handling built in Phase 2. Enhancement only — no new features. |
| **Acceptance Criteria** | Every state transition (idle → validating → loading → success/error) is visually distinct and animated; no regression to Phase 2 functionality. |

---

## Phase 4 — Premium Enterprise Branding & Final Polish

| Field | Instruction |
|---|---|
| **Objective** | Convert the application into a branded, commercial-grade enterprise product surface, without changing workflow logic or functionality already built. |
| **Scope of Work** | Apply organization identity and final production polish across the existing UI. |
| **Required Enhancements** | 1. Organization name and logo in the header. 2. Defined brand theme and color palette (proposed by default; refined on your feedback — see below). 3. Professional header **and** footer (footer: org name, review-tool designation, minimal links/copyright). 4. Deliberate typography, icon set, and button system — not defaults. 5. Confirmed responsive behavior across desktop, tablet, and mobile. 6. Final-pass loading, success, validation, and error messaging, styled consistently with the brand theme. 7. Any additional organization-specific touches (e.g. internal tool badge, environment tag, review-agent version marker). |
| **Constraints** | No change to workflow, webhook contract, or functionality from Phases 1–3. Visual and brand layer only. |
| **Acceptance Criteria** | Interface is indistinguishable in polish from a commercial SaaS product; brand elements consistently applied; all prior functional acceptance criteria still pass. |

---

### Outstanding inputs before execution begins

| # | Needed | Status |
|---|---|---|
| 1 | n8n workflow export (or manually described nodes) — to add the Webhook (first) and Respond to Webhook (last) nodes and confirm the exact request/response payload shape | Not yet received |
| 2 | Organization name, logo, and any existing brand colors/fonts — or confirmation to proceed with a proposed default theme | Not yet received — default theme to be proposed per your instruction |
