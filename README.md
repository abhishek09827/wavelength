<div align="center">
  <h1>λ Wavelength</h1>
  <p><strong>Movies, Web Series, and Games on your frequency.</strong></p>
  <p>A cross-domain social discovery platform and AI recommendation engine bridging entertainment mediums through semantic vibes, narrative tropes, and real-time mood routing.</p>
</div>

---

## ⚡ Why Wavelength (λ)?

In physics, **wavelength ($\lambda$)** measures the spatial period of a wave—and in human connection, being *"on the same wavelength"* means sharing an unspoken emotional and intellectual frequency.

Traditional recommendation engines are trapped in media silos: Netflix doesn't know you just put 80 hours into *Cyberpunk 2077*, and Steam doesn't know you just binged *Severance*. Worse, standard algorithms match superficial genres ("Sci-Fi", "Action") rather than **emotional resonance, pacing, and narrative tone**.

**Wavelength ($\lambda$)** maps your entire entertainment footprint into a unified **Frequency Profile** and tunes recommendations to your **exact mood and time budget**.

---

## ✨ Core Features

* **📚 Unified Cross-Media Shelf:** Log, rate, and review Movies, Web Series, and Video Games in one place.
* **🏷️ Vibe Micro-Tagging:** Go beyond 5-star ratings—tag titles with community frequencies (*Mind-Bending, Cozy Escapism, Bleak, Emotional Damage, Brain-Off Fun*).
* **🎛️ The Frequency Tuner (Mood Router):** Get instant, cross-media recommendations tuned by:
  * **Current Mood:** Natural language prompts or quick mood pills.
  * **Time Budget:** *2-hour movie*, *Weekend miniseries binge*, or *50-hour RPG sink*.
  * **Social Context:** *Solo immersion*, *Couch co-op*, or *Watch with family*.
* **🧬 Personal Taste Frequency:** Watch your vector profile evolve in real time as you log titles, curate cross-media collections, or skip suggestions.
* **🤖 Explainable AI Pitches:** Powered by low-latency LLMs to explain *why* an indie game shares the exact same frequency as your favorite thriller movie in one spoiler-free sentence.

---

## 🏗️ System Architecture & Data Pipeline

```text
[EXTERNAL DATA APIS]          [INGESTION & ENRICHMENT]           [STORAGE LAYER (Supabase)]
• TMDB API (Movies/Series) ──► • Cross-Media Normalizer ────────► • PostgreSQL (Relational Metadata)
• OMDb API (IMDb/RT Scores)    • "Vibe Document" Builder          • pgvector (1536-d Item & User Vectors)
• IGDB & Steam (Games/Tags)    • Embedding Worker (bge-m3/OpenAI) • Redis (Hot Cache & Session State)
                                        ▲                                    │
                                        │ (Feedback Loop: Tags, Lists, Logs) │
[USER TOUCHPOINTS (Next.js)]            ┴                                    ▼
• Frequency Tuner UI ◄──────── [THE WAVELENGTH ENGINE (FastAPI)] ◄───────────┘
• Shareable λ Lists            1. Mood + Profile Vector Blender
• Taste Frequency Card         2. Hybrid Retrieval (ANN Cosine + Bayesian Score)
                               3. LLM Re-Ranker & Pitch Generator (Gemini 2.5 Flash)

```

---

## 🧠 How the λ Engine Blends API Data with User Interactions

To solve the **Cold-Start Problem** on Day 1 while allowing the community to shape recommendations over time, Wavelength uses a **3-Layer Hybrid Scoring & Vector Evolution Model**:

### 1. Dynamic Bayesian Quality Score (API Ratings + Community Ratings)

Initially, item quality relies on normalized external API ratings (TMDB, IMDb, Metacritic, Steam). As Wavelength users log and rate items, the score smoothly transitions toward community consensus using a Bayesian weighted average:

$$S_{\text{quality}} = \frac{(W_{\text{ext}} \cdot R_{\text{external}}) + (N_{\text{users}} \cdot R_{\text{internal}})}{W_{\text{ext}} + N_{\text{users}}}$$

* $R_{\text{external}}$: Normalized 0–10 score aggregated from IMDb/TMDB (Movies/Series) or IGDB/Steam (Games).
* $W_{\text{ext}}$: Prior confidence weight (set to `50` virtual votes).
* $N_{\text{users}}$: Number of internal Wavelength user ratings.
* $R_{\text{internal}}$: Mean rating from Wavelength users.
* *Impact:* A title with 0 internal reviews trusts the API score 100%. Once a title crosses 50+ community reviews, your users' ratings outweigh external critics.

### 2. Semantic "Frequency Drift" (Evolving Item Vectors)

Every title begins with a base embedding ($V_{\text{item}}$) generated from API plot synopses, tropes, and official genres. As users interact with the platform, their actions physically shift the item's vector in 1536-dimensional space:

* **Vibe Micro-Tags & Reviews:** When a threshold of users tag a movie as *"Cozy"* or write a review comparing a series to *Disco Elysium*, an async worker appends those top community tags to the item's **Vibe Document** and re-embeds it.
* **Co-Occurrence in User Collections:** When users curate cross-media lists (e.g., *"Rainy Night Noir"* containing *Se7en*, *True Detective*, and *Alan Wake 2*), a contrastive learning step nudges those items' vectors closer together.

### 3. Implicit & Explicit User Signals (Shaping the User Vector)

Every user action updates their **Personal Frequency Vector ($V_{\text{profile}}$)** with distinct weights:

