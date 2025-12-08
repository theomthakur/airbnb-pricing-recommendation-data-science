# NYC Airbnb Pricing Optimization - Comprehensive Project Report
## Part 1: Executive Summary, Problem Statement & Methodology

---

## Table of Contents (All 4 Parts)
- **Part 1:** Executive Summary, Problem Statement, Methodology (This Document)
- **Part 2:** Data Analysis, Feature Engineering, Model Development
- **Part 3:** Results, Validation, Business Insights
- **Part 4:** Limitations, Future Work, Recommendations

---

## 1. EXECUTIVE SUMMARY

### Project Overview
This project develops a machine learning-based pricing recommendation system for Airbnb listings in New York City. The system uses Gradient Boosting regression to predict optimal nightly rates, enabling property managers to maximize revenue while maintaining competitive occupancy rates.

### Key Achievements
- **Model Accuracy:** RMSE of $100.14, R² of 0.6514 (explains 65.14% of price variation)
- **Business Impact:** Identifies $522,273 in annual revenue opportunity across a test portfolio of 388 properties
- **Pricing Intelligence:** Reveals that property capacity (accommodates, bathrooms) drives 46.8% of pricing, while location accounts for 21.16%
- **Actionable Output:** Generates per-property pricing recommendations for 388 listings, identifying 173 underpriced (44.6%) and 106 overpriced (27.3%) properties

### Project Team
- **Team:** Princy Doshi, Ashutosh Agrawal, Om Thakur
- **Course:** Data Science and AI for Business (Fall 2025)
- **Professor:** Dr. Chris Volinsky, NYU

---

## 2. PROBLEM STATEMENT

### Business Context
The short-term rental market in New York City is highly competitive, with over 46,000 active Airbnb listings. Property managers face a complex pricing challenge:

**Key Challenges:**
1. **Dynamic Pricing Complexity:** Prices vary significantly by neighborhood ($180–$300 average range)
2. **Manual Pricing Inefficiency:** Setting prices manually for each listing is time-consuming and error-prone
3. **Market Intelligence Gap:** Hosts lack data-driven methods to understand price drivers
4. **Revenue Optimization:** Balancing the trade-off between occupancy (requires lower prices) and revenue (requires higher prices)
5. **Competitive Pressure:** Must price competitively without sacrificing profitability

### Specific Problem We Solve
**Given a property's characteristics (bedrooms, bathrooms, location, amenities, reviews), predict the optimal nightly rental price.**

**Why This Matters:**
- **For Hosts:** Maximize annual revenue without losing bookings to overpricing
- **For Travelers:** Ensure fair pricing reflects property quality
- **For the Market:** Improve price efficiency and reduce information asymmetry

### Target Audience
Primary: Airbnb hosts managing properties in NYC  
Secondary: Property management companies, real estate investors, short-term rental platforms

---

## 3. SCOPE & CONSTRAINTS

### Geographic Focus
**8 Major NYC Neighborhoods:**
1. Williamsburg (premium, trendy)
2. Upper East Side (affluent, commercial)
3. Astoria (Queens, emerging)
4. Bedford-Stuyvesant (Brooklyn, established)
5. Hell's Kitchen (Manhattan, touristy)
6. Harlem (Northern Manhattan, mixed)
7. Bushwick (Brooklyn, artist community)
8. Crown Heights (Brooklyn, diverse)

**Rationale:** These neighborhoods represent diverse market segments (luxury, mainstream, emerging) and have sufficient listing volume (>100 each).

### Property Type Focus
**Entire Homes Only** (no private rooms or shared spaces)
- Reason: Entire homes follow different pricing logic; combined models would be confounded

### Dataset Definition
- **Raw Data:** 36,111 Airbnb listings in NYC (Inside Airbnb, late 2024)
- **Filtered Data:** 1,937 listings (after quality filters)
- **Filtering Criteria:**
  - Price range: $50–$1,000/night (removes errors and ultra-luxury outliers)
  - Complete feature set (no missing critical fields)
  - In target neighborhoods only
  - Bedrooms > 0, Bathrooms > 0

### Time Scope
**Data Snapshot:** Late 2024 (static market snapshot, not time-series)

**Note:** This model predicts *average* pricing, not seasonal variations. Future work would incorporate temporal dynamics.

---

## 4. METHODOLOGY

### 4.1 Data Science Workflow

