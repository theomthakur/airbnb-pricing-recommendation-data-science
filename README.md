# Airbnb NYC Pricing Optimization Project

**Course:** Data Science and AI for Business  
**Professor:** Dr. Chris Volinsky  
**Team:** Princy Doshi, Ashutosh Agrawal, Om Thakur  
**Date:** Fall 2025

---

## 📋 Project Overview

This project develops a machine learning-based pricing recommendation system for Airbnb property managers in New York City. Using data from Inside Airbnb, we predict optimal nightly rates to maximize revenue while maintaining healthy occupancy rates.

**Business Problem:** Property managers need to dynamically price their listings to balance occupancy and profitability in a competitive short-term rental market.

**Solution:** Gradient Boosting regression model trained on 1,937 NYC entire home listings across eight diverse neighborhoods (Williamsburg, Upper East Side, Astoria, Bedford-Stuyvesant, Hell's Kitchen, Harlem, Bushwick, Crown Heights), achieving RMSE of $100.14 and R² of 0.6514.

---

## 📁 Project Structure

```
airbnb-pricing-project/
│
├── data/
│   ├── listings.csv.gz           # Raw data (download from Inside Airbnb)
│   └── cleaned_dataset.csv        # Processed data (generated)
│
├── notebooks/
│   ├── 01_data_exploration.ipynb  # Initial data exploration
│   └── 02_master_analysis.ipynb   # Complete analysis pipeline
│
├── outputs/
│   ├── visualizations/
│   │   ├── neighborhood_overview.png
│   │   ├── price_distribution.png
│   │   ├── model_comparison.png
│   │   ├── actual_vs_predicted.png
│   │   ├── feature_importance.png
│   │   └── business_impact.png
│   │
│   ├── results/
│   │   ├── model_results.csv
│   │   ├── feature_importance.csv
│   │   ├── key_statistics.csv
│   │   ├── neighborhood_insights.csv
│   │   └── pricing_recommendations.csv
│   │
│   └── deliverables/
│       ├── Final_Report.pdf
│       └── Presentation_Slides.pptx
│
└── README.md (this file)
```

---

## 🚀 Quick Start Guide

### Step 1: Download Data
1. Go to: http://insideairbnb.com/get-the-data/
2. Find "New York City, New York, United States"
3. Download **listings.csv.gz**
4. Place in `data/` folder

### Step 2: Run Initial Exploration
```python
# Open 01_data_exploration.ipynb
# Run all cells to:
# - Load data
# - Explore neighborhoods
# - Decide on scope
```

### Step 3: Run Complete Analysis
```python
# Open 02_master_analysis.ipynb
# Update TARGET_NEIGHBORHOODS with your chosen areas
# Run all cells (takes 5-10 minutes)
# All outputs will be generated automatically
```

### Step 4: Use Generated Files
- Copy visualizations to presentation
- Use `key_statistics.txt` to fill report template
- Review `pricing_recommendations.csv` for business insights

---

## 📊 Key Results

### Model Performance
- **RMSE:** $109.28 - Average prediction error
- **R²:** 0.5515 - Explains 55.15% of price variation  
- **MAE:** $68.06 - Mean absolute error
- **MAPE:** 27.00% - Mean absolute percentage error

**Context:** On average price of $232.96, RMSE of $109 represents 47% error rate, which is reasonable for real estate pricing with heterogeneous property types.

### Top Price Drivers (Feature Importance)
1. **Accommodates** - 25.63% (guest capacity is primary driver)
2. **Bathrooms** - 21.20% (bathroom count strong signal of property quality)
3. **Longitude** - 12.72% (east-west location within neighborhoods)
4. **Latitude** - 8.44% (north-south location)
5. **Amenities Count** - 6.38% (more amenities justify higher prices)
6. **Minimum Nights** - 6.07%
7. **Number of Reviews** - 3.26%

**Key Insight:** Property capacity (accommodates + bathrooms) drives 47% of pricing power. Location (lat/long) accounts for 21%. Demand indicators (reviews, availability) have minimal impact (<5% combined), suggesting intrinsic property features matter more than booking history.

### Correlation Analysis
**Strongest correlations with price:**
- Accommodates: r = 0.559 ⭐
- Bathrooms: r = 0.552 ⭐
- Bedrooms: r = 0.523 ⭐
- Beds: r = 0.410

**Weak correlations:**
- Availability_365: r = 0.070
- Number of reviews: r = 0.060
- Occupancy proxy: r = -0.070

**Interpretation:** Capacity metrics show strong linear relationships with price, while demand/supply indicators (reviews, availability) show almost no correlation, suggesting price is driven by property characteristics rather than market dynamics in our filtered dataset.

### Business Impact Analysis

**Property Segmentation:**
- **Underpriced:** 54 properties (41.9%) - Average opportunity: +$74.67/night
- **Optimally Priced:** 41 properties (31.8%)
- **Overpriced:** 34 properties (26.4%) - Average adjustment needed: -$129.06/night

**Test Set Revenue Analysis:**
- Current average price: $245.35/night
- Model-recommended average: $242.71/night  
- Net adjustment: -$2.64/night (-1.08%)

**Interpretation:** Test set shows slight overpricing on average, but model identifies significant individual opportunities in both directions. The negative net change demonstrates model objectivity - it recommends price *decreases* when appropriate, not just increases.

**Individual Property Examples:**

**Top Underpriced Opportunity:**
- 3BR/2BA Williamsburg Townhouse
- Current: $175, Recommended: $535 (+$360, +206%)
- Model suggests massive underpricing relative to capacity/location

**Top Overpriced Property:**
- Modern 4700 SF Williamsburg house
- Current: $1000, Recommended: $516 (-$484, -48%)
- Priced above model prediction despite large size

### Model Limitations & Error Analysis

**Residual Statistics:**
- Mean residual: $2.64 (close to 0 - unbiased)
- Std dev: $109.25
- Min error: -$360 (underestimated)
- Max error: +$484 (overestimated)

**Largest Prediction Errors:**
1. Modern 4700 SF house (actual $1000, predicted $516) - unique luxury property
2. Park-side townhouse with parking (actual $800, predicted $389) - parking premium not captured
3. 3BR/2BA Williamsburg townhouse (actual $175, predicted $535) - severely underpriced

**Error Patterns:**
- Model struggles with extreme luxury properties (>$700/night)
- Unique amenities (parking, private outdoor space) not fully captured
- Some properties genuinely mispriced in market

**What's Missing from Model:**
- View quality (park, skyline, waterfront)
- Recent renovations/updates
- Exact street location (within neighborhood)
- Proximity to subway/transportation
- Building type (townhouse vs apartment)
- Outdoor space (terrace, garden, parking)

---

## 🎉 Acknowledgments

- **Professor Chris Volinsky** for project guidance and feedback
- **Inside Airbnb** for providing open data
- **Teaching assistants** for technical support
- **Team members** for collaboration and hard work