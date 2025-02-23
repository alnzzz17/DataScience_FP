# Analysis of Missing Vehicles in New Zealand to Improve Public Security Effectiveness Using K-Means Clustering

## Project Description
This project aims to analyze vehicle theft patterns using the **K-Means Clustering** method in **R**. By utilizing vehicle theft data from **Motor Vehicle Theft** in New Zealand, this project helps identify high-risk areas and provides data-driven insights for public security policies and community awareness.

## Team Members
1. Erllyta Rachma A. (123220008)
2. Rizky Aprilia I. U. (123220012)
3. Zulfikar Ajie Pangarso (123220035)

## Project Objectives
- Identify vehicle theft patterns based on location.
- Cluster regions based on the level of theft risk using **K-Means Clustering**.
- Provide data-driven recommendations to enhance public security.

## Dataset
The dataset used in this project is obtained from **Motor Vehicle Theft - New Zealand Police**. It consists of three main tables:
- **locations**: Information on regions where theft occurred.
- **make_details**: Information on vehicle brands and types stolen.
- **stolen_vehicles**: Details of stolen vehicles, including color, model year, and theft date.

The dataset contains **4,510 vehicle theft records** with 16 key features.

## Methodology
1. **Business Understanding & Analytic Approach**
   - Define the problem and analytical approach using **unsupervised learning**.
2. **Data Preparation**
   - **Merging Data**: Combine *stolen_vehicles*, *locations*, and *make_details* tables.
   - **Data Cleaning**: Handle missing values and convert data types.
   - **Feature Selection**: Select relevant features for analysis.
   - **Normalization**: Apply **Min-Max Scaling**.
3. **Modeling & Evaluation**
   - Apply **K-Means Clustering** with optimal cluster selection.
   - Evaluate the model using **Silhouette Score**, **Davies-Bouldin Index**, and **Calinski-Harabasz Index**.

## R Implementation Code
This project is developed using **R** programming language with the following libraries:
- `tidyverse`
- `tidymodels`
- `ggplot2`
- `factoextra`
- `cluster`
- `shiny` (for interactive visualization)

Key implementation steps in **R**:

### 1. Import Dataset
```r
stolen_vehicles <- read.csv("stolen_vehicles.csv")
make_details <- read.csv("make_details.csv")
locations <- read.csv("locations.csv")
```

### 2. Data Cleaning & Preprocessing
```r
merged_data <- make_details %>%
  right_join(stolen_vehicles, by = "make_id") %>%
  inner_join(locations, by = "location_id")
```

### 3. Data Normalization
```r
normalize <- function(x) {
  (x - min(x)) / (max(x) - min(x))
}
normalized_data <- merged_data %>%
  mutate(across(c("population", "density", "count"), normalize))
```

### 4. Clustering with K-Means
```r
set.seed(250)
kmeans_result <- kmeans(normalized_data$count, centers = 3)
normalized_data$cluster <- kmeans_result$cluster
```

### 5. Model Evaluation
```r
dist_matrix <- dist(normalized_data$count)
silhouette_score <- silhouette(normalized_data$cluster, dist_matrix)
mean(silhouette_score[, 3])
```

### 6. Clustering Result Visualization
```r
ggplot(normalized_data, aes(x = count, y = density, color = as.factor(cluster))) +
  geom_point() +
  labs(title = "Vehicle Theft Clustering Result", x = "Number of Thefts", y = "Population Density")
```

## Results and Analysis
- **Optimal Cluster**: **3 clusters** were chosen as the optimal number based on model evaluation.
- **Cluster Analysis**:
  - **Cluster 1** (*High Risk*): Auckland recorded the highest theft cases.
  - **Cluster 2** (*Medium Risk*): Wellington, Canterbury, and Waikato had moderate risk levels.
  - **Cluster 3** (*Low Risk*): Southland, Nelson, and some other relatively safe regions.
- **Model Evaluation**:
  - **Silhouette Score**: **0.656** (indicates good cluster separation)
  - **DBI (Davies-Bouldin Index)**: **0.291** (clusters are well-separated)
  - **CH Index**: **140.385** (high structure clarity in clusters)

## Recommendations
1. **Enhancing Security in High-Risk Areas**
   Focus on Auckland with increased surveillance cameras and patrol intensity.
2. **Public Awareness Campaigns**
   Educate citizens on vehicle theft prevention strategies based on high-risk regions.
3. **Adopting Security Technologies**
   Implement alarm systems and GPS tracking for vehicles.
4. **Collaboration with Authorities**
   Utilize data to support more effective public security policies.

## Conclusion
Vehicle theft analysis using **K-Means Clustering** successfully identified regions with high, medium, and low theft risks. These insights enable targeted security strategies to reduce vehicle theft in vulnerable areas.

## References
- Data sourced from **New Zealand Police**
- Methods based on clustering research for crime analysis
