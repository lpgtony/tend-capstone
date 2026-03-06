# Tend — AI-Assisted Credit Card Benefits Tracking and Optimization

**Northwestern University, Master of Science in Data Science**
**Capstone Project — Winter 2026**

**Team:** Minjeoung (MJ) Neev, Tony Lee
**Live Application:** [tendcard.com](https://tendcard.com)

---

## What Is Tend?

Credit cards have evolved into complex financial products offering rewards multipliers, statement credits, lounge access, insurance, and time-sensitive promotional offers. Yet most cardholders — especially those with premium cards carrying high annual fees — significantly underutilize the benefits they're paying for. Information is fragmented across issuer apps, emails, and PDFs, creating cognitive overload at the point of purchase.

Tend is an AI-assisted platform that helps users understand, track, and optimize their credit card benefits. It combines structured databases, deterministic reward logic, and large language models into a single system that reduces the cognitive burden of managing a multi-card portfolio.

## How AI Is Used

Tend uses Google Gemini 2.5 Flash across four distinct AI capabilities, each targeting a different type of unstructured financial information:

**Grounded Generation** — When a user adds a card, the system uses Gemini with web grounding to discover the card's current benefits and earning rates from live issuer sources. Results are returned as structured JSON, validated against a project-defined taxonomy, and stored as canonical definitions that are reused across users holding the same card.

**Multimodal Vision** — Promotional offers are only visible as visual elements inside issuer apps and portals. Users upload a screenshot, and Gemini's vision model extracts structured fields — merchant, discount type, value, expiration, and spending limits. Issuer-specific prompt templates improve accuracy across different portal layouts.

**Retrieval-Augmented Generation (RAG)** — The chatbot, purchase planner, and location-aware assistant all follow the same pattern: the backend retrieves the user's actual card data from the database, assembles a structured context payload, and injects it into the LLM prompt. This grounds every recommendation in verified user data rather than parametric model knowledge.

**Location-Aware Recommendations** — The Near Me feature extends the RAG pipeline with real-time geolocation. Device coordinates are resolved server-side to a merchant name and spending category, then the same recommendation pipeline returns the optimal card. Coordinates are never sent to the LLM, and location data is not stored.

## Architecture

| Layer | Technology | Role |
|---|---|---|
| Frontend | Next.js, TypeScript, Tailwind CSS | Single-page app with dashboard, card management, chat |
| Backend | FastAPI, Python, SQLAlchemy | REST API, business logic, LLM orchestration |
| Database | PostgreSQL on Neon | Relational storage with two-layer benefit catalog |
| Auth | Supabase Auth | JWT-based authentication across web and mobile |
| AI | Google Gemini 2.5 Flash | Grounded generation, vision parsing, RAG |
| Infrastructure | AWS Amplify, App Runner, Route 53, SES | Hosting, DNS, email |

A core design principle is the separation of **deterministic** and **probabilistic** layers. Reward math, value calculations, and business rules are handled by deterministic code. LLMs are scoped to extraction, semantic interpretation, and natural-language interaction — never to financial calculations.

## Key Design Decisions

**Two-Layer Benefit Catalog** — Canonical benefit definitions are stored per card product. When a user adds a card that already exists in the catalog, the system copies existing records directly instead of calling the LLM again. This eliminates redundant API calls and ensures consistency across users.

**Human-in-the-Loop** — Every AI-generated record is visible and editable. Users can correct benefit types, values, and descriptions. This isn't a fallback — it's a core product decision that builds trust and catches extraction errors.

**Schema Over Model** — The largest reliability improvements came from better prompts, stricter JSON schemas, and a cleaner benefit taxonomy (seven types including a dedicated "milestone" category) rather than from switching models.

## Results

- Benefit discovery captured 12–18 items per premium card in a single LLM call
- Offer extraction reliably parsed merchants and values; accuracy improved with issuer-specific prompts
- RAG recommendations produced correct card selections when upstream data was complete
- Location-aware assistant performed reliably for chain merchants with graceful fallback for independents
- 130+ automated tests across unit and integration levels

## Repository Contents
```
├── report/           Capstone report (PDF)
├── presentation/     Slide deck (PPTX)
└── diagrams/         ERD and architecture diagrams
```

The application source code is maintained in a private repository. The live application is available at [tendcard.com](https://tendcard.com).

## Contact

- **Tony Lee** — tony.lee@leepartnersgroup.com
- **Minjeoung Neev** — laminjon@gmail.com
