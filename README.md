# International Student Demography 📊

This project analyzes trends in international student admissions in the U.S. by combining multiple datasets to explore student demographics, academic preferences, funding sources, and visa statuses. The insights are designed to support policy decisions and strategic planning in international education. 🌍

## Data Sources
- **Academic Enrollment:** Overall student counts (international vs. U.S.) 🏫
- **Origin Data:** Countries of origin for international students 🌐
- **Academic Details:** Enrollment distribution by academic level (undergraduate, graduate, etc.) 🎓
- **Field of Study & Funding:** Information on chosen academic fields and funding sources 💰
- **Visa Status:** Data on visa types and gender distribution among students 🛂

## Methodology
- **Data Cleaning:**  
  - Standardized year formats (e.g., converting "1948/49" to "1948") using `tidyverse`, `stringr`, and `rebus`.  
  - Handled missing values and ensured data consistency across datasets.
- **Exploratory Analysis:**  
  - Aggregated data by year, country, and academic type to uncover trends.  
  - Summarized key statistics to understand student demographics.
- **Visualization:**  
  - Created static charts with `ggplot2` for clear and informative plots.
  - Developed animated visualizations using `gganimate` to illustrate changes over time.
  - Built interactive maps with `plotly` for geographical insights.
  - Combined multiple plots using `patchwork` for side-by-side comparisons.

## Key Insights
- **Rising Enrollment:**  
  International student numbers have steadily increased over the past 20 years, with noticeable dips during significant events like the COVID-19 pandemic. 📈
- **Dominant Origins:**  
  A large proportion of international students come from Asia (especially China and India), with emerging contributions from other regions.
- **Academic Trends:**  
  There is a clear shift toward higher enrollment in bachelor's programs after 2010, along with a sustained interest in STEM fields.
- **Funding Patterns:**  
  Most international students rely on personal and family funds, indicating potential areas for enhanced financial aid and scholarship programs.
- **Visa & Gender Distribution:**  
  The F1 visa is the most common, and gender trends show a consistently higher number of male students overall.


### Data Cleaning
Standardized the year formats and handled missing values using `tidyverse`, `stringr`, and `rebus`. For example, to clean the "year" column:
  
```r
library(stringr)
library(rebus)

# Define the pattern to remove trailing "/xx" from the year column
pattern <- "/" %R% DGT %R% DGT

# Replace the pattern and convert to a numeric year
academic$year <- str_replace(academic$year, pattern = pattern, replacement = "")
academic$year <- as.numeric(format(as.Date(academic$year, format = "%Y"), "%Y"))
```

**Static Visualization** 

A basic bar chart to display international student numbers over time:

```r
library(ggplot2)
ggplot(academic, aes(x = year, y = students / 1000)) +
  geom_bar(stat = "identity", fill = "red") +
  scale_y_continuous(labels = function(x) paste0(x, "K")) +
  labs(title = "International Students in Last 20 Years",
       x = "Year", y = "No. of Students (thousands)") +
  theme_classic()
```

**Animated Visualization**

An animated bar chart to show how metrics change over time:
```r
library(gganimate)
p <- ggplot(academic, aes(x = year, y = students / 1000)) +
  geom_bar(stat = "identity", fill = "red") +
  labs(title = 'Year: {frame_time}', x = 'Year', y = 'Students (thousands)') +
  transition_time(year) +
  ease_aes('linear')
animate(p, nframes = 50)
```

**Interactive Map**

An interactive map using plotly to visualize the origins of international students:
```r
library(plotly)
library(maps)
world <- map_data("world")
map_data <- merge(world, academic, by.x = "region", by.y = "origin", all.x = TRUE)
interactive_map <- ggplotly(
  ggplot(map_data, aes(x = long, y = lat, group = group, fill = students)) +
    geom_polygon(color = "white", size = 0.1) +
    scale_fill_gradient(low = "lightblue", high = "darkblue", name = "Students") +
    theme_minimal() +
    labs(title = "International Student Origins")
)
interactive_map
```








