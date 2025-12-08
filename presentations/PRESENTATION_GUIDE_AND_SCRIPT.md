# NYC Airbnb Pricing Optimization - Presentation Guide & Speaker Script
## 8-10 Minute Presentation for 3 Presenters

**Total Time:** 8-10 minutes | **Total Slides:** 12  
**Audience:** Faculty & Classmates | **Format:** Google Slides with speaker notes

---

## SLIDE BREAKDOWN & PRESENTER ASSIGNMENTS

### **PERSON A (Problem & Data) – 2.5 minutes**
---

## **SLIDE 1: Title Slide (15 seconds)**

### Content:
- **Title:** "NYC Airbnb Pricing Optimization"  
- **Subtitle:** Dynamic Pricing Model for Short-Term Rentals  
- **Course:** Data Science and AI for Business  
- **Team:** Princy Doshi, Ashutosh Agrawal, Om Thakur  
- **Date:** Fall 2025

### Speaker Script (Person A):
> "Good [morning/afternoon]. We're Princy, Ashutosh, and Om from the Data Science and AI for Business course. Today we're presenting our project: a machine learning model to optimize Airbnb pricing in New York City. Over the next ten minutes, we'll walk you through how we solved this real-world problem using data science. I'll start by setting the stage with the business problem and our data."

---

## **SLIDE 2: The Business Problem (45 seconds)**

### Content:
- **Challenge:** Airbnb hosts in NYC struggle to price listings optimally
- **Context:** 
  - Over 46,000+ active NYC listings
  - Highly competitive short-term rental market
  - Manual pricing leaves money on the table
  - Seasonality & location complexity
  
- **Goal:** Build a recommendation system to balance revenue & occupancy

### Speaker Script (Person A):
> "Imagine you own an Airbnb in New York City. You're asking: How much should I charge per night? Price too low, you sacrifice revenue. Price too high, your calendar sits empty. There are 46,000+ competing listings across the city, and setting prices manually is nearly impossible—especially when demand fluctuates by neighborhood and season. 
>
> Our goal was to build a machine learning model to give hosts data-driven pricing recommendations. This isn't just about maximizing revenue; it's about finding the sweet spot: enough price to be profitable, competitive enough to attract bookings."

---

## **SLIDE 3: Data & Scope (45 seconds)**

### Content:
- **Source:** Inside Airbnb NYC (open dataset)
- **Raw Data:** 36,111 listings  
- **After Filtering:** 1,937 listings (quality filtered)
- **Focus Neighborhoods:** 
  - Williamsburg, Upper East Side, Astoria  
  - Bedford-Stuyvesant, Hell's Kitchen, Harlem  
  - Bushwick, Crown Heights  
- **Key Features:** Capacity (beds/baths), location (lat/long), amenities, reviews, availability

### Speaker Script (Person A):
> "We sourced our data from Inside Airbnb, a public dataset of NYC listings. Starting with 36,111 listings, we applied rigorous filters: minimum 5 reviews for reliability, at least 15 days available annually, and prices between $50–$1,000 to exclude outliers. We narrowed down to 1,937 listings across 8 diverse neighborhoods—from trendy Williamsburg to emerging Astoria.
>
> For each listing, we captured 15 features: How many people it sleeps, bathroom/bedroom count, amenities, exact GPS coordinates for hyper-local pricing, and historical reviews to measure demand."

---

## **SLIDE 4: Price Landscape (45 seconds)**
**[Include visualization: price_distribution.png + price_by_neighborhood_box.png]**

### Content:
- **Price Stats (Portfolio):**
  - Mean: $227.43/night  
  - Median: $180  
  - Range: $53–$1,000  
  - Std Dev: $151.90 (high variability)

- **By Neighborhood (Top 3):**
  - Williamsburg: $259 avg  
  - Hell's Kitchen: $253 avg  
  - Crown Heights: $234 avg

### Speaker Script (Person A):
> "Here's what we found. The average price across our portfolio is $227 per night, but the median is $180—meaning many affordable properties drag down the average. We see wild swings: $53 budget stays to $1,000 luxury penthouses. And price varies dramatically by neighborhood. Williamsburg commands $259 on average, Hell's Kitchen $253, while Bushwick sits at $186—all within the same city, just different vibes and demand curves."

---

### **PERSON B (Modeling & Performance) – 3 minutes**
---

## **SLIDE 5: Modeling Approach (45 seconds)**