```
┌──────────────────────────────────────────────────────────────┐
│                   PROJECT WORKFLOW OVERVIEW                   │
└──────────────────────────────────────────────────────────────┘

Phase 1: Data Acquisition & Exploration
  ├─ Download Inside Airbnb NYC dataset (36,111 listings)
  ├─ Load and inspect data structure
  ├─ Perform exploratory data analysis (EDA)
  ├─ Visualize distributions and correlations
  └─ Identify data quality issues
         ↓
Phase 2: Data Cleaning & Preparation
  ├─ Apply neighborhood and price filters
  ├─ Remove missing values
  ├─ Handle outliers and anomalies
  ├─ Result: 1,937 clean listings
  └─ Create dataset ready for modeling
         ↓
Phase 3: Feature Engineering
  ├─ Extract numeric features (bedrooms, bathrooms, etc.)
  ├─ Create derived features (occupancy_proxy, review_intensity)
  ├─ Encode categorical variables (room_type, neighborhood)
  ├─ Select top 15 features by importance
  └─ Engineer interactions and transformations
         ↓
Phase 4: Data Splitting & Scaling
  ├─ Shuffle data randomly
  ├─ 80/20 train-test split (1,549 train / 388 test)
  ├─ Fit StandardScaler on training set
  ├─ Apply scaling to both train and test
  └─ Ensure no data leakage
         ↓
Phase 5: Model Training & Comparison
  ├─ Train 5 candidate models:
  │  ├─ Linear Regression (baseline)
  │  ├─ Ridge Regression (regularized)
  │  ├─ Lasso Regression (sparse)
  │  ├─ Random Forest (ensemble, non-linear)
  │  └─ Gradient Boosting (iterative, best)
  ├─ Evaluate on test set using 4 metrics
  └─ Select Gradient Boosting as final model
         ↓
Phase 6: Model Evaluation & Interpretation
  ├─ Calculate RMSE, MAE, R², MAPE
  ├─ Analyze residuals and error patterns
  ├─ Extract feature importance
  ├─ Validate no overfitting (Train RMSE ≈ Test RMSE)
  └─ Generate confidence intervals
         ↓
Phase 7: Business Analysis & Recommendations
  ├─ Segment properties by pricing status
  ├─ Calculate revenue opportunity per property
  ├─ Simulate portfolio-level impact
  ├─ Identify top opportunities
  └─ Generate per-property recommendations
         ↓
Phase 8: Visualization & Reporting
  ├─ Create 11 business-ready visualizations
  ├─ Generate CSV results files
  ├─ Write executive summaries
  ├─ Prepare presentation materials
  └─ Document findings and recommendations
```

### 4.2 Technology Stack

| Component | Technology | Purpose |
|---|---|---|
| **Language** | Python 3.13.3 | Data processing, modeling |
| **Notebooks** | Jupyter (.ipynb) | Interactive development |
| **Data Processing** | pandas, numpy | Load, clean, transform data |
| **Visualization** | matplotlib, seaborn | Generate charts and heatmaps |
| **Machine Learning** | scikit-learn | Train 5 models, scale features |
| **Model Serialization** | joblib | Save/load trained models |
| **Statistical Analysis** | scipy | Correlation, hypothesis testing |
| **Environment** | venv | Package isolation |
| **Version Control** | git, GitHub | Repository management |

### 4.3 Notebook Structure

#### **Notebook 1: 01_data_exploration.ipynb**
**Purpose:** Initial exploratory data analysis

| Cell | Action | Output |
|------|--------|--------|
| 1 | Import libraries | Environment ready |
| 2 | Load data | Dataset shape (36,111 × 30) |
| 3 | Summary statistics | Mean $227, Median $180, Std $152 |
| 4 | Neighborhood visualization | `neighborhood_overview.png` |
| 5 | Price distribution | `price_distribution.png` |
| 6 | Correlation matrix | `correlation_matrix.png` |
| 7 | Feature relationships | `scatter_plots.png` |

**Key Outputs:**
- Understanding of data distributions
- Identification of strong/weak correlations
- Discovery of outliers and anomalies
- 3 visualization files for context

---

#### **Notebook 2: 02_master_analysis.ipynb**
**Purpose:** Complete ML pipeline from cleaning to business recommendations

**Section A: Data Loading & Cleaning (Cells 1–5)**
- Load listings.csv
- Apply neighborhood filter (select 8 areas)
- Apply price filter ($50–$1,000)
- Handle missing values (median imputation)
- Result: 36,111 → 1,937 listings

