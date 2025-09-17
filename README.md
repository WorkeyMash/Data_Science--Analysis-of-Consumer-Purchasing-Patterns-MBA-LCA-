# Marketing Analytics


## Overview

This repository contains the implementation of Marketing Analytics, focusing on Market Basket Analysis (MBA) and Latent Class Analysis (LCA) applied to a supermarket transactional dataset. The project analyzes customer purchasing patterns for products like margarine, meat spreads, and frozen desserts to derive actionable marketing strategies.

Key components:
- **Data**: Grocery transaction data in Excel format (`Market Basket Analysis Data For Students 2025 (1).xlsx`).
- **Code**: Jupyter Notebook (`Marketing_Analytics_Final_Assessment.ipynb`) for data preparation, association rule mining, visualization, LCA, and interpretation.
- **Report**: PDF report (`Copy of Mashabela Workers_217017696_(251DAT9X03) MARKETING ANALYTICS_Final Assessment_2025 (1).pdf`) detailing the analysis.
- **Tools Used**: Python with libraries such as pandas, mlxtend (for Apriori algorithm), scikit-learn (for KMeans as LCA proxy), matplotlib, and networkx.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Introduction](#introduction)
- [Section A: Data Preparation](#section-a-data-preparation)
- [Section B: Association Rule Analysis](#section-b-association-rule-analysis)
- [Section C: Visualisation and Interpretation](#section-c-visualisation-and-interpretation)
- [Section D: Marketing Strategy](#section-d-marketing-strategy)
- [Section E: Latent Class Analysis (LCA)](#section-e-latent-class-analysis-lca)
- [Conclusion](#conclusion)
- [References](#references)
- [Contributing](#contributing)
- [License](#license)

## Installation

To run the analysis locally:

1. Clone the repository:
   ```
   git clone https://github.com/your-username/marketing-analytics-assessment.git
   cd marketing-analytics-assessment
   ```

2. Install required dependencies (Python 3.12+ recommended):
   ```
   pip install pandas mlxtend scikit-learn matplotlib networkx
   ```

No additional installations are needed beyond these libraries.

## Usage

1. Open the Jupyter Notebook:
   ```
   jupyter notebook Marketing_Analytics_Final_Assessment.ipynb
   ```

2. Run the cells sequentially:
   - Load and clean the data.
   - Generate association rules using Apriori.
   - Visualize rules and product networks.
   - Perform LCA via KMeans clustering.
   - Interpret results.

Example code snippet for association rule generation (from the notebook):

```python
from mlxtend.frequent_patterns import apriori, association_rules

# Assuming 'df_clean' is the cleaned binary transaction DataFrame
frequent_itemsets = apriori(df_clean[product_cols], min_support=0.05, use_colnames=True)
rules = association_rules(frequent_itemsets, metric="lift", min_threshold=1)

# Sort and display top rules
top_rules = rules.sort_values(by='lift', ascending=False).head(15)
print(top_rules)
```

This will output rules like:

| antecedents              | consequents     | antecedent support | consequent support | support | confidence | lift  | leverage | conviction | zhangs_metric |
|--------------------------|-----------------|--------------------|--------------------|---------|------------|-------|----------|------------|--------------|
| (margarine, meat spreads)| (frozen dessert)| 0.105098          | 0.232544          | 0.091874| 0.874251  | 3.7604| 0.067443| 5.827     | 0.829       |

## Introduction

A thorough examination of consumer purchasing patterns utilizing Market Basket Analysis (MBA) and Latent Class Analysis (LCA) on a transactional dataset of supermarket products led to the development of the comprehensive marketing plan described in this study.

Cleaning, transforming, analyzing, interpreting, and applying the data's insights to support successful marketing strategies was the aim of this study. In order to classify customers according to their shopping behaviors and find strong links between products, we looked at purchasing patterns. High confidence and lift values in the rules connecting margarine, meat spreads, and frozen desserts indicate a significant linkage between these commodities, according to a major finding from the Market Basket Analysis. In particular, rules like (margarine, meat spreads) ⇒ frozen dessert demonstrated significant lift (~3.76) and confidence (~87.43%), suggesting that consumers who purchase margarine and meat spreads together are quite likely to also buy frozen desserts. This was supported by complementary analyses of product association values, which revealed that among the three pairs, margarine and frozen dessert had the strongest correlation. Concurrently, several client segments were determined via Latent Class Analysis according to their likelihood of buying these products. Customers that consistently buy all three products, for example, make up Segment 2. With an emphasis on increasing sales of these identified product combinations, this report uses these insights to outline marketing strategies that are specifically intended to promote co-purchases of these frequently associated items. These strategies include targeted bundling strategies, promotions, and strategic product placements.

## Section A: Data Preparation

As stated in the assignment structure, the first stage entails getting the supplied grocery dataset ready for analysis. The actions listed below were taken, as explained in detail in Section A of the Appendix that is attached:

01. Open the grocery dataset that was supplied. The transactional dataset, which is identified as already being in transactional format for Section B, is loaded first in the analysis.

02. The 'Customer ID' and product columns should be the only ones left in the dataset. This entails separating the pertinent product data and client identifiers for analysis.

03. Take out any columns that aren't relevant. To make the data more streamlined, any columns in the original dataset that are not necessary for identifying customer purchases or the products themselves are eliminated.

04. Create a CSV file from the cleaned dataset so it can be analyzed. The processed data is subsequently saved into a Comma Separated Values (CSV) file format, making it ready for additional analysis using various tools.

## Section B: Association Rule Analysis

To find trends in the co-occurrence of grocery products within transactions, Association Rule Analysis was used.

01. Python was selected as the software tool for this investigation. This procedure would be made easier by the built-in capabilities for transaction encoding, data preprocessing, and rule mining that Python packages like mlxtend are known to offer.

02. Generate association rules.

As explained in Appendix Section B: Association Rule Analysis, association rules were produced using the Apriori method and the cleaned transactional data in Python. The procedure entailed creating rules that are distinguished by important metrics such as Lift, Confidence, and Support. A thorough grasp of the rules was also thought to be possible with the use of additional metrics that are commonly employed in association rule analysis, including Leverage, Conviction, Zhang's Metric, Jaccard, Certainty, and Kulczynski.

The produced rules and the associated metrics are shown in Figure 1 (recreated as Markdown table below):

| ID | Antecedents                  | Consequents     | Support | Confidence | Lift  | Leverage | Conviction | Zhang's Metric | Jaccard | Certainty | Kulczynski |
|----|------------------------------|-----------------|---------|------------|-------|----------|------------|---------------|---------|-----------|------------|
| 14 | (meat spreads, margarine)   | (frozen dessert)| 0.091874| 0.874251  | 3.7604| 0.067443| 5.827     | 0.829        | 0.641  | 0.64588  | 0.487     |
| 13 | (frozen dessert, meat spreads)| (margarine)   | 0.091874| 0.843931  | 4.0760| 0.069337| 5.062     | 0.845        | 0.653  | 0.65321  | 0.492     |
| 12 | (frozen dessert, margarine) | (meat spreads) | 0.091874| 0.834286  | 4.3602| 0.070794| 5.031     | 0.859        | 0.659  | 0.64124  | 0.493     |
| 6  | (meat spreads)              | (frozen dessert)| 0.108874| 0.569079  | 2.4473| 0.064402| 2.809     | 0.706        | 0.467  | 0.33679  | 0.401     |
| ...| ...                         | ...             | ...     | ...        | ...   | ...      | ...        | ...           | ...     | ...       | ...        |

(Note: Full table truncated; refer to notebook for complete output.)

The produced top rules and the associated metrics are shown in Figure 2 (top rules sorted by lift):

| Antecedents                  | Consequents     | Support | Confidence | Lift  |
|------------------------------|-----------------|---------|------------|-------|
| (frozen dessert, meat spreads)| (margarine)   | 0.091874| 0.843931  | 4.0760|
| (meat spreads, margarine)   | (frozen dessert)| 0.091874| 0.874251  | 3.7604|
| (frozen dessert, margarine) | (meat spreads) | 0.091874| 0.834286  | 4.3602|
| (meat spreads)              | (margarine)     | 0.105098| 0.549342  | 2.6532|
| (meat spreads)              | (frozen dessert)| 0.108874| 0.569079  | 2.4473|

03. Interpreting the top rules in detail using support, confidence, and lift.

The following is an interpretation of the top rules that were found, with an emphasis on the connections between margarine, meat spreads, and frozen desserts, as well as their metrics:

a. Rule 14: (margarine, meat spreads) ⇒ frozen dessert  
   i. Support: around 9.19 percent. This indicates that margarine, meat spreads, and frozen dessert are combined in 9.19% of all transactions in the dataset.  
   ii. Approximately 87.43% confidence. This indicates that when margarine and meat spreads are bought together, frozen dessert is also bought in 87.43% of transactions.  
   iii. Lift: about 3.76. Consumers who purchase meat spreads and margarine are roughly 3.76 times more likely to purchase frozen desserts than by chance.

(Similar interpretations for other rules; see full report for details.)

04. Explain the significance of each rule in terms of product combinations and potential sales.

Important rules include (margarine, meat spreads) ⇒ frozen dessert, etc. These have high lift and confidence, offering growth prospects through cross-selling and bundling.

## Section C: Visualisation and Interpretation

Understanding the connections and patterns found by ARM is aided by visualizations.

01. A graph for the rules showing antecedents and consequents.

A scatter plot of association rules shows rules according to their confidence and support (Figure 1 in report).

02. A network or connection graph of the most connected products.

The product association network (Figure 2 in report) shows strong bonds between margarine and frozen dessert (~4.0760 lift).

03. Interpret the relationships and discuss patterns and insights.

The visuals show a distinct pattern of co-purchases for these products, suggesting complementary usage.

## Section D: Marketing Strategy

The insights form the foundation of the marketing strategy, focusing on promoting co-purchases.

01. Foundation in Advanced Data Analytics and Segmentation.

02. Segment-Based Targeted Marketing.

03. Incentives and Promotions (Bundling Strategies).

04. Strategic Product Placement (In-Store).

05. In-Store Sampling and Displays.

06. Leveraging Digital Channels and Omnichannel Integration.

07. Social media and content marketing.

08. Seasonal Campaigns.

09. Continuous Monitoring and Optimization.

10. Ethical Considerations.

(See full report for detailed strategies.)

## Section E: Latent Class Analysis (LCA)

01. Procedure: Using KMeans on purchase data to identify 4 segments (code snippet from notebook):

```python
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler

# Products of interest: margarine, meat spreads, frozen dessert
X = lca_df[products_of_interest]
X_scaled = StandardScaler().fit_transform(X)
kmeans = KMeans(n_clusters=4, random_state=42)
lca_df['Segment'] = kmeans.fit_predict(X_scaled)

segment_summary = lca_df.groupby('Segment')[products_of_interest].mean()
print(segment_summary)
```

Output:

| Segment | Margarine | Meat Spreads | Frozen Dessert |
|---------|-----------|--------------|----------------|
| 0       | 0.000000  | 1.000000     | 0.197080       |
| 1       | 0.122130  | 0.000000     | 0.000000       |
| 2       | 1.000000  | 1.000000     | 0.874251       |
| 3       | 0.147959  | 0.000000     | 1.000000       |

Segment Sizes: 0 (137), 1 (1089), 2 (167), 3 (196).

02. Reflecting on how these segments align with association rules.

Segment 2 aligns strongly with high-lift rules, while others support weaker associations.

## Conclusion

The analysis provides a data-driven marketing plan to boost sales through targeted strategies based on identified patterns and segments.

## References

1. Huaman Llanos, A.A., et al. (2024). Toward Enhanced Customer Transaction Insights: An Apriori Algorithm-based Analysis of Sales Patterns at University Industrial Corporation. *International Journal of Advanced Computer Science & Applications*, 15(2).

2. Hruschka, H. (2024). Analyzing market basket data through sparse multivariate logit models. *Journal of Marketing Analytics*, pp.1-16.

3. Omol, E., et al. (2024). Apriori algorithm and market basket analysis to uncover consumer buying patterns: Case of a Kenyan supermarket. *Buana Information Technology and Computer Sciences (BIT and CS)*, 5(2), pp.51-63.

4. Sajwan, I. and Tripathi, R. (2024). Unveiling Consumer Behavior Patterns: A Comprehensive Market Basket Analysis for Strategic Insights. In *2024 Sixth International Conference on Computational Intelligence and Communication Technologies (CCICT)* (pp. 372-377). IEEE.

5. Sun, C. (2024). Data Analysis of Customer Segmentation and Personalized Strategy in the Era of Big Data. *Advances in Economics, Management and Political Sciences*, 92, pp.46-52.

## Contributing

This is an academic project. Contributions are not expected, but feel free to fork and experiment.

## License

This project is for educational purposes only. No license is specified.
