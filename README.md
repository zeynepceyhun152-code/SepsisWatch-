# SepsisWatch 🩺

**AI-powered sepsis prediction 6 hours before clinical onset — with strict pre-onset evaluation.**

> Built for Creator Colosseum Startup Competition 2026 | by Zeynep

---

## What is SepsisWatch?

SepsisWatch is a machine learning pipeline that predicts sepsis onset in ICU patients **6 hours early**, using only data available before any treatment begins. It achieves **AUC 0.9385 ± 0.0026** (5-fold CV) on 39,910 ICU patients — compared to Epic's real pre-onset AUC of 0.63.

The key difference: **honest evaluation**. Most sepsis AI tools (including Epic's) evaluate predictions after clinicians already suspect sepsis, which artificially inflates performance. SepsisWatch truncates all data at the first antibiotic order or blood culture — eliminating this leakage entirely.

---

## Results

| Metric | SepsisWatch | Epic (real-world) |
|---|---|---|
| AUC (pre-onset eval) | **0.9385 ± 0.0026** | 0.63 |
| False alert rate | TBD (threshold-dependent) | 18% of all patients |
| Dataset | PhysioNet/CinC 2019 (39,910 pts) | 800+ deployed hospitals |
| Evaluation method | Strict pre-onset truncation | Post-suspicion (inflated) |

---

## How It Works

1. **Data:** PhysioNet/Computing in Cardiology Challenge 2019 — 39,910 ICU patients across two hospital systems (training_setA + training_setB)

2. **Pre-onset labeling:** For every sepsis patient, all data after the first antibiotic order or blood culture is removed. This is the step most models skip.

3. **Feature engineering:** 281 features per patient from 40 clinical variables:
   - Mean, std, min, max, last value
   - Trend (change from admission to current hour)
   - Missingness rate (missing labs can signal clinical concern)
   - ICU length of stay aggregates

4. **Model:** XGBoost with `scale_pos_weight` to handle 6.3% sepsis class imbalance. Stratified 5-fold cross-validation.

5. **Explainability:** SHAP TreeExplainer on every prediction — clinicians see exactly which features triggered the alert.

---

## Top Predictors (SHAP)

- ICU length of stay trend
- FiO2 missingness
- Temperature (last value)
- Heart rate (last value)
- Respiratory rate (mean)

---

## Run It

Open in Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1w_effgy54QvJ93RBrL2L916brJA-rSSf?usp=sharing)

Or clone and run locally:

```bash
git clone https://github.com/YOUR_USERNAME/sepsiswatch.git
cd sepsiswatch
pip install -r requirements.txt
jupyter notebook sepsiswatch.ipynb
```

---

## Stack

- Python 3.10
- XGBoost
- scikit-learn
- SHAP
- pandas / numpy
- Google Colab Pro

---

## What's Next

- [ ] External validation on MIMIC-IV
- [ ] Real-time FHIR API integration prototype
- [ ] FDA SaMD Class II clearance pathway (510k)
- [ ] Hospital pilot partnership

---

## About

Built by **Zeynep** — 15-year-old AI/ML researcher from Turkey, incoming student at Thomas Jefferson School (Missouri). Research intern at DAYTAM since 2023. USAII Global AI Hackathon 2026: 3rd place (320+ teams). TEKNOFEST 2025: 3rd place internationally in AI in Health.

I've been afraid of hospitals since I was a kid. That's why I build AI for healthcare.

---

*Dataset: PhysioNet/CinC Challenge 2019 — available at physionet.org*
