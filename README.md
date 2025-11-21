# Vertigo Games – Data Analyst Case Study - Eren Keçeci
*Author:* Eren Keçeci  
*Version:* v1.0  

---

# 📌 Overview

This repository contains my complete solution to the two-part Vertigo Games Data Analyst Case Study:

- *Task 1:* A/B test modeling, retention simulation, DAU estimation, and monetization forecasting  
- *Task 2:* Exploratory Data Analysis (EDA) on player behavior, monetization patterns, skill segmentation, and long-term retention insights

All work is implemented in modular Jupyter Notebooks and uses reproducible data science methodology.

---

# 🧪 *TASK 1 — A/B Test Modeling & Simulation*

## 🎯 Purpose of Task 1
The goal is to model player retention and revenue for two variants (*A* and *B*) receiving identical daily installs. Using the provided retention, purchase, impression, and eCPM inputs, I simulate:

- Daily Active Users (DAU)
- Cumulative revenue (IAP + Ads)
- Impact of a temporary 10-day sale
- Impact of a new user source with new retention curves
- Overall comparison of variants under different conditions

The analysis answers questions (a)–(f) as requested.

---

# 🧠 Methodology (Task 1)

### *Retention Modeling*
- Raw retention values provided for *D1, D3, D7, D14*
- A complete daily retention curve (Day 1–30) is generated using:
  - *Linear interpolation* between anchor days  
  - *Exponential smoothing* for tail behavior  

### *DAU Simulation*
- Each variant receives *20,000 installs/day*
- Day N DAU is calculated using cohort survival:
  \[
  DAU_N = \sum (Installs_{day_i} \times Retention_{N-i})
  \]

### *Revenue Modeling*
Revenue is composed of:

#### *IAP Revenue*
\[
Revenue_{IAP} = DAU \times PurchaseRate \times ARPPU
\]

#### *Ad Revenue*
\[
Revenue_{Ads} = DAU \times Impressions \times \frac{eCPM}{1000}
\]

ARPPU is kept constant across variants since pricing is unchanged.

### *Sale Scenario (Day 15–24)*
- Purchase rate increased by *+1% absolute*

### *New User Source Scenario*
From Day 20 onward:
- 12,000 installs/day from original source
- 8,000 installs/day from new source  
- New user retention:
  - A_new: \( 0.58 \cdot e^{-0.12(x-1)} \)
  - B_new: \( 0.52 \cdot e^{-0.10(x-1)} \)

---

# 🔍 Assumptions (Task 1)
- ARPPU constant across both variants  
- Retention interpolation is representative of real decay  
- No seasonality or install irregularities  
- Ad impressions and purchase rates remain static except during sale period  
- New user source revenue only counted Days 20–30  

---

# 📊 Key Findings (Task 1)

## *(a) Which variant has the most Day 15 DAU?*  
✔ *Winner: Variant B*  
- B retains significantly better in the mid–late retention window (especially D14).
<img width="606" height="385" alt="Ekran Resmi 2025-11-21 03 58 46" src="https://github.com/user-attachments/assets/a96aef07-1151-47d8-8950-e77b4a1265a8" />


---

## *(b) Which variant earns the most total money by Day 15?*  
✔ *Winner: Variant A*  
- A serves much more ads per user (*2.3 vs 1.6 impressions*), giving it a large ad revenue advantage.
<img width="1624" height="978" alt="image" src="https://github.com/user-attachments/assets/201a1e12-46bb-47aa-860f-1377d674804b" />


---

## *(c) Which variant earns the most total money by Day 30?*  
✔ *Winner: Variant A*  
- B’s retention advantage is not enough to offset A’s stronger ad monetization over 30 days.
<img width="1682" height="982" alt="image" src="https://github.com/user-attachments/assets/0c9a19f4-4c3a-4635-8748-cc8f62b152fa" />


---

## *(d) Does a 10-day sale change the Day 30 outcome?*  
*Incremental Revenue (vs baseline):*  
- A: *+5,746.26*  
- B: *+6,285.16*  

✔ *Winner: Variant A*  
- B gains slightly more incremental IAP, but A’s total revenue remains higher.
<img width="1670" height="978" alt="image" src="https://github.com/user-attachments/assets/5b616aea-ea03-4679-b58d-fc7e343fa723" />

---

## *(e) With a new user source (Day 20+), which variant earns more by Day 30?*  
*Incremental Revenue (vs baseline):*  
- A: *+3,479.38*  
- B: *+2,866.11*  

✔ *Winner: Variant A*  
- New players age only 11 days before Day 30 → A’s stronger monetization dominates shorter windows.
<img width="1700" height="964" alt="image" src="https://github.com/user-attachments/assets/f81b9564-bd33-489b-bf70-8962461060a8" />

---

## *(f) If only one change can be made, which should be prioritized?*  
### ⭐ *Prioritize the new permanent user source*  
- The sale gives a temporary boost  
- A new user source gives:
  - Permanent DAU growth  
  - Long-term revenue compounding  
  - Healthier matchmaking pools  
  - More stable ad ecosystem  

---

# 📁 Task 1 Artifacts
- Vertigo_Task1_all.ipynb — full simulation  
- /charts — all visualizations  

---