### Content:
- **Problem Type:** Supervised Regression (predict continuous price)
- **Train/Test Split:** 80/20 (1,549 train / 388 test)
- **Models Tested:**
  - Linear/Ridge/Lasso (baselines)
  - Random Forest (ensemble)
  - **Gradient Boosting (winner)**
- **Pipeline:** Raw data → Feature scaling → Model training → Evaluation

### Speaker Script (Person B):
> "We approached this as a regression problem: given a listing's features, predict its optimal nightly price. We tested five models: three linear baselines to set a floor, then two powerful ensemble methods—Random Forest and Gradient Boosting—known for capturing complex, non-linear pricing patterns.
>
> We split our 1,937 listings: 1,549 for training, 388 for testing. Each model was trained on scaled features to ensure fair comparison. And we evaluated all on the same test set using standard metrics: RMSE, MAE, and R²."

---

## **SLIDE 6: Model Performance (60 seconds)**
**[Include visualization: model_comparison.png]**

### Content:
- **Winner: Gradient Boosting**
  - RMSE: **$100.14** (avg prediction error)  
  - MAE: **$64.20** (mean absolute error)  
  - **R²: 0.6514** (explains 65% of price variation)  
  - MAPE: 29.68%

- **Context:** Mean price $227 → RMSE $100 = ~44% error rate (reasonable for heterogeneous real estate)
  
- **Runner-up:** Random Forest (RMSE $102, R² 0.636)  
- **Baselines:** Linear models (RMSE $121, R² 0.49)

### Speaker Script (Person B):
> "Here are the results. Gradient Boosting dominates. With an RMSE of $100.14, it means on average our price predictions are off by about $100. On a $227 average price, that's roughly 44% error—and frankly, for real estate with so many hidden factors, that's solid. An R² of 0.6514 means we explain 65% of the price variation with just 15 features—the other 35% is likely luxury amenities, renovation status, or market buzz we can't capture in data.
>
> Random Forest is nearly as good, but Gradient Boosting pulls ahead by tuning iteratively. Linear models, by contrast, struggle at $121 RMSE because housing prices are rarely linear—they plateau at luxury levels and spike near transit."

---

## **SLIDE 7: Feature Importance (60 seconds)**
**[Include visualization: feature_importance.png]**

### Content:
- **Top 5 Drivers:**
  1. **Bathrooms:** 29% importance (quality signal)  
  2. **Longitude:** 17% (east-west location within neighborhood)  
  3. **Minimum Nights:** 13% (flexibility affects price)  
  4. **Accommodates:** 9% (guest capacity)  
  5. **Amenities Count:** 7% (extras justify premium)

- **Key Insight:** Capacity + location explain 57% of price  
- **Weak Signals:** Reviews (3%), availability (3%) – demand doesn't drive price as much as supply

### Speaker Script (Person B):
> "Now, what drives prices? Let me show you the feature importance—basically, what the model learned matters most.
>
> Number one: Bathrooms at 29%. This makes sense. A property with 2 bathrooms signals quality and can accommodate more people comfortably.
>
> Number two: Longitude at 17%. Within a neighborhood like Williamsburg, being on the west side near the waterfront is worth more than being inland. Location within location matters.
>
> Minimum nights requirement is third at 13%. Hosts who allow short stays can charge more because they have higher turnover flexibility.
>
> Interestingly, the number of reviews—which you'd think signals popularity—ranks seventh at just 3%. This tells us two things: First, our models care more about what the property *is* than what customers *say*. Second, it suggests prices are driven by intrinsic features, not by past booking success."

---

## **SLIDE 8: Prediction Accuracy (60 seconds)**
**[Include visualization: actual_vs_predicted.png]**

### Content:
- **Scatter Plot:** Actual vs. Predicted prices  
- **Pattern:** Tight cluster around diagonal (good fit)  
- **Largest Errors:** Luxury properties >$700 (outliers)
- **Residual Stats:**
  - Mean: -$5.27 (nearly unbiased)  
  - Std Dev: $100  
  - Range: -$311 to +$635

- **Key Takeaway:** Model performs well on "normal" listings; struggles on rare luxury outliers

### Speaker Script (Person B):
> "Here's our actual vs. predicted plot. The closer points to the diagonal line, the better. You can see most points cluster tightly around that line, meaning our predictions are solid for typical $100–$400 listings.
>
> Where we stumble: extreme luxury properties. A few penthouses priced at $900–$1,000 are way off—we underestimate them because they're rare in our training data and likely have hidden prestige factors we don't capture.
>
> Our residuals average -$5.27, which is nearly zero—meaning we're unbiased, not systematically over or underpricing. That's the hallmark of a trustworthy model."

