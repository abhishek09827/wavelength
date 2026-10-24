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
• OMDb API (IMDb/RT Scores)    • "Vibe Document" Builder          • pgvector (1536-d Item & User