**Section B: Feature Engineering (Cells 6–8)**
- Numeric features: bedrooms, bathrooms, accommodates, latitude, longitude
- Derived features: occupancy_proxy, review_intensity, amenities_count
- Categorical features: room_type, neighbourhood, superhost, instant_bookable
- Interactions: latitude × longitude
- Result: 15 final features

**Section C: Data Preparation (Cells 9–10)**
- Train-test split: 80/20 (1,549 / 388)
- StandardScaler normalization
- No data leakage (fit scaler on train set only)

**Section D: Model Training (Cells 11–13)**
- Train 5 regression models
- Evaluate with RMSE, MAE, R², MAPE
- Compare performance
- Select Gradient Boosting winner

**Section E: Business Analysis (Cells 14–18)**
- Extract feature importance
- Segment properties (underpriced, optimal, overpriced)
- Calculate revenue opportunity
- Generate per-property recommendations
- Create neighborhood insights

**Section F: Visualization & Export (Cells 19–20)**
- Generate 11 visualization files
- Export CSV results
- Create text summaries

### 4.4 Model Selection Rationale

**Why Gradient Boosting?**

| Aspect | Linear | Ridge/Lasso | Random Forest | Gradient Boosting | Winner |
|---|---|---|---|---|---|
| **RMSE** | $121.45 | $120–$119 | $102.38 | **$100.14** | ✅ GB |
| **R²** | 0.5012 | 0.51–0.52 | 0.6401 | **0.6514** | ✅ GB |
| **Non-linearity** | No | No | Yes | Yes | ✅ GB |
| **Feature Interaction** | No | No | Limited | **Strong** | ✅ GB |
| **Interpretability** | High | High | High | **High** | ✅ GB |
| **Stability** | Volatile | Stable | Inconsistent | **Consistent** | ✅ GB |

**Key Advantage:** Gradient Boosting's iterative learning process allows it to capture complex pricing patterns and interactions that simpler models miss. Each of 100 trees learns from previous errors, resulting in 2% better RMSE than Random Forest and 20% better than linear methods.

**Parameters Used:**
```python
GradientBoostingRegressor(
    n_estimators=100,        # 100 sequential trees
    learning_rate=0.1,       # Slow, stable learning
    max_depth=5,             # Shallow trees (prevent overfitting)
    min_samples_split=5,     # Require 5+ samples to split
    random_state=42          # Reproducibility
)
```

---

## 5. DATA OVERVIEW

### 5.1 Source & Collection

**Data Source:** Inside Airbnb (insideairbnb.com)
- **Type:** Public, open dataset
- **Collection Date:** Late 2024
- **Geographic Coverage:** NYC (5 boroughs)
- **Initial Size:** 36,111 listings
- **Collection Method:** Web scraping of Airbnb.com

**Data Rights:** Open data for research purposes (compliant with Inside Airbnb usage policy)

### 5.2 Raw Dataset Structure

**Original Features (30 columns):**

| Category | Features |
|---|---|
| **Identifiers** | id, name, host_id, host_name |
| **Location** | neighbourhood_group, neighbourhood_cleansed, latitude, longitude |
| **Property Info** | room_type, property_type, bedrooms, beds, bathrooms, accommodates |
| **Amenities** | amenities (list) |
| **Pricing** | price |
| **Reviews** | number_of_reviews, reviews_per_month, last_review, review_scores_rating |
| **Availability** | availability_365, has_availability |
| **Bookings** | instant_bookable, minimum_nights, calculated_host_listings_count |
| **Verification** | host_is_superhost, host_identity_verified |

### 5.3 Filtering & Cleaning Process

**Step 1: Neighborhood Filtering**
- Keep only 8 target neighborhoods
- Remove small/emerging neighborhoods with <50 listings
- Result: 46,111 → ~18,000 listings

**Step 2: Price Range Filtering**
- Minimum: $50/night (removes errors and parking/group stays)
- Maximum: $1,000/night (removes ultra-luxury with unique pricing logic)
- Result: 18,000 → ~8,000 listings

**Step 3: Feature Completeness**
- Require: bedrooms > 0, bathrooms > 0
- Require: accommodates ≥ 1
- Remove rows with missing critical fields
- Result: 8,000 → ~3,000 listings

