

***

# ML‑Powered Adaptive Quiz Coach (IDEATHON 2022 – ACFL)

An **ML‑driven adaptive exam‑prep app** that personalizes question sequences, diagnoses weaknesses, and predicts exam readiness instead of using a fixed NEXT/PREV flow. 

***

## Overview

Traditional quiz apps show the same linear question order to every learner, wasting time for both weak and strong students.  This project proposes an adaptive quiz coach that chooses each next question using machine learning, keeps difficulty in the learner’s “flow zone,” and surfaces clear analytics on mastery and readiness. 

***

## Problem Statement

Learners using typical quiz or test‑prep apps face several issues: 

- One‑size‑fits‑all sequencing: fixed chapter order or simple NEXT/PREV flow, regardless of prior knowledge. 
- No real mastery model: only “percent correct,” with no per‑topic mastery state or uncertainty estimate.
- Difficulty mismatch: items can be too easy (boredom, inflated confidence) or too hard (frustration, drop‑off). 
- No diagnostic or planning layer: students lack topic‑wise diagnosis, readiness forecasting, or a personalized study plan. 
***

## Proposed Solution

The app runs a **closed‑loop adaptive learning cycle** after every attempt, rather than a fixed question list. 

Key ideas:

- Adaptive learning loop: attempt → mastery update → next‑best question → feedback → updated study plan. 
- Key outputs for learners: 
  - Topic mastery heatmap (strengths, weaknesses, progress).  
  - Next‑best question that maximizes learning gain with appropriate difficulty.  
  - Exam readiness score and forecast (score range + confidence + trajectory).  
  - Personalized study plan with daily targets and spaced review for retention.  
- ML‑driven, not rule‑based, using: 
  - Learner model (BKT/DKT) for topic/skill mastery over time.  
  - Content model (IRT) to learn true item difficulty and quality.  
  - Decision policy to pick the next question under constraints (coverage, diversity, no repeats).  

***

## Adaptive Learning Flow

At inference time, each learner session cycles through these steps: 
1. User answers a question (with time‑to‑answer, hints, retries, optional confidence). 
2. Model updates mastery state per topic/skill using knowledge tracing (BKT/DKT). 
3. Recommender selects the next‑best question using calibrated difficulty (IRT) and expected learning gain. 
4. System provides feedback, explanations and similar examples based on tags and error patterns. 
5. System updates exam readiness forecast and adjusts the study plan and review schedule. 

Result: each practice session becomes a personalized coaching loop, instead of a static sequence. 
***

## System Architecture

### Data & Telemetry

- Question bank with metadata: topic/skill tags, difficulty, question type, solution/explanation. 
- Telemetry logs: correctness, time‑to‑answer, hints used, retry count, optional self‑reported confidence. 
- Feature store: user‑topic mastery vectors, rolling performance trends, recency/forgetting features, and optional embeddings. 

### Core ML Components

- Difficulty estimator / IRT calibration for item difficulty and discrimination learned from real attempts. 
- Knowledge tracing model (BKT/DKT) to estimate and update mastery per topic across time. 
- Recommendation policy (bandit/heuristics) to choose the next‑best question optimizing learning gain and coverage. 

### Serving & App Layer

- Inference API returning next question, updated mastery summary, and reason codes for transparency. 
- Android client with an on‑device cache of candidate questions and recent state to reduce latency and support limited offline use; logs sync when connectivity is available.
- Continuous improvement loop: logs → feature store → offline retraining → model registry → updated models deployed to inference service. 

***

## User Experience & Analytics

The learner and teacher experience focuses on transparency and actionable insights: 
- Topic mastery heatmap to visualize strong and weak areas at a glance. 
- Readiness score showing predicted exam performance with confidence band and trajectory over time. 
- Study plan with daily targets and spaced review to improve retention. 
- Instant feedback powered by explanation retrieval and similar examples per skill tag. 
- Teacher/coach dashboard to see class‑wide gaps, problematic questions, and content needing re‑teaching. 
***

## Benefits and Impact

The adaptive quiz coach aims to deliver: 

- Higher efficiency: fewer questions required to reach mastery using targeted selection.  
- Personalized paths: each learner gets a different, tailored sequence based on their mastery profile.  
- Better retention: automated spaced repetition informed by performance signals.  
- Transparent analytics: mastery heatmaps, readiness forecast, and improvement trends.  
- Fairness and quality: detection of ambiguous items, drift, and abnormal/cheating patterns from logs.  
- Higher engagement: flow‑state difficulty reduces boredom and frustration, increasing session completion.  
- Stronger explanations: error‑pattern‑aware remediation using similar examples and concept notes.  
- Scalable content growth: auto‑tagging and difficulty calibration allow faster integration of new questions.  



***

## Evaluation & Metrics

The project evaluates success at three levels: 

- Offline ML metrics  
  - AUC, log loss for models.  
  - Calibration error (ECE).  
  - Ranking metrics such as NDCG@k.  
- Learning outcomes (primary)  
  - A/B test adaptive vs fixed question order.  
  - Pre‑test → post‑test learning gain.  
  - Time‑to‑mastery and 7‑day retention.  
- Product metrics  
  - Session completion rate.  
  - Weekly active practice.  
  - Drop‑off after wrong answers.  

***

## Roadmap

Planned evolution of the system:

- MVP  
  - Manually defined tags and difficulty tiers.  
  - BKT‑based mastery updates.  
  - Heuristic ranking for next‑best question.  
  - Basic analytics and simple study plan.  

- V1  
  - IRT calibration from accumulated attempt logs.  
  - Bandit‑style policy for next‑best question selection.  
  - Question quality scoring to flag poor items.  

- V2  
  - DKT/Transformer‑based knowledge tracing (data‑dependent).  
  - NLP‑based auto‑tagging and grounded tutoring assistant.  
  - Controlled question generation with human‑in‑the‑loop review.  

***

## Repository Structure (Suggested)

You can adapt this to your actual code layout:

- `app/` – Android client code and UI flows.  
- `backend/` – APIs for question delivery, logging, and analytics.  
- `ml/` – training scripts for IRT, BKT/DKT, and recommendation policy.  
- `data/` – schemas for question bank, telemetry logs, and feature store.  
- `notebooks/` – experiments, offline evaluation, and A/B test analysis.  
- `docs/` – slides (this IDEATHON deck), diagrams, and design notes.  

***

## Getting Started (Conceptual)

Because this repository currently represents a design/ideation project, a minimal implementation path could be:

1. Stand up a simple backend with a question API and telemetry logging.  
2. Implement manual difficulty tiers, basic BKT, and heuristic next‑question selection.  
3. Log attempts and build a feature store schema.  
4. Train IRT and improved KT models offline, then integrate into the recommendation policy.  

