# Customer Segmentation Analysis

This repository implements customer segmentation using K-means clustering on sales transaction data. The analysis creates user-product matrices, determines optimal clusters via silhouette analysis, and profiles segments by demographics and purchasing behavior. 

## Project Overview

The project processes member demographics (`members.csv`) and 2024 sales data (`sales-2024.csv`) to identify customer segments. Key steps include correlation heatmaps of product purchases, optimal K selection, clustering, and segment profiling with visualizations and summary tables.

## Key Components

- **Data Preparation**: User-product matrix with purchase quantities; Pearson correlation matrix.
- **Exploratory Analysis**: Product correlation heatmap using `pheatmap`.
- **Clustering**: K-means with silhouette analysis to select K=5; cluster assignment for all members.
- **Profiling**: Top products per cluster; demographic distributions (gender, age, card level).
- **Visualization**: Bar plots, histograms, and summary tables with `ggplot2` and `kableExtra`.

## Requirements

- R 4.0+
- Core packages: `dplyr`, `ggplot2`, `tidyr`, `readr`
- Visualization: `pheatmap`, `kableExtra`
- Clustering: `cluster`, `factoextra`

Install via:
```r
install.packages(c("dplyr", "ggplot2", "tidyr", "readr", "pheatmap", "kableExtra", "cluster", "factoextra"))
```

## Usage

1. Place `members.csv` and `sales-2024.csv` in the repository root.
2. Set working directory and run: `rmarkdown::render("Segmentation-1.Rmd")`.
3. Outputs include correlation heatmap, silhouette plot, cluster profiles, and demographic visualizations.

## Results Structure

| Analysis | Output Description |
|----------|-------------------|
| Product Correlation | Heatmap of item-item Pearson correlations |
| Optimal K | Silhouette plot showing peak at K=5 |
| Cluster Tops | Top 5 products per cluster by total quantity |
| Demographics | Gender/age/card level proportions and distributions |
| Summary Table | Cluster profiles with top 3 products |

Analysis reveals distinct segments based on product preferences and member characteristics.

## Code Highlights

Core clustering pipeline:

```r
# User-product matrix
user_product <- sales %>%
  group_by(member_id, product) %>%
  summarise(qty = sum(quantity), .groups = "drop") %>%
  pivot_wider(names_from = product, values_from = qty, values_fill = 0)

product_matrix <- as.matrix(user_product[,-1])

# Silhouette analysis for optimal K
sil <- sapply(2:10, function(k){
  km <- kmeans(product_matrix, centers=k, nstart=20)
  ss <- silhouette(km$cluster, dist(product_matrix))
  mean(ss[,3])
})

# K-means clustering (K=5)
set.seed(3567)
km_res <- kmeans(product_matrix, centers=5, nstart=25)
```

Demographic profiling uses stacked bar plots and summary tables for interpretation.

## Limitations

- Assumes CSV data availability (not included due to privacy).
- Fixed seed (3567) for reproducibility; silhouette method selects K=5.
- No advanced preprocessing or hierarchical clustering.

## Author

Ting-Ying Lai  
National Taiwan University  
Data Science Portfolio
