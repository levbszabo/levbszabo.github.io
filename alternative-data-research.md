## Miami Beach Hotel Arbitrage — Alternative Data Alpha Generation

<img src="images/miami-beach-hotel1.jpg?raw=true" alt="Placeholder: alternative data research visualization"/>

**Executive Summary:** Signal-driven acquisition and repositioning of a five-hotel, 356-key Miami Beach portfolio. We used alternative data to surface a quantifiable “crisis of competence” in 3–4 star assets and a management arbitrage among low‑touch operators. Modeled outcomes: 26.5% levered IRR and 2.85× equity multiple over five years (20%+ IRR in downside).

### Download

- Full case study (PDF): [/pdf/MiamiHotel-CaseStudy.pdf](/pdf/MiamiHotel-CaseStudy.pdf)

### At a Glance

- Portfolio: 5 hotels, 356 keys
- Purchase: ~$75.0M (≈$210.7K/key)
- PIP: ~$10.0M (≈$28.1K/key)
- Total Project Cost: ~$86.7M
- Capital: 60% LTC senior debt; ~$34.9M equity
- Modeled Returns: 26.5% levered IRR; 2.85× EM over 5 years; 18.2% unlevered IRR; ~11.8% average CoC
- Stabilized metrics: Year 3 cap on cost ~11.2%; Year 6 exit @ 6.5% cap ≈ $322.4M

### Core Thesis

The opportunity exploits a widening bifurcation in Miami Beach: under-managed 3–4 star assets exhibit a persistent “crisis of competence” (A/C outages, cleanliness, WiFi failures) while tech-centric, low-touch operators ("ghost hotels") underperform in a high-touch market. A new zoning ordinance (2025‑4717) constrains future supply on non-waterfront parcels, creating a regulatory moat that amplifies scarcity and terminal value.

### Alternative Data Methodology

- **Google Reviews via Apify:** Scraped thousands of Google reviews per asset with the Apify Google Maps Reviews actor. Captured review text, star rating, timestamp, and basic reviewer metadata; added de‑duplication and retries to ensure complete coverage.
- **Review Signals (Sentiment + Keywords):** Computed per‑review sentiment scores and weekly trendlines; extracted high‑frequency keywords and phrase co‑occurrences to quantify recurring failure modes such as “broken A/C,” “mold,” “no WiFi,” “last‑minute cancellation,” and “no staff/security.”
- **SEC/EDGAR Deep Research:** Tracked 10‑K/10‑Q/8‑K events for Sonder (SOND) and LuxUrban (LUXH)—including deficiency notices for late filings, auditor changes, and lease terminations—to construct a rolling operator Distress Index.
- **Lawsuit/Legal Signals:** Collected public legal notices and docket events tied to target assets (e.g., eviction actions), severity‑scored them, and aligned them to operator timelines.
- **Market/Regulatory Layer:** Integrated CBRE RevPAR forecasts and Miami Beach Ordinance 2025‑4717 to model supply constraints and the resulting regulatory moat.
- **Entity Resolution & Geospatial Context:** Matched properties and operator/owner entities across data sources and placed them in neighborhood context (demand generators and competitive set) to support underwriting.

### Methodology Deep Dive (for PMs, Researchers, Builders)

- Weekly review trends to detect real breakdowns vs noise.
- Failure‑mode taxonomy (HVAC, cleanliness, connectivity, booking/cancel, staff/security) tied to revenue impact and PIP scope.
- Operator fingerprinting: a distinct “ghost hotel” cluster (no staff, remote support, last‑minute cancellations).
- Align review spikes with SEC events (auditor change, lease termination, deficiency notices) to validate distress.
- Map signals to occupancy/ADR penalties and PIP payback.

### Alpha Signals

- **Crisis of Competence:** Quantified operational failure creates an underserved “Reliability‑Seeking” traveler segment that will pay a premium for consistent execution.
- **Management Arbitrage:** Replace failing low‑touch operators with high‑touch management to unlock immediate RevPAR uplift and asset value expansion.

### How We Found the “Ghost Management” Arbitrage

- Review topics surfaced a distinct cluster: “no staff,” “no security,” “last‑minute cancellation,” “scam,” “no response.”
- These assets overlapped with operators flagged by the Distress Index (late filings, auditor changes, lease terminations).
- Legal dockets confirmed on‑the‑ground breakdowns (e.g., eviction actions). The triangulation revealed a management‑model failure, not just bad properties.
- Underwriting implication: swap operator → immediate service normalization → occupancy recovery + ADR normalization with minimal structural capex.

### Signal → Financial Value (Underwriting Mapping)

- **Operational Failure Rate (reviews)** → Occupancy penalty and ADR discount by failure class; drives PIP scope and payback.
- **Distress Index (SEC/EDGAR)** → Acquisition discount and probability‑weighted operator replacement timeline.
- **Legal Events (lawsuits/evictions)** → Catalyst flags and timing for control changes; informs interim cash drag.
- **Regulatory Moat (ordinance)** → Exit cap rate compression (“scarcity multiplier”).
- **Geospatial Advantage** → Pricing power uplift vs comp set post‑stabilization.

### Target Portfolio & Strategy

- **Tier 1 (Anchor Acquisition):** James Hotel — neglected but prime location; for‑sale status accelerates execution.
- **Tier 2 (Heavy Lift):** Seacoast Suites — large condo‑hotel; advanced physical decay; bulk‑unit acquisition + focused PIP.
- **Tier 2 (Boutique Repositioning):** Kaskades Hotel — severe health/safety issues; curated turnaround plan.
- **Tier 3 (Management Arbitrage):** The Astor — operator collapse under LuxUrban; legal leverage to replace operator.
- **High‑Risk/High‑Reward Flagship:** Cardozo Hotel — iconic brand with reputational damage; asymmetric upside post‑stabilization.

### Financial Model (Summary)

- Acquisition ~$75.0M; PIP ~$10.0M; Total project cost ~$86.7M (incl. fees)
- Senior debt: ~$51.8M (≈60% LTC); Equity: ~$34.9M
- Stabilized occupancy ≈ 82%; ADR uplift via reliability branding; RevPAR +2% CAGR post‑stabilization
- Exit Year 6 @ 6.5% cap ≈ $322.4M
- Outcomes: 26.5% levered IRR; 18.2% unlevered IRR; 2.85× EM; ~11.8% avg CoC

### Risks & Mitigations

- **Execution Risk:** 10% PIP contingency, reputable GCs, performance‑tied contracts
- **Market Risk:** Defensive reliability positioning; regulatory moat limits new supply
- **Financing Risk:** Conservative leverage; early term sheet locking
- **Management Risk:** Replace operators; performance‑based management agreements

### Developer/Analyst Replication Notes

1. Automate review scraping (e.g., Apify) and run NLP for sentiment, keyword frequency, topic clustering
2. Integrate EDGAR API for late filings, lease terminations, auditor changes
3. Scrape legal case databases for disputes tied to target assets
4. Build a geospatial layer (competition, demand drivers) to contextualize pricing power
5. Link operational improvements to valuation in a dynamic financial model; support scenario and stress testing
6. Generalize the pipeline to new geographies, asset classes, and cycles



> Disclaimer: Informational research summary only; not investment advice. All projections are illustrative and subject to change.