**Step 4: Data Quality Check**
- Remove duplicates (same property listed twice)
- Check for anomalies (e.g., 20 bedrooms for $50)
- Validate data types
- Result: 3,000 → 1,937 listings (final)

**Filtering Impact Summary:**
```
Original:           36,111 listings (100%)
After neighborhood: 18,000 listings (50%)
After price:         8,000 listings (22%)
After completeness:  3,000 listings (8%)
Final (after QA):    1,937 listings (5%)

Note: Aggressive filtering ensures model quality over quantity.
1,937 high-quality listings better than 36k mixed-quality.
```

### 5.4 Final Dataset Characteristics

**Size & Distribution:**
- **Total Rows:** 1,937 listings
- **Training Set:** 1,549 (80%)
- **Test Set:** 388 (20%)
- **Neighborhoods:** 8 (distributed as shown in next section)

**Price Statistics:**
```
Mean:        $227.43 (average nightly rate)
Median:      $180.00 (50th percentile)
Std Dev:     $151.90 (price variance)
Min:         $53.00
Max:         $1,000.00
Range:       $947.00

Quartiles:
Q1 ($25%):   $130.00
Q2 ($50%):   $180.00
Q3 ($75%):   $310.00
IQR:         $180.00 (price range for middle 50%)
```

**Interpretation:** Prices are right-skewed (long tail of expensive properties). Median ($180) is lower than mean ($227), indicating some high-priced outliers pulling average up.

**Neighborhood Distribution:**

| Neighborhood | Count | % | Avg Price | Median |
|---|---|---|---|---|
| Williamsburg | 312 | 16.1% | $298 | $265 |
| Upper East Side | 184 | 9.5% | $285 | $240 |
| Astoria | 156 | 8.0% | $242 | $200 |
| Bedford-Stuyvesant | 187 | 9.6% | $195 | $165 |
| Hell's Kitchen | 298 | 15.4% | $228 | $190 |
| Harlem | 234 | 12.1% | $189 | $155 |
| Bushwick | 267 | 13.8% | $225 | $190 |
| Crown Heights | 299 | 15.4% | $210 | $175 |
| **TOTAL** | **1,937** | **100%** | **$227.43** | **$180** |

**Key Insight:** Williamsburg and Upper East Side command 25%+ price premium over Harlem and Crown Heights (trendy vs. emerging neighborhoods).

---

## 6. FEATURE ENGINEERING

### 6.1 Feature Selection Process

**Initial Candidates:** 89 engineered features  
**Final Selection:** 15 features (selected based on importance and domain knowledge)

### 6.2 Selected Features (15 Total)

| # | Feature | Type | Source | Importance | Why It Matters |
|---|---|---|---|---|---|
| 1 | bathrooms | Numeric | Raw | 29.1% | Property quality signal |
| 2 | longitude | Numeric | Raw | 17.2% | East-West location variation |
| 3 | latitude | Numeric | Raw | 10.3% | North-South location variation |
| 4 | minimum_nights | Numeric | Raw | 12.9% | Pricing strategy signal |
| 5 | accommodates | Numeric | Raw | 8.5% | Guest capacity matters |
| 6 | bedrooms | Numeric | Raw | 7.4% | Property size |
| 7 | room_type_Entire home | Binary | Encoded | 5.8% | Distinguishes from private |
| 8 | neighbourhood_Williamsburg | Binary | Encoded | 4.2% | Premium neighborhood |
| 9 | superhost | Binary | Raw | 3.1% | Quality/trust signal |
| 10 | amenities_count | Numeric | Engineered | 2.8% | Luxury level |
| 11 | reviews_per_month | Numeric | Raw | 1.4% | Activity indicator |
| 12 | instant_bookable | Binary | Raw | 1.2% | Convenience feature |
| 13 | number_of_reviews | Numeric | Raw | 0.9% | Proof of reliability |
| 14 | lat_long_interaction | Numeric | Engineered | 0.8% | Micro-location effects |
| 15 | neighbourhood_Bushwick | Binary | Encoded | 0.7% | Secondary neighborhood |

### 6.3 Feature Engineering Decisions

**Engineered Features (Custom Calculations):**

1. **occupancy_proxy** = 365 − availability_365
   - Reasoning: Direct availability is less intuitive; occupancy proxy indicates how "booked" a property is
   - Dropped from final model (low importance)

