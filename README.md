# TehranHousePrice

Analyze and model **Tehran real-estate prices** with reproducible data cleaning, EDA, and baseline ML.

<p align="center">
  <img src="figures/price_hist.png" alt="Price distribution" width="32%">
  <img src="figures/price_vs_area.png" alt="Price vs area" width="32%">
  <img src="figures/price_by_district.png" alt="Price per m² by district" width="32%">
</p>


---

## 🔎 Overview

- Clean and explore Tehran housing data (apartments / villas).
- Build baseline price and price-per-m² predictors.
- Visualize key relationships (area, district, rooms, age, amenities).
- Provide a compact, reproducible workflow (R Markdown; optional Python).

---

## 📁 Repository Layout (what you’ll find here)

TehranHousePrice/
├─ data/
│ ├─ list.csv # raw listings (example name)
│ ├─ clean_list.csv # cleaned dataset used in analyses
├─ notebooks/
│ ├─ rain-phase1.Rmd # main R Markdown analysis
│ └─ rain-phase1.html # rendered report (output)
├─ figures/ # charts auto-saved here
├─ scripts/
│ ├─ generate_charts.R # EDA plots (R)
│ └─ generate_charts.py # EDA plots (Python, optional)
└─ README.md


## Quick start

### 1) Clone

git clone https://github.com/Rain-marine/TehranHousePrice.git
cd TehranHousePrice

### 2) R dependencies

install.packages(c(
  "tidyverse", "readr", "janitor", "lubridate",
  "ggplot2", "scales", "GGally"
))


### 3) Knit the main analysis (optional)

Open notebooks/rain-phase1.Rmd in RStudio and Knit to HTML.

### Generate Charts

If figures/ doesn’t exist yet, create it and run one of these scripts.

# scripts/generate_charts.R
library(tidyverse); library(readr); library(scales); library(janitor)

dir.create("figures", showWarnings = FALSE, recursive = TRUE)

# Load data
df <- read_csv("data/clean_list.csv") |> clean_names()

# Ensure price_per_m2 column exists
if (!"price_per_m2" %in% names(df) && all(c("price","area_m2") %in% names(df))) {
  df <- df |> mutate(price_per_m2 = price / pmax(area_m2, 1))
}

# 1) Price distribution
p1 <- ggplot(df, aes(price)) +
  geom_histogram(bins = 60) +
  scale_x_continuous(labels = label_number_si()) +
  labs(title = "Price distribution", x = "Price (IRR)", y = "Count")
ggsave("figures/price_hist.png", p1, width = 7, height = 4, dpi = 180)

# 2) Price vs. Area
p2 <- ggplot(df, aes(area_m2, price, color = factor(ifelse("rooms" %in% names(df), rooms, NA)))) +
  geom_point(alpha = 0.5, size = 1.4, show.legend = !all(is.na(df$rooms))) +
  scale_y_continuous(labels = label_number_si()) +
  scale_x_continuous(labels = label_number_si()) +
  labs(title = "Price vs. Area", x = "Area (m²)", y = "Price (IRR)", color = "Rooms")
ggsave("figures/price_vs_area.png", p2, width = 7, height = 4, dpi = 180)

# 3) Price per m² by District
top_d <- df |>
  count(district, sort = TRUE) |>
  slice_head(n = 12) |>
  pull(district)

p3 <- df |>
  filter(district %in% top_d) |>
  mutate(district = fct_reorder(district, price_per_m2, median, na.rm = TRUE)) |>
  ggplot(aes(district, price_per_m2)) +
  geom_boxplot(outlier.alpha = 0.2) +
  coord_flip() +
  scale_y_continuous(labels = label_number_si()) +
  labs(title = "Price per m² by district (top 12)", x = NULL, y = "IRR / m²")
ggsave("figures/price_by_district.png", p3, width = 7, height = 5, dpi = 180)

# 4) Correlation matrix (optional)
num <- df |> select(where(is.numeric)) |> drop_na()
if (ncol(num) >= 2) {
  suppressWarnings({
    library(GGally)
    p4 <- ggpairs(num, columns = 1:min(6, ncol(num)))
    ggsave("figures/corr_matrix.png", p4, width = 8, height = 6, dpi = 160)
  })
}

message("✅ Charts saved to figures/")

source("scripts/generate_charts.R")



## Modeling (baseline ideas)

Linear/elastic net on price or price_per_m2.

Tree-based models (Random Forest / XGBoost) with feature importance.

Train/test split or K-fold CV; report MAE / RMSE.

## Data expectations

Typical columns (rename in scripts if different):

price (total IRR), area_m2, price_per_m2

district, rooms, age_years, floor, elevator, parking, year_built, lat, lon
