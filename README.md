
# HousingMarket

Analysis of the **Villeurbanne (France)** housing market, 2017–2021  
by Zahra Bahri

---

<img width="817" height="566" alt="image" src="https://github.com/user-attachments/assets/6fc38480-4a4d-4b55-bdef-3f84dd1ce47d" />
<img width="821" height="596" alt="image" src="https://github.com/user-attachments/assets/198b8448-d560-4b2f-80c4-ae1b52e541a2" />


## 📄 Overview

This project explores **housing prices, spatial patterns, and price determinants** in Villeurbanne from 2017 to 2021.  
It combines data cleaning, exploratory data analysis (EDA), and basic modeling to identify the main factors that influence housing prices.

**Goals:**
- Understand price variations by district, area, and number of rooms  
- Examine how proximity to transport affects property values  
- Build baseline predictive models for price estimation  
- Visualize insights with clear, reproducible charts  

---

## 📁 Repository Structure
```
HousingMarket/
├── data/
│   ├── lyon_housing.csv              # raw dataset
│   ├── roomdummy.csv                 # engineered features (dummy vars)
│   └── station_coordinates.json      # transport station locations
├── notebooks/
│   ├── rain-phase2.Rmd               # R Markdown notebook
│   └── rain-phase2.html              # rendered HTML report
├── images/
│   ├── s.png                         # sample visualization
│   └── ss.jpg                        # additional chart
└── README.md
```

---

## 🚀 Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/Rain-marine/HousingMarket.git
cd HousingMarket
````

### 2. Install Dependencies

In R or RStudio:

```r
install.packages(c("tidyverse", "sf", "ggplot2", "readr", "lubridate", "janitor"))
```

### 3. Run or Knit the Analysis

Open `notebooks/rain-phase2.Rmd` in RStudio and click **Knit** → **HTML** to reproduce all charts and results.

---

## 📊 Main Visualizations

Typical visuals generated in the analysis include:

| Visualization                            | Description                                             |
| ---------------------------------------- | ------------------------------------------------------- |
| 📈 **Price Distribution Histogram**      | Shows how property prices are spread across the dataset |
| 🏘 **Price vs Surface Scatterplot**      | Illustrates how property size relates to total price    |
| 📦 **Boxplot: Price per m² by District** | Compares neighborhood-level price differences           |
| 🚉 **Map Overlay (Stations)**            | Displays housing data with public transport proximity   |

> If images are missing or blank, re-knit the R Markdown file to regenerate them.

---

## 🧮 Analytical Workflow

1. **Data Cleaning**

   * Remove duplicates, missing values, and extreme outliers
   * Standardize numeric columns and feature names
   * Merge datasets (`roomdummy.csv`, `station_coordinates.json`)

2. **Exploratory Data Analysis**

   * Distribution plots for price and area
   * Correlation checks between variables
   * Grouping and summarizing by neighborhood or room count

3. **Modeling**

   * Baseline linear regression on `price` and `price_per_m2`
   * Log-transformed models for skewed variables
   * Feature importance interpretation

4. **Visualization**

   * Boxplots, scatterplots, maps, and regression diagnostics
   * All generated within `rain-phase2.Rmd`

---

## 🧾 Data Notes

Expected key columns:

| Column                 | Description                                           |
| ---------------------- | ----------------------------------------------------- |
| `price`                | Total price of the property                           |
| `surface` or `area_m2` | Property size in square meters                        |
| `rooms`                | Number of rooms                                       |
| `district`             | Neighborhood identifier                               |
| `price_per_m2`         | Price divided by area (computed)                      |
| `lat`, `lon`           | Geographic coordinates                                |
| `station_distance`     | Distance to nearest metro/tram station (if available) |

If column names differ, adjust them in the R Markdown file before running.

---

## 🧠 Possible Extensions

* Add **spatial regression** (account for geographic clustering)
* Include **time trends** (price evolution from 2017–2021)
* Compare Villeurbanne vs Lyon or other nearby areas
* Deploy a **Shiny dashboard** for interactive price visualization

---

