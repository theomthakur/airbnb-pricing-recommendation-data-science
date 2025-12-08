# NYC Airbnb Pricing Optimization - Project Documentation
## Part 1: Project Structure, Workflow, and Technical Architecture

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Directory Structure](#directory-structure)
3. [Data Pipeline](#data-pipeline)
4. [File-by-File Breakdown](#file-by-file-breakdown)
5. [Technology Stack](#technology-stack)
6. [Workflow Explanation](#workflow-explanation)
7. [Key Design Decisions](#key-design-decisions)

---

## Project Overview

### What is this project?
This is a **supervised machine learning project** that builds a regression model to predict optimal nightly prices for Airbnb listings in New York City. The project takes raw Inside Airbnb data, cleans and engineers features from it, trains multiple regression models, and outputs business-ready pricing recommendations.

### Why does it matter?
- **Problem:** Airbnb hosts struggle to price listings competitively while maximizing revenue
- **Solution:** A machine learning model that recommends optimal nightly prices based on property features, location, and market dynamics
- **Impact:** Unlocks $522k+ in annual revenue opportunity for a test portfolio of 388 properties

### Project Goals
1. **Predictive:** Accurately estimate nightly rental prices given property features
2. **Prescriptive:** Recommend specific price adjustments for hosts to increase revenue
3. **Interpretable:** Explain *why* certain features drive pricing (feature importance)
4. **Reproducible:** Automate the entire pipeline for future data updates

---

## Directory Structure

```
Project/
│
├── README.md                                 # Project overview and quick start guide
│
├── data/                                     # Raw data directory
│   ├── listings.csv                         # Raw Airbnb listing data (36,111 rows)
│   ├── reviews.csv                          # Guest reviews metadata
│   ├── neighbourhoods.csv                   # NYC neighborhood information
│   ├── neighbourhoods.geojson               # Neighborhood geospatial boundaries
│   └── cleaned_dataset.csv                  # Processed data (1,937 rows, 15 features)
│
├── notebook/                                 # Jupyter notebooks (execution environment)
│   ├── 01_data_exploration.ipynb            # EDA and data understanding
│   ├── 02_master_analysis.ipynb             # Complete ML pipeline
│   └── [Generated outputs during execution] # CSVs, PNGs, TXTs (generated, not committed)
│
├── outputs/                                  # Organized output directory
│   ├── results/                             # Model outputs and metrics
│   │   ├── model_results.csv                # 5 models compared with RMSE, MAE, R²
│   │   ├── feature_importance.csv           # Ranked importance of 15 features
│   │   ├── key_statistics.csv               # Portfolio summary metrics
│   │   ├── key_statistics.txt               # Human-readable statistics
│   │   ├── neighborhood_insights.csv        # 8-neighborhood analysis
│   │   ├── pricing_recommendations.csv      # Per-property price recommendations
│   │   └── cleaned_dataset.csv              # Final processed data
│   │
│   └── visualizations/                      # Model and business charts
│       ├── neighborhood_overview.png        # Top 15 neighborhoods by metrics
│       ├── price_distribution.png           # Histogram and statistics
│       ├── price_by_neighborhood_box.png    # Box plots by area
│       ├── correlation_matrix.png           # Feature correlation heatmap
│       ├── scatter_plots.png                # Key feature relationships
│       ├── model_comparison.png             # 5-model performance chart
│       ├── actual_vs_predicted.png          # Regression fit plot
│       ├── feature_importance.png           # Bar chart of feature importance
│       ├── business_impact.png              # Revenue opportunity visualization
│       ├── neighborhood_insights.png        # Neighborhood-level analysis
│       └── error_analysis.png               # Residual diagnostics
│
└── presentations/                            # Presentation artifacts
    ├── PRESENTATION_GUIDE_AND_SCRIPT.md     # 12-slide presentation with speaker scripts
    ├── PRESENTER_QUICK_REFERENCE.md         # 1-page cheat sheet for presenters
    ├── PROJECT_DOCUMENTATION_PART1.md       # This file (structure & workflow)
    ├── PROJECT_DOCUMENTATION_PART2.md       # Technical details & Q&A
    ├── DataProject_Rubric.pdf               # Course grading rubric
    ├── Project Presentation FAQ.pdf         # Course presentation guidelines
    └── Syllabus_Fall25.pdf                  # Course syllabus

```

---

## Data Pipeline

### High-Level Flow
```
┌─────────────────────────────────────────────────────────────────┐
│                     DATA ACQUISITION                             │
│  Inside Airbnb NYC (listings.csv, reviews.csv, neighbourhoods)  │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                  EXPLORATORY DATA ANALYSIS                       │
│  (01_data_exploration.ipynb)                                     │
│  • Load data, inspect shapes and types                           │
│  • Visualize price distributions by neighborhood                 │
│  • Identify data quality issues                                  │
│  • Understand feature correlations                               │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│               MASTER DATA SCIENCE PIPELINE                       │
│              (02_master_analysis.ipynb)                          │
│                                                                   │
│  Step 1: Data Cleaning (36,111 → 1,937 rows)                   │
│  • Filter by neighborhoods (8 major areas)                       │
│  • Remove outliers (price < $50, > $1000)                       │
│  • Handle missing values (median imputation)                     │
│                                                                   │
│  Step 2: Feature Engineering (15 features)                      │
│  • Numeric: beds, baths, accommodates, lat, long                │
│  • Calculated: occupancy_proxy, review_intensity                │
│  • Categorical: room_type, neighbourhood, superhost             │
│                                                                   │
│  Step 3: Train-Test Split & Scaling                             │
│  • 80/20 split: 1,549 train + 388 test                          │
│  • StandardScaler normalization                                  │
│                                                                   │
│  Step 4: Model Training (5 algorithms)                          │
│  • Linear Regression (baseline)                                  │
│  • Ridge & Lasso (regularized linear)                           │
│  • Random Forest (ensemble, non-linear)                         │
│  • Gradient Boosting (iterative, best performer)                │
│                                                                   │
│  Step 5: Evaluation & Comparison                                │
│  • Metrics: RMSE, MAE, R², MAPE                                 │
│  • Gradient Boosting wins: RMSE $100.14, R² 0.6514             │
│                                                                   │
│  Step 6: Interpretation & Business Analysis                     │
│  • Extract feature importance (bathrooms 29%, location 17%)      │
│  • Segment pricing (44.6% underpriced, 27.3% overpriced)        │
│  • Calculate revenue opportunity ($522k)                         │
│  • Generate per-property recommendations                         │
│                                                                   │
│  Step 7: Visualization & Reporting                              │
│  • Create 11 business-ready charts                              │
│  • Export metrics and recommendations to CSV                     │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                  OUTPUT & DELIVERY                               │
│  • Model results (CSV, TXT, PNG)                                │
│  • Pricing recommendations (per-property)                       │
│  • Presentation & speaker scripts                               │
│  • Documentation (this file)                                    │
└─────────────────────────────────────────────────────────────────┘
```

---

## File-by-File Breakdown

### 1. **data/listings.csv** (Raw Input)
**What it is:** Primary dataset from Inside Airbnb NYC  
**Size:** 36,111 rows × ~30 columns (raw)  
**Key columns used:**
- `name, id`: Property identifier
- `neighbourhood_group, neighbourhood_cleansed`: Location
- `room_type`: Entire home, private room, shared room
- `price`: Nightly rate (target variable)
- `bedrooms, beds, bathrooms`: Property capacity
- `accommodates`: Number of guests
- `amenities`: Count of available amenities
- `reviews_per_month, number_of_reviews`: Booking history
- `minimum_nights`: Minimum stay requirement
- `availability_365`: Days available per year
- `latitude, longitude`: Geospatial location

**Significance:** Raw truth data; contains all information used for modeling

---

### 2. **data/reviews.csv** (Supplementary)
**What it is:** Metadata on guest reviews  
**Size:** 1M+ reviews for all NYC properties  
**Used for:**
- Calculating `reviews_per_month` and `number_of_reviews` aggregates
- Understanding booking patterns (proxy for demand)

**Significance:** Indirectly feeds feature engineering; shows which properties are actively booked

---

### 3. **data/neighbourhoods.csv & neighbourhoods.geojson**
**What they are:**
- CSV: Neighborhood reference data with names and IDs
- GeoJSON: Geospatial boundaries for mapping

**Used for:**
- Mapping listings to neighborhoods
- Grouping analysis by area
- Visualization (though not used in final model)

**Significance:** Contextual data for location-based analysis

---

### 4. **notebook/01_data_exploration.ipynb**
**Purpose:** Initial exploratory data analysis  
**7 Code Cells:**

| Cell | Action | Output |
|------|--------|--------|
| 1 | Import libraries (pandas, numpy, matplotlib, seaborn) | Notebook ready |
| 2 | Load listings.csv and neighborhoods.csv | Display shape: (36,111, ~30) |
| 3 | Summary statistics (mean, median, std, quantiles) | Console output |
| 4 | Price distribution by neighborhood (bar chart) | `neighborhood_overview.png` |
| 5 | Box plots of price by room type and neighborhood | Visual inspection |
| 6 | Correlation matrix of numeric features | `correlation_matrix.png` |
| 7 | Scatter plots (price vs capacity, price vs amenities) | `scatter_plots.png` |

**Key Insights Generated:**
- Mean price: $227/night; Median: $180/night
- Price range: $50–$1,000+ (right-skewed distribution)
- Location (neighborhood) is a strong price predictor
- Amenities and capacity show positive correlation with price

**Output:** Console logs + 3 visualizations (no CSV exports)

**Why run this?**
- Understand data before modeling
- Validate data quality
- Generate initial business insights
- Identify outliers to filter

---

### 5. **notebook/02_master_analysis.ipynb** (The Core Pipeline)
**Purpose:** Complete machine learning workflow  
**18+ Code Cells:** (Below is the logical flow)

#### **Section A: Data Loading & Cleaning (Cells 1-5)**

| Cell | Action | Input → Output | Significance |
|------|--------|----------------|---|
| 1 | Import all libraries | - | Brings in pandas, numpy, matplotlib, seaborn, sklearn, scipy, joblib |
| 2 | Load listings.csv | 36,111 rows | Raw data ingestion |
| 3 | Create neighborhood filter | Available neighborhoods | Focuses analysis on 8 major areas |
| 4 | Apply filters | 36,111 → 1,937 | Removes: low-revenue areas, outlier prices, missing core features |
| 5 | Handle missing values | Missing bedrooms/bathrooms | Median imputation strategy |

**Filtering Logic:**
```python
# Keep listings where:
- neighborhood_cleansed in [selected 8 areas]
- price >= $50 (avoid noise, extreme low prices)
- price <= $1000 (remove ultra-luxury outliers that aren't predictable)
- bedrooms > 0 and bathrooms > 0 (property fundamentals)
- accommodates >= 1
```

**Why filter?**
- Focus on mainstream market (exclude both bargain and ultra-luxury)
- Remove data quality issues
- Ensure sufficient training examples per feature (1,937 ÷ 15 features ≈ 129 per feature)
- Improve model generalization

**Output:** Cleaned dataset with 1,937 rows

---

#### **Section B: Feature Engineering (Cells 6-8)**

| Feature | Type | Calculation/Source | Why It Matters |
|---------|------|---|---|
| `bedrooms` | Numeric | From raw data | More beds → higher price |
| `bathrooms` | Numeric | From raw data | **29% feature importance**—top driver |
| `accommodates` | Numeric | From raw data | Larger parties pay premium |
| `latitude` | Numeric | From raw data | **17% importance**—North vs South |
| `longitude` | Numeric | From raw data | East-West variation (Williamsburg > Bushwick) |
| `amenities_count` | Engineered | Count of amenities list | More luxuries → higher price |
| `occupancy_proxy` | Engineered | 365 − availability_365 | Proxy for demand (if booked often, hosts can charge more) |
| `review_intensity` | Engineered | reviews_per_month × days_in_listing | Activity level and property appeal |
| `room_type` | Categorical | From raw data | Entire home > private room > shared |
| `neighbourhood` | Categorical | From raw data | Neighborhood premium (Williamsburg > Astoria) |
| `superhost` | Binary | From raw data | Quality signal |
| `instant_bookable` | Binary | From raw data | Convenience feature |
| `minimum_nights` | Numeric | From raw data | Affects pricing strategy |
| `number_of_reviews` | Numeric | From raw data | Booking history and proof of quality |
| `latitude` × `longitude` | Interaction | Geospatial product | Captures exact location effect |

**Feature Selection Rationale:**
- **Dropped:** review_sentiment (not in data), availability_365 (replaced by occupancy_proxy)
- **Kept:** 15 engineered features (balance interpretability with predictive power)
- **Reasoning:** More features = more noise; fewer features = loss of signal. 15 is the sweet spot.

**Output:** DataFrame with 15 features + target variable (price)

---

#### **Section C: Train-Test Split & Scaling (Cells 9-10)**

| Step | Action | Data |
|------|--------|------|
| 1 | Shuffle 1,937 rows | Randomize order |
| 2 | 80/20 split | 1,549 train + 388 test |
| 3 | Separate features (X) and target (y) | X: 15 features, y: nightly price |
| 4 | Fit StandardScaler on training set | Learn mean & std |
| 5 | Apply scaler to train & test | Normalize to mean=0, std=1 |

**Why StandardScaler?**
- Algorithms like Ridge, Lasso, Random Forest are sensitive to feature magnitude
- Without scaling, high-magnitude features (lat/long) dominate
- Scaling ensures each feature contributes fairly

**Output:** X_train, X_test, y_train, y_test (all scaled)

---

#### **Section D: Model Training & Evaluation (Cells 11-13)**

| Model | Algorithm | RMSE | MAE | R² | Why? |
|-------|-----------|------|-----|----|----|
| Linear Regression | y = mx + b | $121.45 | $78.92 | 0.5012 | Baseline; assumes linear pricing |
| Ridge | Linear + L2 penalty | $120.89 | $78.45 | 0.5089 | Prevents overfitting on correlated features |
| Lasso | Linear + L1 penalty | $119.23 | $77.89 | 0.5201 | Sparse (zeros out weak features) |
| Random Forest | 100 decision trees | $102.38 | $65.87 | 0.6401 | Non-linear; captures interactions |
| **Gradient Boosting** | **Iterative trees (winner)** | **$100.14** | **$64.20** | **0.6514** | **Iterative refinement; best at capturing nuance** |

**Training Details:**
```python
Models = [
  LinearRegression(),
  Ridge(alpha=1.0),
  Lasso(alpha=0.1),
  RandomForestRegressor(n_estimators=100, random_state=42),
  GradientBoostingRegressor(n_estimators=100, learning_rate=0.1, random_state=42)
]

For each model:
  1. Fit on training data
  2. Predict on test data
  3. Calculate RMSE, MAE, R²
  4. Store results in model_results.csv
```

**Why Gradient Boosting Won:**
- **Iterative learning:** Each tree learns from previous errors
- **Non-linear:** Captures complex pricing patterns (e.g., amenities have diminishing returns)
- **Feature interaction:** Discovers that bathrooms + neighborhood matter more together
- **Performance:** $100 RMSE on $227 mean price = 44% error (acceptable for real estate)

**Output:** model_results.csv with all metrics

---

#### **Section E: Feature Importance & Business Analysis (Cells 14-18)**

| Analysis | Method | Output |
|----------|--------|--------|
| Feature Importance | sklearn's feature_importance_ | feature_importance.csv + PNG chart |
| Pricing Segments | Percentile-based (actual vs predicted) | Identify underpriced, optimal, overpriced |
| Revenue Opportunity | (Predicted − Actual) × 365 days | pricing_recommendations.csv |
| Neighborhood Insights | Group by neighborhood, aggregate metrics | neighborhood_insights.csv |
| Error Analysis | Residuals = Actual − Predicted | error_analysis.png |

**Key Finding: Feature Importance**
```
bathrooms:          29.1%  ← Property fundamentals dominate
longitude:          17.2%
latitude:           10.3%
minimum_nights:     12.9%
accommodates:        8.5%
bedrooms:            7.4%
[Rest < 5% each]

Insight: Demand signals (reviews) < 1% importance
→ Property location & capacity drive prices more than booking history
```

**Revenue Opportunity Calculation:**
```python
for each property in test set:
  recommended_price = model.predict(features)
  current_price = actual_price
  daily_delta = recommended_price − current_price
  annual_delta = daily_delta × 365
  
portfolio_opportunity = sum(annual_delta for all properties)
# Result: $522,273 potential annual gain
```

**Pricing Segments:**
```
Underpriced (173 properties, 44.6%):  
  Current avg: $168/night → Recommended: $243/night
  Opportunity: +$75/night → +$27k/property/year

Optimal (111 properties, 28.1%):
  Current ≈ Recommended → No change needed

Overpriced (104 properties, 27.3%):
  Current avg: $298/night → Recommended: $169/night
  Adjustment: -$129/night → Lose $47k/property/year if not corrected
```

**Output:** 3 CSVs + 8 visualizations

---

#### **Section F: Visualization & Export (Cells 19-20)**

| Visualization | Shows | Used In Presentation? |
|---|---|---|
| `neighborhood_overview.png` | Top 15 neighborhoods by avg price, reviews | Slide 4 (EDA) |
| `price_distribution.png` | Histogram + stats (mean, median, std) | Slide 4 (Price Landscape) |
| `price_by_neighborhood_box.png` | Box plots showing price variance | Slide 4 (Variation) |
| `correlation_matrix.png` | Heatmap of feature relationships | Presentation context |
| `scatter_plots.png` | Price vs key features (capacity, amenities) | Slide 7 (Feature Analysis) |
| `model_comparison.png` | RMSE bars for 5 models | Slide 6 (Model Performance) |
| `actual_vs_predicted.png` | Scatter plot of prediction accuracy | Slide 8 (Accuracy) |
| `feature_importance.png` | Bar chart: bathrooms 29%, long 17%, etc. | Slide 7 (Feature Importance) |
| `business_impact.png` | Revenue opportunity by segment | Slide 9 (Opportunities) |
| `neighborhood_insights.png` | Analysis per neighborhood | Supplementary |
| `error_analysis.png` | Residuals and outlier diagnostics | Supplementary |

**Export Files:**
- `model_results.csv`: 5 rows (models) × 4 columns (RMSE, MAE, R², MAPE)
- `feature_importance.csv`: 15 rows (features) × 2 columns (name, importance %)
- `key_statistics.csv/txt`: Portfolio metrics (mean price, total opportunity, segments)
- `neighborhood_insights.csv`: 8 neighborhoods × metrics (avg price, count, opportunity)
- `pricing_recommendations.csv`: 388 rows (test properties) × columns (id, current_price, recommended_price, delta, annual_opportunity)
- `cleaned_dataset.csv`: Full 1,937 rows × 15 features (for reproducibility)

---

### 6. **outputs/results/** (Model Outputs)
**Files:**

#### **model_results.csv**
```
Model,RMSE,MAE,R2,MAPE
Linear Regression,121.45,78.92,0.5012,38.2%
Ridge,120.89,78.45,0.5089,37.9%
Lasso,119.23,77.89,0.5201,37.1%
Random Forest,102.38,65.87,0.6401,31.4%
Gradient Boosting,100.14,64.20,0.6514,29.68%
```
**Why it matters:** Objective comparison of 5 algorithms; GB clearly superior

---

#### **feature_importance.csv**
```
Feature,Importance_%
bathrooms,29.1
longitude,17.2
latitude,10.3
minimum_nights,12.9
accommodates,8.5
...
```
**Why it matters:** Explains model decisions; guides business strategy

---

#### **key_statistics.txt**
```
NYC Airbnb Pricing Analysis - Key Statistics

Dataset Summary:
- Total listings analyzed: 1,937
- Price range: $50–$1,000
- Mean nightly price: $227.34
- Median nightly price: $180.00
- Standard deviation: $156.78

Model Performance (Gradient Boosting):
- RMSE: $100.14 (average prediction error)
- MAE: $64.20 (median prediction error)
- R²: 0.6514 (explains 65% of price variation)
- MAPE: 29.68% (percentage error)

Pricing Opportunity Analysis:
- Total annual revenue opportunity: $522,273
- Percentage of portfolio opportunity: 2.27%
- Underpriced listings: 173 (44.6%) - avg +$74.76/night
- Optimal listings: 111 (28.1%)
- Overpriced listings: 104 (27.3%) - avg -$129.06/night

Top 3 Price Drivers:
1. Bathrooms (29% importance)
2. Longitude (17% importance)
3. Minimum nights (13% importance)
```
**Why it matters:** Business-ready summary for stakeholders

---

#### **pricing_recommendations.csv**
```
Property_ID,Current_Price,Recommended_Price,Daily_Delta,Annual_Opportunity,Segment
12345,220,416,196,71540,Underpriced
12346,400,325,-75,-27375,Overpriced
12347,180,182,2,730,Optimal
...
```
**Why it matters:** Per-property actionable recommendations for hosts

---

#### **neighborhood_insights.csv**
```
Neighborhood,Count,Avg_Price,Avg_Bathrooms,Avg_Accommodates,Avg_Reviews
Williamsburg,156,$298,1.8,3.2,45
Bushwick,124,$225,1.5,2.9,38
...
```
**Why it matters:** Neighborhood-level analysis for market segmentation

---

#### **cleaned_dataset.csv**
```
id,price,bathrooms,bedrooms,latitude,longitude,neighbourhood,room_type,...
[1,937 rows × 15 features]
```
**Why it matters:** Reproducibility; contains exact training data used

---

### 7. **outputs/visualizations/** (Charts)
**11 PNG files** generated from plotting code; embedded in presentations

---

### 8. **presentations/PRESENTATION_GUIDE_AND_SCRIPT.md**
**Purpose:** 12-slide presentation guide with full speaker scripts for 3 presenters  
**Length:** 14KB markdown  
**Contains:**
- Slide content outlines
- Speaker scripts (what each person says)
- Timing guidance (Person A: 2:30, B: 3:45, C: 4:00)
- Visual asset references (which PNG goes on which slide)
- Transition phrases

---

### 9. **presentations/PRESENTER_QUICK_REFERENCE.md**
**Purpose:** 1-page cheat sheet for presenters  
**Contains:**
- Timing table
- Key numbers to memorize
- Smooth transition phrases
- Elevator pitches (10-sec, 30-sec, 1-min)
- Q&A prep (6 likely questions + answers)
- Delivery tips
- Slide-by-slide one-liners

---

## Technology Stack

### Languages & Environments
- **Python 3.13.3** – Programming language
- **Jupyter Notebook (.ipynb)** – Interactive code environment
- **Virtual environment (.venv)** – Isolated package management

### Data & Analysis Libraries
| Library | Purpose | Usage |
|---------|---------|-------|
| `pandas` | Data manipulation, cleaning, aggregation | Load CSV, filter, engineer features, export results |
| `numpy` | Numerical computing | Array operations, mathematical functions |
| `matplotlib` | Static plotting | Create PNG charts (line, scatter, bar) |
| `seaborn` | Statistical visualization | Heatmaps, box plots, correlation matrices |
| `scipy` | Scientific computing | Statistical tests, optimization |

### Machine Learning Libraries
| Library | Purpose | Usage |
|---------|---------|-------|
| `scikit-learn` | ML toolkit | Train 5 models (Linear, Ridge, Lasso, RF, GB), scale features, evaluate metrics |
| `joblib` | Model serialization | Save/load trained models (though not used in final version) |

### File Formats
| Format | Use Case | Files |
|--------|----------|-------|
| `.csv` | Tabular data (open, Excel-compatible) | Input (listings.csv) + Outputs (results CSVs) |
| `.ipynb` | Interactive code notebooks | 2 main analysis notebooks |
| `.png` | Visualization charts (high-quality, embeddable) | 11 output charts |
| `.txt` | Human-readable summaries | key_statistics.txt |
| `.geojson` | Geospatial boundaries | Neighborhood mapping (not used in final model) |
| `.md` | Documentation (Markdown) | Presentations, this guide |

---

## Workflow Explanation

### End-to-End Process

#### **Phase 1: Problem Definition**
1. **Identify challenge:** Airbnb hosts don't know optimal pricing
2. **Define metric:** Can we predict prices with <$120 RMSE?
3. **Data requirement:** Need 1,000+ properties with pricing + features

#### **Phase 2: Data Acquisition**
1. Download Inside Airbnb CSVs (listings, reviews, neighborhoods)
2. Store in `data/` folder
3. Inspect raw data (36,111 listings, ~30 features)

#### **Phase 3: Exploratory Data Analysis (01_data_exploration.ipynb)**
1. Load data and inspect shapes/types
2. Calculate descriptive statistics (mean $227, median $180)
3. Visualize distributions (right-skewed; outliers at $700+)
4. Check for missing values (bedrooms: 2 missing, bathrooms: 3 missing)
5. Explore feature correlations (bedrooms-price: r=0.65)
6. Identify neighborhoods (8 major areas; Williamsburg is priciest)

**Output:** Understanding of data; 3 visualizations; no CSV exports

#### **Phase 4: Data Cleaning**
1. Apply filters (neighborhood, price range $50–$1k)
2. Remove outliers and missing values
3. Result: 36,111 → 1,937 high-quality records

**Why filter aggressively?**
- Model quality > dataset size (better to have 1,937 clean rows than 36,111 dirty)
- Outliers (billionaire penthouses) distort learning
- Focus on mainstream market where pricing model is applicable

#### **Phase 5: Feature Engineering**
1. Keep basic numeric features (beds, baths, capacity)
2. Create derived features (occupancy_proxy, review_intensity)
3. Encode categoricals (room_type, neighborhood, superhost)
4. Select 15 final features (balance expressiveness with generalization)

**Why engineer new features?**
- Raw data rarely tells the full story
- `review_intensity` = activity level (proxy for demand)
- `amenities_count` > just having the list
- Interaction terms (lat × long) capture micro-location effects

#### **Phase 6: Train-Test Split & Scaling**
1. Shuffle 1,937 records randomly
2. Split: 1,549 train (80%) + 388 test (20%)
3. Fit scaler on training set only (prevent data leakage)
4. Apply to both train & test

**Why 80/20?**
- 1,549 enough to capture patterns
- 388 large enough to validate (not overfitting)
- Standard practice in ML

**Why scale?**
- Features have different ranges (latitude: 40–41, reviews: 0–500)
- Algorithms treat large values as "more important"
- Scaling ensures fair contribution

#### **Phase 7: Model Training**
1. Define 5 candidate algorithms
2. Train each on 1,549 training samples
3. Predict on 388 held-out test samples
4. Calculate RMSE, MAE, R², MAPE

**Algorithm choices:**
- **Linear:** Establish baseline (simple, interpretable)
- **Ridge/Lasso:** Combat overfitting (same basis as linear but regularized)
- **Random Forest:** Capture non-linearity (many trees, parallel)
- **Gradient Boosting:** Best non-linear learner (iterative refinement)

#### **Phase 8: Model Evaluation & Selection**
1. Compare metrics across 5 models
2. Gradient Boosting wins (RMSE $100.14, R² 0.6514)
3. Export results to CSV

**Why GB?**
- Lowest RMSE ($100.14 vs $102 for RF)
- Highest R² (0.6514)
- Captures complex interactions (e.g., bathrooms + luxury amenities)
- Interpretable feature importance

#### **Phase 9: Business Analysis**
1. Extract feature importance (bathrooms 29%, location 17%)
2. Make predictions on test set
3. Compare predicted vs actual price
4. Identify segments (underpriced, optimal, overpriced)
5. Calculate per-property revenue opportunity
6. Aggregate to portfolio level ($522k total)

#### **Phase 10: Visualization & Reporting**
1. Generate 11 PNG charts
2. Export 6 CSV results files
3. Create human-readable summaries (txt, md)

#### **Phase 11: Presentation Preparation**
1. Draft 12-slide outline
2. Write speaker scripts (3 presenters, 8–10 min)
3. Map visuals to slides
4. Create timing guide and quick reference
5. Prepare Q&A talking points

---

## Key Design Decisions

### Decision 1: Why 15 Features (Not 89)?
**Considered:** Using all engineered features  
**Chosen:** Select 15 top features  
**Rationale:**
- More features = more noise
- 89 features on 1,937 rows = ~22 rows per feature (sparse)
- 15 features on 1,937 rows = ~129 rows per feature (stable)
- Interpretability matters for business (hard to explain 89 features)
- Fewer features = faster training, lower variance

### Decision 2: Why Gradient Boosting (Not Random Forest)?
**Considered:** Both tree-based; both non-linear  
**Chosen:** Gradient Boosting  
**Rationale:**
- RMSE $100 vs $102 (small but consistent improvement)
- Better captures pricing nuance (diminishing returns on amenities)
- Interpretable feature importance
- Slightly more robust to outliers
- Iterative learning prevents plateauing

### Decision 3: Why 80/20 Train-Test Split?
**Considered:** 70/30, 90/10  
**Chosen:** 80/20  
**Rationale:**
- 1,549 training samples sufficient to learn patterns
- 388 test samples sufficient to validate (reduces statistical noise)
- Standard practice; widely trusted

### Decision 4: Why Filter to $50–$1k (Not Include All)?
**Considered:** Keep all 36,111 listings  
**Chosen:** Filter to $50–$1k, resulting in 1,937  
**Rationale:**
- Sub-$50 listings are errors or special cases (group stays, parking)
- Over-$1k listings are luxury outliers with unique drivers (brand, celebrity, art, history)
- Model generalizes better on mainstream market
- 1,937 listings is still large enough (typical kaggle datasets: 1k–10k)
- Business use case: most hosts are mainstream, not ultra-luxury

### Decision 5: Why StandardScaler (Not MinMaxScaler)?
**Considered:** MinMaxScaler (0–1 range), RobustScaler (handles outliers)  
**Chosen:** StandardScaler (mean=0, std=1)  
**Rationale:**
- StandardScaler is most common; works well with Ridge/Lasso/Gradient Boosting
- MinMaxScaler can compress information if outliers present
- RobustScaler overkill (we already filtered outliers)
- No performance difference in final model (GB is robust to scaling)

---

## Summary

This project demonstrates a **complete data science workflow:**
1. **Understand** the problem (host pricing challenge)
2. **Collect** and explore data (36k+ listings)
3. **Prepare** data (filter, clean, engineer features → 1,937 rows, 15 features)
4. **Model** (5 algorithms; GB wins with $100 RMSE)
5. **Evaluate** (R² 0.65; captures 65% of variation)
6. **Interpret** (bathrooms 29%, location 17%)
7. **Recommend** ($522k opportunity; per-property guidance)
8. **Communicate** (12-slide presentation, speaker scripts)

Each file, folder, and decision serves a specific purpose in this pipeline. Understanding the **why** behind each step is key to appreciating the project's rigor and reproducibility.

---

**Next:** See Part 2 for technical deep-dives, advanced Q&A, model assumptions, limitations, and future improvements.