# 🧪 *TASK 2 — User Behavior & Monetization Analysis*

## 🎯 Purpose of Task 2
Using 30 days of daily player logs (16 .csv.gz files), the objective is to understand:

- Engagement patterns  
- First-day behavior impact  
- Long-term retention by segment  
- Skill and platform differences  
- Country-level monetization  
- Session duration trends over time  
- Impact of server errors  
- Conversion funnels between segments  

This analysis provides actionable insights for Product, Growth, and Monetization teams.

---

# 🧠 Methodology (Task 2)

### *2.1 Data Preparation*
- Merged 16 compressed datasets  
- Parsed dates and normalized schema  
- Created:
  - days_since_install
  - total_revenue
  - win_rate
  - error_per_session
- Missing countries → "UNKNOWN"

---

### *2.2 First-Day Engagement Segmentation*
Users segmented using *D1 session duration quartiles*:
- Low  
- Mid  
- High  

Used for retention, revenue, and funnel analyses.

---

### *2.3 Retention Modeling*
Cohort-based retention calculated for:
- D1, D3, D7, D14, D30  

Across:
- segment × platform  
- segment × country  
- skill × platform  

Weighted by install counts.

---

### *2.4 Monetization Analysis*
- Per-user revenue  
- IAP vs Ad composition  
- Revenue by:
  - Segment  
  - Platform  
  - Skill group  
  - Country  

---

### *2.5 Behavior & Skill Analysis*
- Session duration trends  
- Session count  
- Match frequency  
- Win-rate quartile segmentation  
- Error impact on monetization  

---

### *2.6 Conversion Funnel (Low → Mid → High)*
- ~75% of D1-Low users convert to Mid/High by Day 2–3  
- These converted users show *2× D30 retention*  

---

# 🔍 Assumptions (Task 2)
- Retention computed on lifecycle (days_since_install)  
- Revenue = IAP + ad  
- Win-rate quartiles define skill  
- No-match users = "No_matches"  
- Missing countries grouped as "UNKNOWN"  
- Error-per-session undefined for zero-session users  

---

# 📊 Key Findings (Task 2)

### *1. First-day engagement predicts long-term value*
- High D1 users generate *2–3× more revenue*
- Low D1 users almost never retain beyond D7  
<img width="1396" height="944" alt="image" src="https://github.com/user-attachments/assets/da9004ef-6e65-4f09-957a-9fa20b36b3db" />
<img width="1442" height="956" alt="image" src="https://github.com/user-attachments/assets/34ce82c1-1f5c-4657-b554-2318dde2e66c" />

---

### *2. iOS users are more valuable*
- Higher retention  
- Significantly higher ARPU  
- Outperform Android across all segments  
<img width="1756" height="968" alt="image" src="https://github.com/user-attachments/assets/c4045c55-e771-4883-bc33-aec10c7a4212" />
<img width="1758" height="982" alt="image" src="https://github.com/user-attachments/assets/3eda2c04-4c97-48f2-b2ad-83f59530c1ab" />

---

### *3. Major country-level monetization differences*
- Brazil, Turkey, India → high volume, low ARPU (ad-driven)  
- US, Vietnam, Indonesia → strong IAP behavior  
<img width="1706" height="1162" alt="image" src="https://github.com/user-attachments/assets/11f8bcaa-f060-4b27-b350-3a67b68613b8" />
<img width="1810" height="1184" alt="image" src="https://github.com/user-attachments/assets/5c7e53ce-ae99-4649-b0e9-c0a0ac17ff52" />

---

### *4. Skill segmentation reveals optimal difficulty*
- Mid-skill players monetize the most  
- High-skill players churn slower but spend less  
<img width="1696" height="970" alt="image" src="https://github.com/user-attachments/assets/96f6e9ce-f09f-43a5-a0dc-a6899c201c7f" />

---

### *5. Server errors hurt monetization*
- Strong negative correlation between error rate and revenue  
- Android-low users most affected  
<img width="1802" height="984" alt="image" src="https://github.com/user-attachments/assets/1c8c9467-c3d7-4615-bf21-e6c010752202" />

---

### *6. Session duration declines steadily after Day 5*
- Decline sharper on Android  
- Indicates possible early-game fatigue  
<img width="2202" height="992" alt="image" src="https://github.com/user-attachments/assets/3189fce9-a3b0-49c8-ac5d-3f928f9a794d" />

---

### *7. Low → Mid Conversion Funnel*
- ~75% of Low D1 users transition to Mid/High by Day 2–3  
- Their D30 retention is *2× higher*  
- Major opportunity for onboarding & FTUE improvements  
<img width="2284" height="946" alt="image" src="https://github.com/user-attachments/assets/f65d6809-9f13-49e4-bc61-f8574a0a2b93" />

---

# 🖼️ Included Visuals
(Add screenshots to /images folder)
- Retention curves  
- Revenue distribution  
- Country heatmaps  
- Skill heatmaps  
- Conversion funnel graph  

---

# 🚀 Release Version
Final submission is tagged as:

*v1.0*

---

# 🎉 Thank You
This repository reflects my full end-to-end analytics workflow:  
retention modeling, monetization simulation, segmentation, cohort analysis, and actionable insights for real-world game optimization.

Feel free to reach out for questions or discussion!