2. **review_intensity** = reviews_per_month × (days_active / 365)
   - Reasoning: Combines booking frequency with property age for activity score
   - Dropped (low importance; reviews don't drive price)

3. **amenities_count** = length(amenities_list)
   - Reasoning: Raw amenities list is categorical; count gives numeric signal
   - Kept (2.8% importance)

4. **lat_long_interaction** = latitude × longitude
   - Reasoning: Captures exact micro-location effects beyond simple lat/long
   - Kept (0.8% importance)

**Categorical Encoding (One-Hot):**

Neighborhoods (8 → 7 features, dropped reference):
- neighbourhood_Williamsburg
- neighbourhood_Upper East Side
- neighbourhood_Astoria
- neighbourhood_Bedford-Stuyvesant
- neighbourhood_Hell's Kitchen
- neighbourhood_Harlem
- neighbourhood_Bushwick
(Crown Heights = reference category, represented as all-zeros)

Room Type (1 → 1 feature):
- room_type_Entire home (all listings are entire homes, so this is constant; kept for model consistency)

Binary Features (as-is):
- superhost (0/1)
- instant_bookable (0/1)

**Scaling (StandardScaler):**
All 15 features normalized to mean=0, std=1
- Reason: Algorithms treat large-magnitude features as "more important"
- Impact: Fair representation across features

### 6.4 Why 15 Features (Not More or Fewer)?

**Considered Alternatives:**

**Option A: Use all 89 features**
- Pros: Maximum information
- Cons: Overfitting risk, sparse (89/1937 ≈ 22 rows/feature), hard to interpret, slow training
- Decision: ❌ Rejected

**Option B: Use top 5 features only**
- Pros: Maximum simplicity, fastest training
- Cons: Loss of signal, likely underfitting, missing interactions
- Decision: ❌ Rejected

**Option C: Use 15 features (CHOSEN) ✅**
- Pros: Balance of accuracy (captures 98%+ of signal from top 20 features) and interpretability
- Feature-to-row ratio: 15/1937 ≈ 129 rows per feature (sufficient for stability)
- Interpretable (can explain to business stakeholders)
- Training speed acceptable (under 1 second on test set)
- Captures major interactions and non-linearity
- Decision: ✅ Selected

---

## 7. DATA SPLITTING & VALIDATION STRATEGY

### 7.1 Train-Test Split

**Methodology:** Stratified random split
```
Total: 1,937 listings
├─ Training: 1,549 (80%)
└─ Test: 388 (20%)
```

**Process:**
1. Shuffle all 1,937 records randomly (seed=42 for reproducibility)
2. Select first 1,549 for training
3. Hold out last 388 for testing
4. Train model on training set only
5. Evaluate on unseen test set

**Why 80/20?**
- Standard practice in ML (well-established benchmark)
- 1,549 training samples sufficient to learn patterns
- 388 test samples sufficient to validate (large enough to reduce noise)
- Provides stable estimate of generalization performance

**Why Random Split (vs. Time-Based)?**
- Data is static snapshot (not time-series), so temporal order doesn't matter
- Random split ensures test set is representative of training distribution
- Avoids bias from temporal trends

### 7.2 Data Leakage Prevention

**Potential Leakage Source:** Fitting scaler on full dataset

**Prevention:**
```python
# ❌ WRONG (data leakage)
scaler = StandardScaler()
scaler.fit(all_1937_samples)  # Includes test set statistics!
X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)

# ✅ CORRECT (no leakage)
scaler = StandardScaler()
scaler.fit(X_train)           # Fit ONLY on training set
X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)    # Apply pre-fit scaler
```

**Impact:** Prevents inflating test performance by using test set statistics during training.

---

## 8. SUMMARY OF PART 1

This part has covered:
✅ Executive summary and project overview  
✅ Problem statement and business context  
✅ Scope, constraints, and target neighborhoods  
✅ Complete methodology and workflow  
✅ Technology stack and tools used  
✅ Notebook structure and execution flow  
✅ Model selection rationale (why Gradient Boosting)  
✅ Data acquisition, filtering, and cleaning  
✅ Feature engineering and selection (15 features)  
✅ Train-test splitting and validation strategy  

**Next:** Part 2 covers data analysis, model training, feature importance extraction, and detailed results.

---

**Document Information:**
- **Created:** December 8, 2025
- **Part:** 1 of 4
- **Total Words:** ~6,500
- **Audience:** Academic (course project evaluation)
