---
status: ACTIVE_DESIGN
author: Gemini
domain: Parsing Architecture
last_updated: 2026-07-31
---
## 1. The Multi-Tier Extraction Flow

To survive frontend UI changes during Phase 2 Routine Scrapes across our 6 target marketplaces, the acquisition engine employs a strict, prioritized extraction hierarchy. This prevents a single CSS class change from completely breaking the parser.

1.  **Tier 1: JSON-LD & Embedded State (Primary)**
    *   The parser first searches for structured data (`<script type="application/ld+json">`) or embedded frontend states (e.g., `window.__INITIAL_STATE__`). This data rarely changes because it drives the site's own backend logic and SEO.
2.  **Tier 2: Semantic Locators (Secondary)**
    *   If structured data is missing or incomplete, the parser falls back to semantic HTML structure rather than brittle classes (e.g., searching for a `<th>` containing the exact text "Year Built" and grabbing the adjacent `<td>`).
3.  **Tier 3: Scrapling Auto-Relocation (Fallback)**
    *   When rigid selectors fail, the adapter engages Scrapling's adaptive engine (`adaptive=True`). Scrapling uses the element fingerprint (saved during a previous successful scrape via `auto_save=True`) to intelligently relocate the missing field based on context, parent tags, and text density.

## 2. AI-Assisted Repair Guardrail

While Scrapling provides immediate auto-relocation, there are scenarios where a major site overhaul completely destroys the DOM map. In these catastrophic failures, we employ AI (AgentQL/Stagehand) to attempt a repair. 

**STRICT RULE:** AI-generated selector repairs **MUST** be human-approved before merging into the production branch.
*   **The Risk of Silent Corruption:** Without a human gate, an AI might confidently (but incorrectly) identify a monthly financing cost as the boat's primary asking price, permanently corrupting the YPI valuation dataset.
*   **The Workflow:** If the extraction fails, an alert is generated. The AI proposes a new set of selectors based on the raw HTML snapshot. A human reviews the proposed fix against the snapshot before updating the adapter's code.

## 3. Offline Mutation Experiment

Before deploying Scrapling's adaptive relocation into the live production engine, we must scientifically prove its reliability. We will execute an **Offline Mutation Experiment**.

### Experimental Design:

1.  **Baseline Generation:** Take 20 archived HTML snapshots of Boat24 listing detail pages. Run a standard parser and save the Scrapling element fingerprints.
2.  **Artificial Mutation:** Run a Python script to aggressively alter the archived HTML files. The script will:
    *   Randomly rename CSS classes (e.g., `.price-tag` to `.v2-pricing-container`).
    *   Wrap target elements in arbitrary `<div>` or `<span>` tags to change the DOM depth.
    *   Reorder non-essential elements on the page.
3.  **Measurement & Validation:** Run the Scrapling adaptive parser against the mutated files.
4.  **Success Criteria:**
    *   **Accuracy:** Does Scrapling correctly identify the exact price, year, and make/model fields on >95% of the mutated files without extracting incorrect sibling elements?
    *   **Speed:** Does the adaptive search execute in under 100ms per file to maintain our required processing throughput?