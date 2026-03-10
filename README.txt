# 🚀 Project Aurora — SpeakX Notification Orchestrator

> A self-learning notification orchestrator that solves three problems in sequence: WHO to reach (MECE segmentation + behavioral scoring), WHAT to say (LLM-generated bilingual templates grounded in Octalysis), and WHEN to send (timing + frequency optimization) — then closes the loop with Thompson Sampling to measurably improve after every iteration.

---

## 🏗️ System Architecture


         Company KB (PDF/MD/TXT) + User Behavioral CSV
                            │
                            ▼
         ╔══════════════════════════════════════════╗
         ║              TASK 1 · Intelligence       ║
         ║  KB Engine      → north_star.json        ║
         ║  User Ingestion → weighted score columns ║
         ║  MECE Engine    → 10 lifecycle segments  ║
         ║  Goal Builder   → 30-row journey KB      ║
         ╚══════════════════════╦═══════════════════╝
                                ║
                                ▼
         ╔══════════════════════════════════════════╗
         ║         TASK 2 · Communication & Timing  ║
         ║  Theme Engine   → Octalysis themes       ║
         ║  Template Gen   → 150 bilingual templates║
         ║  Timing Opt.    → top-4 windows/segment  ║
         ║  Frequency Opt. → 3–9 notifs/day         ║
         ╚══════════════════════╦═══════════════════╝
                                ║
                                ▼
         ╔══════════════════════════════════════════╗
         ║         TASK 3 · Execution & Learning    ║
         ║  Scheduler      → Iteration 0 schedule   ║
         ║        ↕  experiment_results.csv         ║
         ║  Classifier     → GOOD / NEUTRAL / BAD   ║
         ║  Thompson Samp. → Iteration 1 schedule   ║
         ║  Delta Reporter → causal audit trail     ║
         ╚══════════════════════════════════════════╝
                            │
                            ▼
          iter0/ · iter1/ · learning_delta_report.csv


---

## ⚙️ Key Components
-------------------------------------------------------------------------------------------------------------------
|  Engine             | Method                                                                                    |
|---------------------|-------------------------------------------------------------------------------------------|
| KB Parser           | `pdfplumber` + regex heading reconstruction + fuzzy section matching                      |
| MECE Segmentation   | Lifecycle + propensity decision tree → 10 non-overlapping segments                        |
| Theme Engine        | K-Means (silhouette-selected k=2–4) inside each segment → 9-rule Octalysis rulebook       |
| Template Generator  | **Groq LLaMA-3.3-70B** · 5 creative slots · Hinglish from emotional core (not translated) |
| Timing Optimizer    | CTR × engagement × streak-risk scoring per segment × 6 time windows                       |
| Frequency Optimizer | Activeness bands (>0.7→8/day, 0.4–0.7→5/day, <0.4→3/day) + uninstall guardrail            |
| Learning Engine     | **Thompson Sampling Beta-Bernoulli**: GOOD→α+1, BAD→β+1, posterior means rerank windows   |
| Delta Reporter      | 5 entity types (slot/user/template/timing/segment), every change with causal explanation  |
-------------------------------------------------------------------------------------------------------------------
---

 🔑 API Keys & Setup

Only one external API is used — **Groq** (template generation):


 Replace before running
GROQ_KEYS = ["gsk_KEY1", "gsk_KEY2", "gsk_KEY3"]  # 3 keys = 84 RPM combined

Get free keys at [console.groq.com](https://console.groq.com). All other engines run fully locally.

pip install pdfplumber markdown-it-py pandas numpy scikit-learn groq

---

▶️ Run Instructions

```
1. Open codebase/aurora_pipeline.ipynb in Google Colab
2. Upload Company KB (PDF/MD/TXT) when prompted
3. Upload User Behavioral CSV when prompted
4. Set your 3 Groq API keys in the Template Generator cell
5. Run all cells → Iteration 0 outputs saved
6. Upload experiment_results.csv from evaluator
7. Run Task 3 cells → Iteration 1 + delta report generated
```

---

📈 What Changes Iter 0 → Iter 1

- BAD arms (template × window) suppressed permanently
- Window rankings updated via Beta posterior means
- Guardrail segments auto-reduced by 2 notifs/day
- Every change documented with metric trigger + causal explanation

Domain-agnostic: Swap KB + CSV → same orchestrator runs for FinTech, HealthTech, or any B2C app.

---
No hardcoded outputs · No mock learning · Real measurable delta