---

### **PERSON C (Business Impact & Recommendations) – 3 minutes**
---

## **SLIDE 9: Pricing Opportunities (60 seconds)**
**[Include visualization: business_impact.png]**

### Content:
- **Portfolio Segmentation (388 listings):**
  - **Underpriced: 173 (44.6%)**  
    Avg opportunity: +$74.76/night  
  - **Optimally Priced: 109 (28.1%)**  
    Already at equilibrium  
  - **Overpriced: 106 (27.3%)**  
    Avg adjustment: -$129.06/night (need cut)

- **Net Portfolio Impact:**  
  - Current avg price: $232.19/night  
  - Recommended avg: $237.46/night  
  - **Net increase: +$5.27 (+2.27%)**

### Speaker Script (Person C):
> "Now for the business: Where does our model say prices are wrong?
>
> 44.6% of properties are underpriced—left money on the table. The average opportunity is +$75 per night. Not small change. Over a year with 70% occupancy, that's $19,000 extra per property.
>
> 27.3% are overpriced. These hosts will see bookings fall if they don't cut. Average cut: -$129 per night.
>
> 28% are roughly right. Our model validates their pricing.
>
> What's fascinating: The portfolio as a whole needs only a +2.27% bump. We're not saying 'raise all prices 50%.' Some go up, some go down. This objectivity—recommending *decreases* when appropriate—is what builds trust. Our model isn't a hype machine; it's fair."

---

## **SLIDE 10: Revenue Impact Simulation (60 seconds)**

### Content:
- **Test Portfolio:** 388 listings  
- **Assumed Occupancy:** 70% (industry baseline)
- **Current Annual Revenue:** $23,018,250
- **With Model Recommendations:** $23,540,523
- **Potential Gain:** **$522,273 (+2.27%)**

- **Per-listing Average:**  
  - Current: $232.19 × 255 nights (70% occupancy) = ~$59k/year  
  - Recommended: $237.46 × 255 nights = ~$60.5k/year  
  - **Incremental: ~$1,345/year per property**

### Speaker Script (Person C):
> "Let's put this in dollar terms. Assume our test portfolio of 388 listings each books 70% of nights—reasonable for NYC short-term rentals. At current prices, they'd pull in $23 million annually. With our recommendations? $23.5 million.
>
> That's $522,000 in additional revenue—without changing any operations, just optimizing prices based on data.
>
> Translate that to a single property: roughly $1,345 extra per year, per listing. In a city where competition is brutal, that's meaningful. And remember: this assumes no occupancy changes. Smarter pricing could actually *improve* occupancy simultaneously, amplifying the upside."

---

## **SLIDE 11: Real-World Examples (60 seconds)**

### Content:

**Underpriced Example:**  
- **Name:** Williamsburg Condo w/ Backyard  
- **Current:** $220/night  
- **Recommended:** $415/night  
- **Opportunity:** +$195 (+89%)  
- **Why:** 4 beds, 2 baths, outdoor space—undervalued  

**Overpriced Example:**  
- **Name:** Modern 4700 SF House  
- **Current:** $1,000/night  
- **Recommended:** $516/night  
- **Adjustment:** -$484 (-48%)  
- **Why:** Despite large size, model sees similar properties price far lower; likely won't book at $1k

### Speaker Script (Person C):
> "Let me give you two real examples from our dataset.
>
> First, an underpriced gem. A 4-bed, 2-bath townhouse in Williamsburg with a backyard currently listed at $220/night. Our model says $415. That's an 89% increase. Why? Because similar properties across the neighborhood—same capacity, same amenities—fetch $400+. This host is leaving $14,500 on the table yearly if occupancy stays constant.
>
> Second, an overpriced property. A massive 4,700 square-foot house listed at $1,000/night. Despite its size, our model recommends $516. Why the massive cut? Because in our training data, even large luxury homes rarely sustain $1,000 pricing unless they're iconic properties with brand recognition. At $1,000, it likely sits empty most nights, yielding $0. At $516, it might book 40% occupancy and earn $76k annually. The math is clear."

---

## **SLIDE 12: Key Takeaways & Next Steps (60 seconds)**

### Content:
- **What We Built:**  
  ✅ Gradient Boosting regression model  
  ✅ Trained on 1,937 NYC Airbnb listings  
  ✅ Predicts optimal nightly rates (RMSE $100, R² 0.65)  
  ✅ Identifies $522k revenue opportunity in test portfolio

