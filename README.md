# Fisheries Inventory: Trout Data at Rocky Mountain National Park (2021–2022)

---

## 📌 Introduction
This project analyzes trout population data collected from Rocky Mountain National Park between 2021 and 2022. It aims to understand how wildfire events may have impacted trout population structure, abundance, and associated aquatic ecosystems.

---

## 🌍 Scenario
As an aspiring data analyst contributing to wildlife conservation and monitoring initiatives, I explored data collected by the National Park Service. This case study focuses on the ecological effects of the 2020 Cameron Peak and East Troublesome wildfires on fish populations in high-altitude streams.

---

## ❓ Ask

### Business Task: Determine how recent wildfires influenced trout populations in Rocky Mountain National Park. The analysis investigates trends in trout counts, sizes, and species composition across different sites and years.

---

## 📦 Prepare 
**Dataset:**  Fisheries Inventory, Trout Data, at Rocky Mountain National Park 2021–2022.  
**Source:** [Catalog.Data.Gov](https://catalog.data.gov/dataset/fisheries-inventory-trout-data-at-rocky-mountain-national-park-2021-2022-open-format-datas)

# Load necessary libraries
library(tidyverse)
library(lubridate)
library(readr)

# Load data
ROMOFish_TroutData_SurveyEvent <- read_csv("Wildfire Impact on Trout/ROMOFish_TroutData_SurveyEvent.csv")
ROMOFish_TroutData_Occurrence <- read_csv("Wildfire Impact on Trout/ROMOFish_TroutData_Occurrence.csv")
ROMOFish_TroutData_Taxon <- read_csv("Wildfire Impact on Trout/ROMOFish_TroutData_Taxon.csv")

---

## 🧹 Process

```r
# Load necessary libraries
library(tidyverse)
library(lubridate)

# Load dataset
SWAN_FW_Contaminants_Data_LACL_2019_2020 <- read_csv("SWAN_FW_Contaminants_Data_LACL_2019_2020.csv")

# Select and rename key variables
processed_data <- filtered_data %>%
  select(-Comments, -Methyl_Hg) %>%
  rename(
    total_hg = Total_Hg,
    length = Length,
    weight = Weight,
    age = Age
  )

# Filter out rows with missing values in key fields
processed_data <- processed_data %>%
  filter(!is.na(total_hg), !is.na(length), !is.na(weight), !is.na(age))

# Convert date column if applicable (if needed for temporal analysis)
processed_data$date <- mdy(processed_data$Date_Collected)

# Inspect cleaned data
summary(processed_data)

```

## 📊 Analyze
* Spatial Analysis: Compared mercury concentrations between Fishtrap Lake and Lake Clark.
* Temporal Analysis: Assessed year-to-year differences between 2019 and 2020 samples.
* Biological Analysis:
  * Correlated fish length and weight with mercury concentration.
  * Analyzed species-specific differences in mercury accumulation.
* Outlier Detection: Identified individual fish with exceptionally high mercury levels for further attention

```{r}
# Summary stats by lake
summary_stats_lake <- processed_data %>%
  group_by(Lake) %>%
  summarise(
    avg_mercury = mean(total_hg, na.rm = TRUE),
    max_mercury = max(total_hg, na.rm = TRUE),
    sample_count = n()
  )
summary_stats_lake

# Summary stats by species
summary_stats_species <- processed_data %>%
  group_by(Species) %>%
  summarise(
    avg_mercury = mean(total_hg, na.rm = TRUE),
    max_mercury = max(total_hg, na.rm = TRUE),
    sample_count = n()
  )
summary_stats_species
```

---

## 📤 Share

![Plot Mercury by Lake](https://github.com/ssagastume11/Mercury-Concentrations-in-Resident-Lake-Fish/blob/main/plot_mercury_by_lake.png)
```{r}
# Plot: Mercury concentration by lake
ggplot(processed_data, aes(x = Lake, y = total_hg)) +
  geom_boxplot(fill = "#69b3a2") +
  labs(title = "Mercury Concentration by Lake",
       x = "Lake",
       y = "Total Mercury (ppm)",
       caption = "Source: SWAN Mercury Monitoring Dataset, catalog.data.gov")
```

![Plot Mercury by Species](https://github.com/ssagastume11/Mercury-Concentrations-in-Resident-Lake-Fish/blob/main/plot_mercury_by_species_v2.png)
```{r}
# Plot: Mercury concentration by species
ggplot(processed_data, aes(x = Species, y = total_hg)) +
  geom_boxplot(fill = "#d95f02") +
  labs(title = "Mercury Concentration by Species",
       x = "Species",
       y = "Total Mercury (ppm)",
       caption = "Source: SWAN Mercury Monitoring Dataset, catalog.data.gov")
```

![Plot Mercury vs. Fish Weight by Species](https://github.com/ssagastume11/Mercury-Concentrations-in-Resident-Lake-Fish/blob/main/plot_mercury_vs_weight_species_v2.png)
```{r}
# Optional: Relationship between mercury and weight
ggplot(processed_data, aes(x = weight, y = total_hg, color = Species)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  labs(title = "Mercury vs. Fish Weight by Species",
       x = "Weight (g)",
       y = "Total Mercury (ppm)",
       caption = "Source: SWAN Mercury Monitoring Dataset, catalog.data.gov")
```

---

# 🛡️ Act
## Summary of Key Findings
- Mercury concentrations vary by lake, with **Lake Clark** showing higher average levels than Kijik Lake.
- Among species, **Lake trout** exhibited the highest mercury concentration on average.
- There may be slight **temporal variation** between 2019 and 2020, but more data is needed to confirm trends.
- A **positive relationship** appears between fish weight and mercury concentration, particularly in lake trout.

# 🧩 Recommendations
1. **Prioritize species** for further study and potential public health advisories due to elevated mercury concentrations.
2. **Share findings** to National Park Service ecologists, fisheries biologists, and public health officials to inform conservation and safety strategies.
3. **Expand future monitoring** to other lakes within the SWAN region and include more years to strengthen trend analysis.
4. **Educate local communities** and park visitors about mercury exposure and safe fish consumption practices.

---

### 🔗 Data Source
[Fisheries Inventory, Trout Data, at Rocky Mountain National Park 2021–2022 – catalog.data.gov](https://catalog.data.gov/dataset/fisheries-inventory-trout-data-at-rocky-mountain-national-park-2021-2022-open-format-datas)