| User Interaction | Signal Weight | Engine Impact |
| --- | --- | --- |
| **Added to Custom "λ List" + 5★ Review** | `+1.0` | Strongly pulls $V_{\text{profile}}$ toward the item's vector. |
| **Logged as "Completed / Loved" (4★)** | `+0.7` | Standard positive update to long-term taste profile. |
| **Added to Watchlist / Backlog** | `+0.3` | Weak positive signal; boosts similar items in short-term sessions. |
| **"Off-Frequency Tonight" (Session Skip)** | `-0.4` (Temp) | Penalizes that sub-vibe *only* for the active mood session vector ($V_{\text{mood}}$). |
| **"Dropped / Disliked" (1★ or Hide)** | `-0.8` | Pushes $V_{\text{profile}}$ away and adds a hard SQL exclusion filter. |

### 4. Final Ranking & Blending Formula

When a user tunes their mood, the engine blends their active mood ($V_{\text{mood}}$) with their historical profile ($V_{\text{profile}}$) using weight $\lambda_{\text{mood}} \in [0, 1]$:

$$V_{\text{target}} = \lambda_{\text{mood}} \cdot V_{\text{mood}} + (1 - \lambda_{\text{mood}}) \cdot V_{\text{profile}}$$

It then retrieves the top 50 candidates via `pgvector` and ranks them using:

$$\text{Final Score} = 0.55 \cdot \text{CosineSim}(V_{\text{target}}, V_{\text{item}}) + 0.25 \cdot S_{\text{quality}} + 0.20 \cdot S_{\text{wavelength\_match}}$$

*(Where $S_{\text{wavelength\_match}}$ boosts items loved by users with high cosine similarity to the current user).*

---

## 🛠️ Tech Stack

* **Frontend:** Next.js 15 (App Router), TypeScript, Tailwind CSS, Framer Motion, Shadcn UI
* **Backend / ML Engine:** Python 3.11+, FastAPI, SQLAlchemy, Pydantic
* **Database & Vector Store:** PostgreSQL + `pgvector` (Supabase)
* **Async Queue & Cache:** Redis (Upstash) + Celery / ARQ
* **External Data Sources:**
* **Movies & Series:** TMDB API, OMDb API
* **Video Games:** IGDB API (Twitch OAuth), Steam Store API, HowLongToBeat wrapper


* **AI & Embeddings:** OpenAI `text-embedding-3-small` (or local `BAAI/bge-m3`), Google Gemini 2.5 Flash

---

## 📂 Repository Structure

```text
wavelength/
├── apps/
│   ├── web/                    # Next.js Frontend & Social UI
│   │   ├── src/app/            # App Router (Feed, Frequency Tuner, Profile)
│   │   ├── src/components/     # Media cards, λ Tuner sliders, Taste graphs
│   │   └── src/lib/            # Supabase client & API hooks
│   └── engine/                 # FastAPI Recommendation & Ingestion Service
│       ├── app/
│       │   ├── api/            # Endpoints: /tune, /ingest, /feedback
│       │   ├── core/           # Vector blending, Bayesian math, LLM prompts
│       │   ├── pipelines/      # Medallion ETL: TMDB & IGDB normalizers
│       │   └── workers/        # Async user vector & frequency-drift updaters
│       └── requirements.txt
├── supabase/
│   └── migrations/             # SQL schemas, pgvector HNSW indexes & RPCs
├── docker-compose.yml          # Local Postgres (pgvector) + Redis setup
└── README.md

```

---

## 🚀 Getting Started

### 1. Prerequisites

* **Node.js** (v20+) & **pnpm**
* **Python** (v3.11+)
* **Docker** (for local PostgreSQL with `pgvector` and Redis)
* API Keys for **TMDB**, **Twitch/IGDB**, and **Gemini / OpenAI**

### 2. Environment Variables

Create a `.env` file in the root directory:

```env
# Database & Cache
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/wavelength"
REDIS_URL="redis://localhost:6379/0"

# External Media APIs
TMDB_API_KEY="your_tmdb_key"
IGDB_CLIENT_ID="your_twitch_client_id"
IGDB_CLIENT_SECRET="your_twitch_client_secret"
OMDB_API_KEY="your_omdb_key"

# AI & Embeddings
OPENAI_API_KEY="your_openai_key"
GEMINI_API_KEY="your_gemini_key"

```

### 3. Run Locally

```bash
# 1. Start PostgreSQL (with pgvector) and Redis
docker compose up -d

# 2. Start the FastAPI λ Engine
cd apps/engine
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000

# 3. Start the Next.js Web App
cd ../web
pnpm install
pnpm dev

```

---

## 🗺️ Roadmap

* [x] **Phase 1: Unified Ingestion Pipeline** – Normalize TMDB & IGDB schemas into Bronze/Silver/Gold layers.
* [ ] **Phase 2: Social Logging & λ Lists** – User auth, media logging, micro-tag pills, and shareable cross-media collections.
* [ ] **Phase 3: Vector Engine & Frequency Profile** – `pgvector` HNSW indexing, user profile embeddings, and Bayesian score blending.
* [ ] **Phase 4: Frequency Tuner & LLM Pitches** – Interactive mood routing UI with real-time Gemini 2.5 Flash explanations.
* [ ] **Phase 5: One-Click Imports** – Sync existing watch/play history from Letterboxd CSV, Trakt, and Steam Public Profiles.

---

## 📄 License

Distributed under the MIT License.

```

 Notice how using $\lambda_{\text{mood}}$ right in the vector blending formula ties the brand identity (`λ`) directly into the math of your engine. Want to draft the **PostgreSQL + `pgvector` schema migration** next to start wiring up the backend?

```
