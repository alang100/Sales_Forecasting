# Time-Series Analysis and Forecasting: Grocery Sales in Ecuadorian Stores

### 1. Introduction

Supermarkets play a vital role in providing essential goods and services to communities, and their operations are influenced by various economic, social, and seasonal factors. Accurately predicting sales can help optimize inventory management, reduce waste, and improve overall customer satisfaction. In this project, Facebook Prophet, a powerful open-source tool for time series forecasting, is employed to model and forecast sales of the "Grocery I" product family across all Favorita stores in Ecuador.

The focus on the "Grocery I" category is deliberate, as its sales data is consistent and spans the entire dataset, making it ideal for exploring Prophet’s configuration and capabilities. By narrowing the scope to a single product family, the project emphasizes model optimization and tuning rather than generalizing across diverse product categories with varying sales patterns.

The dataset utilized for this project is sourced from the Kaggle competition "Store Sales - Time Series Forecasting." It contains detailed historical sales data, store metadata, promotions, oil prices, and holiday information, all of which provide valuable context for understanding and forecasting sales trends.

### 2. Description of the Dataset

The [Store Sales - Time Series Forecasting dataset](https://www.kaggle.com/competitions/store-sales-time-series-forecasting/data) comprises several interrelated files that collectively provide a comprehensive view of supermarket sales in Ecuador. Below is a summary of the key files and their contents:

#### Main Files

1. **train.csv**:
   - Contains the primary time-series data used for training the model.
   - Key columns:
     - `store_nbr`: Identifies the store.
     - `family`: Indicates the product category.
     - `sales`: The target variable, representing total sales for a product family at a store on a given date. Fractional values are possible for products sold in weights (e.g., 1.5 kg of cheese).
     - `onpromotion`: The number of items in a product family that were promoted at a store on a given date.

2. **test.csv**:
   - Contains the features for which sales predictions are required.
   - Includes the same columns as `train.csv` except for the target variable `sales`.
   - Covers the 15 days following the last date in `train.csv`.

3. **sample_submission.csv**:
   - Provides a template for the submission format.

#### Supplementary Files

1. **stores.csv**:
   - Provides metadata about the stores, including:
     - `city` and `state`: Location of the store.
     - `type`: Store type, indicating its size or format.
     - `cluster`: Groups similar stores based on shared characteristics.

2. **oil.csv**:
   - Records daily oil prices, which are crucial for Ecuador’s economy as an oil-dependent nation.
   - Missing values are interpolated to ensure continuity.

3. **holidays_events.csv**:
   - Lists holidays and events with metadata:
     - `type`: Categorizes the event (e.g., Holiday, Additional, Bridge).
     - `transferred`: Indicates if the holiday was officially moved to another date.
     - `description`: Provides the name of the holiday or event.
     - Includes holidays that impact sales, such as Christmas and additional holidays around major events.

#### Key Observations
- **Seasonality**: Grocery sales exhibit clear seasonal trends, influenced by holidays, weekends, and other recurring events.
- **Economic Context**: Oil prices are included as a regressor due to their impact on Ecuador’s economy and consumer behavior.
- **Holidays and Events**: Public sector wages, paid biweekly, and major events like the 2016 earthquake significantly affect sales patterns.

This dataset provides a rich foundation for developing and evaluating forecasting models, leveraging both time-series trends and external factors to improve prediction accuracy.

## Dataset Description

The dataset, part of a Kaggle competition, comprises four main components:

1. **Sales Data**: Daily sales and promotions by store and product family.
2. **Oil Prices**: Daily oil prices, critical for Ecuador's economy and retail sales.
3. **Holidays**: A record of local, regional, and national holidays, including transferred and bridge days.
4. **Store Information**: Metadata about stores, such as location, type, and cluster.

The "Grocery I" category was selected due to its robust daily sales and pronounced seasonality, making it suitable for a focused forecasting study.

## Exploratory Data Analysis (EDA)

EDA revealed several critical insights into sales patterns:

1. **Seasonality**:
   - Daily, weekly, and yearly trends are evident.
   - Sundays are peak sales days, while Thursdays record the lowest sales.
   - December is the highest sales month due to Christmas, followed by April (Easter) and January (New Year).

2. **Trends**:
   - Promotions have increased exponentially, though their relationship with sales is nonlinear.
   - Oil prices exhibit a moderate negative correlation with sales, reflecting their impact on consumer spending.

3. **Correlations**:
   - Sales show strong positive correlations with promotions and weekly patterns.
   - Significant outliers include extreme sales days like April 18, 2016, following the Manabí earthquake.

4. **Visualizations**:
   - Time-series charts, box plots, and scatter plots highlighted these trends, confirming the importance of capturing seasonality in the model.

## Methodology

### Data Preprocessing
- Handled missing data through interpolation for oil prices.
- Created a dedicated dataset for "Grocery I" by aggregating sales and promotion data.

### Feature Engineering
- Time-based features like day of the week, month, quarter, and wet/dry seasons were added to capture seasonality.
- Flags for "first weekend of the month" and "weekend after payday" were included to account for payday effects.

### Modeling Approach
- **Data Splitting**:
  - Training Set: January 2013 to December 2016.
  - Test Set: January 2017 to mid-August 2017.
- **Models**:
  - FB Prophet to capture yearly and weekly seasonality and trends.
  - Linear Regression as a baseline to explore relationships between features and sales.
- **Evaluation Metrics**:
  - RMSE (Root Mean Square Error), MAE (Mean Absolute Error), and MAPE (Mean Absolute Percentage Error).

## Results

- **FB Prophet**: Effectively captured yearly and weekly seasonality but struggled with extreme outliers, such as sales spikes after the Manabí earthquake.
- **External Regressors**: Incorporating oil prices and log-transformed promotions significantly improved performance.
- **Linear Regression**: Provided a simple baseline but lacked the complexity to fully model seasonality and trends.

## Conclusion

This project provided a comprehensive analysis of the "Grocery I" category, leveraging feature engineering and time-series modeling. The strong seasonality and influence of promotions and oil prices were key findings. 

### Future Work
- Refine models to better handle extreme events, such as earthquakes and holiday anomalies.
- Explore advanced techniques like Long Short-Term Memory (LSTM) networks for capturing complex dependencies.
- Extend the methodology to other product categories and stores for broader forecasting applications.

This work serves as a foundation for forecasting in retail settings with similar datasets.