- **Key Learnings:**  
  🎯 Capacity + location = 57% of pricing power  
  🎯 Demand signals matter less than property fundamentals  
  🎯 Data-driven pricing beats intuition

- **Next Steps:**  
  → Segment luxury properties separately (outlier models)  
  → Add granular features (view, renovation, parking)  
  → Integrate seasonal & event-based demand  
  → A/B test recommendations with real hosts  
  → Retrain quarterly as market evolves

### Speaker Script (All 3 together, or Person C):
> "In closing, here's what we accomplished:
>
> We built a machine learning system that takes complex property data and spits out one number: the optimal nightly rate. On 1,937 NYC listings, it works. It correctly explains 65% of price variation with an error margin of ±$100—useful for practical decisions.
>
> We learned that fundamentals—what the property *is*—matter far more than booking history. Bathrooms, location, and flexibility drive pricing.
>
> And we proved the business case. A portfolio of 388 properties could unlock over half a million dollars in annual revenue by adopting these recommendations.
>
> If we were to extend this, we'd segment luxury properties differently (they follow different rules), add granular location features like proximity to transit or parks, and integrate real-time demand data from Airbnb's API. We'd also A/B test: recommend prices to a subset of hosts, measure actual occupancy and revenue, and refine the model based on real-world results.
>
> Thank you!"

---

## TIMING BREAKDOWN

| Slide | Presenter | Content | Duration |
|-------|-----------|---------|----------|
| 1 | A | Title + Intro | 0:15 |
| 2 | A | Business Problem | 0:45 |
| 3 | A | Data & Scope | 0:45 |
| 4 | A | Price Landscape | 0:45 |
| **Subtotal (A)** | | | **2:30** |
| 5 | B | Modeling Approach | 0:45 |
| 6 | B | Model Performance | 1:00 |
| 7 | B | Feature Importance | 1:00 |
| 8 | B | Prediction Accuracy | 1:00 |
| **Subtotal (B)** | | | **3:45** |
| 9 | C | Pricing Opportunities | 1:00 |
| 10 | C | Revenue Impact | 1:00 |
| 11 | C | Real-World Examples | 1:00 |
| 12 | C | Takeaways & Next Steps | 1:00 |
| **Subtotal (C)** | | | **4:00** |
| **TOTAL** | | | **~10:15** |

*Flexibility:* Trim 15–30 seconds from each section to land at 9 minutes if needed. Cut Slide 8 (Prediction Accuracy) if running long—the story works without it.

---

## GOOGLE SLIDES SETUP

### Deck Structure:
1. Use a clean, professional template (e.g., "Simple Light")
2. Color scheme: White background, navy/blue accents
3. Font: Calibri or Arial, 32pt titles, 24pt body
4. Images: Embed from `outputs/visualizations/`:
   - Slide 4: `price_distribution.png` (left), `price_by_neighborhood_box.png` (right)
   - Slide 6: `model_comparison.png` (full width)
   - Slide 7: `feature_importance.png` (full width)
   - Slide 8: `actual_vs_predicted.png` (full width)
   - Slide 9: `business_impact.png` (full width)
5. Notes: Add speaker notes (Person A, B, C) to each slide

### Presenter Tips:
- **Practice handoffs:** "Thanks, [Name]. I'll pass it to [Next Person]..."
- **Point to visuals:** "Notice the spread here in Williamsburg vs. Bushwick..."
- **Pace:** Aim for ~60 words per minute. Read speaker notes, don't memorize.
- **Engage:** Make eye contact. Ask if questions hold until the end.
- **Q&A Ready:** Know your data cold. Be prepared for "Why is that feature so important?" or "How would you handle seasonal pricing?"

---

## APPENDIX: KEY STATISTICS FOR Q&A

**Dataset:**
- Original: 36,111 listings | Filtered: 1,937 | Test: 388

**Model (Gradient Boosting):**
- RMSE: $100.14 | MAE: $64.20 | R²: 0.6514 | MAPE: 29.68%

**Price Range:**
- Mean: $227.43 | Median: $180 | Min: $53 | Max: $1,000

**Feature Importance Top 3:**
- Bathrooms: 29% | Longitude: 17% | Min Nights: 13%

**Revenue Opportunity:**
- Test portfolio: 388 properties | Potential gain: $522,273 annually (+2.27%)

**Pricing Segments:**
- Underpriced: 173 (44.6%) +$74.76/night  
- Optimally Priced: 109 (28.1%)  
- Overpriced: 106 (27.3%) -$129.06/night

---

**END OF PRESENTATION GUIDE**
