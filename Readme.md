# Time-Series Analysis and Grocery Sales Forecasting with Prophet

## <span style="color: darkblue;">Abstract</span>
This project explores a systematic and practical approach to time-series forecasting using Facebook Prophet to predict grocery sales in Ecuador's Favorita supermarkets. Focusing on the "Grocery I" product family, the analysis highlights iterative refinements through feature engineering, incorporation of external factors, and hyperparameter optimisation to enhance predictive accuracy. By leveraging key features such as holiday adjustments, targeted regressors for anomalies like the Manabí earthquake and payday effects, and fine-tuning seasonality parameters, the project demonstrates Prophet’s flexibility and adaptability in handling complex sales patterns.

The study addresses critical challenges, such as managing extreme outliers, incorporating domain knowledge, and balancing model complexity with generalisation through cross-validation. The results show significant progress, with the final model improving upon the baseline by reducing RMSE (Root Mean Square Error) from 41,012.7 to 24,679.2, MAPE (Mean Absolute Percentage Error) from 81.5% to 7.03%, and accuracy increasing from 18.5% to 92.97%. This demonstrates the effectiveness of Prophet when carefully configured, providing very accurate results for optimising operations and solving real-world business challenges.

---

## <span style="color: darkblue;">Introduction</span>

Supermarkets play a vital role in providing essential goods and services to communities, with their operations influenced by various economic, social, and seasonal factors. Accurately predicting sales is critical for optimizing inventory management, reducing waste, and improving overall customer satisfaction. This project utilizes Facebook Prophet, a powerful open-source tool for time-series forecasting, to model and forecast sales of the "Grocery I" product family across all Favorita stores in Ecuador.

The focus on the "Grocery I" category is deliberate, as its consistent and comprehensive sales data make it an ideal candidate for exploring Prophet’s configuration and capabilities. By narrowing the scope to a single product family, the project emphasizes advanced model optimization and tuning, rather than generalizing across diverse product categories with varying sales patterns.

The dataset is sourced from the Kaggle competition "Store Sales - Time Series Forecasting." It provides detailed historical sales data, store metadata, promotions, oil prices, and holiday information, offering valuable context for understanding and forecasting sales trends. This project demonstrates the application of rigorous data analysis, feature engineering, and hyperparameter optimization to build a robust and practical forecasting model with real-world applications.

This project showcases how Facebook Prophet can be effectively fine-tuned to accurately forecast time-series events such as sales by capturing regular annual, monthly, and weekly patterns in customer behaviour while also accounting for holidays and unique, one-time events.

---

## <span style="color: darkblue;">Description of the Dataset</span>

The [Store Sales - Time Series Forecasting dataset](https://www.kaggle.com/competitions/store-sales-time-series-forecasting/data) comprises several interrelated files that collectively provide a comprehensive view of supermarket sales for the supermarket chain Favorita in Ecuador. Below is a summary of the key files and their contents:

#### Main File

1. **train.csv**:
   - Contains the primary time-series data used for training the model.
   - Key columns:
     - `store_nbr`: Identifies the store.
     - `family`: Indicates the product category. Category **Grocery I** was used in this project.
     - `sales`: The target variable, representing total sales for a product family at a store on a given date. Fractional values are possible for products sold in weights (e.g., 1.5 kg of cheese).
     - `onpromotion`: The number of items in a product family that were promoted at a store on a given date.
   
This file consists of over 3 million rows of sales data ranging from 01 January 2013 to 15 August 2018.

#### Supplementary Files

1. **stores.csv**:
   - Provides metadata about the stores, including:
     - `city` and `state`: Location of the store.
     - `type`: Store type, indicating its size or format.
     - `cluster`: Groups similar stores based on shared characteristics.
<br><br>
2. **oil.csv**:
   - Records daily oil prices, which are crucial for Ecuador’s economy as an oil-dependent nation.
   - Missing values are interpolated to ensure continuity.
<br><br>
3. **holidays_events.csv**:
   - Lists holidays and events with metadata:
     - `type`: Categorizes the event (e.g., Holiday, Additional, Bridge).
     - `transferred`: Indicates if the holiday was officially moved to another date.
     - `description`: Provides the name of the holiday or event.
     - Includes holidays that impact sales, such as Christmas and additional holidays around major events.

#### Additional Notes
- Wages in the public sector are paid every two weeks on the 15 th and on the last day of the month. Supermarket sales could be affected by this.
- A magnitude 7.8 earthquake struck Ecuador on April 16, 2016. People rallied in relief efforts donating water and other first need products which greatly affected supermarket sales for several weeks after the earthquake.

#### Key Observations
- **Seasonality**: Grocery sales exhibit clear seasonal trends, influenced by holidays, weekends, and other recurring events.
- **Economic Context**: Oil prices are included as a regressor due to their impact on Ecuador’s economy and consumer behaviour.
- **Holidays and Events**: Public sector wages, paid fortnightly, and major events like the 2016 earthquake significantly affect sales patterns.

This dataset provides a solid foundation for developing and evaluating forecasting models, consisting of both time-series trends and external factors to potentially improve prediction accuracy.

#### Train-Test Split

For this project, the focus was to make an accurate model for the **Grocery I** category which has a consistent and solid sales pattern throughout  the entire dataset. It is also the largest category, making it the core sales category of the supermarket chain.

The training set used for this project was the four calendar years from 01 January 2013 to 31 December 2016.  
The test set consists of the seven and a half months from 01 January 2017 to 15 August 2017.

---

## <span style="color: darkblue;">Exploratory Data Analysis (EDA)</span>

EDA revealed several critical insights into sales patterns:

1. **Seasonality**:
   - Daily, weekly, and yearly trends are evident.
   - Sundays are peak sales days, while Thursdays record the lowest sales.
   - December is the highest sales month due to Christmas, followed by April (Easter) and January.

2. **Trends**:
   - Promotions have increased exponentially, though their relationship with sales is nonlinear.
   - Oil prices exhibit a moderate negative correlation with sales, reflecting their impact on consumer spending.

3. **Correlations**:
   - Sales show strong positive correlations with promotions and weekly patterns.
   - Significant outliers include extreme sales days like April 18, 2016, following the Manabí earthquake.

4. **Visualizations**:
   - Time-series charts, box plots, and scatter plots highlighted these trends, confirming the importance of capturing seasonality in the model.

---

## <span style="color: darkblue;">Modelling Methodology</span>

### Data Preprocessing
- Handled missing data through interpolation for oil prices.
- Created a dedicated dataset for the **Grocery** category by aggregating sales from individual stores.

### Feature Engineering
- Time-based features like day of the week, month, and holidays were added to capture seasonality.
- Flags for key dates identified through analysis, such as the **first day of the month** and the **weekend after payday**, were incorporated to enhance modelling accuracy.

### Modelling Approach
- **Data Splitting**:
  - Training Set: January 2013 to December 2016.
  - Test Set: January 2017 to mid-August 2017.
- **Models**:
  - The initial model was created using the default configuration of Facebook Prophet as a baseline. Numerous enhancements were applied to both data preparation and Prophet’s parameter settings. Each of the seven subsequent models incorporated additional improvements, demonstrating their impact on the modelling results. The process culminated in identifying the best-performing model, which then underwent hyperparameter tuning to further optimize its performance.
  - Key steps included:
      - **Incorporating Holiday Effects**: Accounting for significant sales fluctuations during holidays to improve predictions. Holidays like New Year’s Day, with near-zero sales, were modelled separately to address extreme outliers effectively.
      - **Adding Regressors**: External events such as the Manabí earthquake and recurring patterns like payday weekends were modelled with dedicated regressors to better capture their impact on sales.
      - **Outlier Handling**: Extreme outliers, such as New Year’s Day, were modelled separately, while high-error dates in the training set were identified and addressed with additional regressors, reducing both training and test set errors.
      - **Seasonality Modes**: Leveraging additive and multiplicative seasonality modes to capture complex sales behaviours, leading to a sharper yearly seasonality after hyperparameter tuning.
      - **Hyperparameter Optimization**: A carefully chosen cross-validation process (defining the initial, period, and horizon parameters) was used to tune key hyperparameters systematically. This enhanced the model’s ability to generalize to unseen data, leading to significant improvements in metrics.
- **Evaluation Metrics**:
  - RMSE (Root Mean Square Error), MAE (Mean Absolute Error), and MAPE (Mean Absolute Percentage Error).

---

## <span style="color: darkblue;">Summary of Results</span>

### Model Summary

#### Prophet Model 1: Baseline Model
- **Objective**: Establish a baseline for forecasting grocery sales using default settings.
- **Key Outcomes**:
  - Captured general seasonality and trend components.
  - Struggled with extreme values and holidays, especially New Year’s Day.
- **Performance (Test Set)**:
  - **RMSE**: 41,012.7
  - **MAE**: 24,738.0
  - **MAPE**: 81.5%
  - **Accuracy**: 18.5%


#### Prophet Model 2.1: Incorporating Holidays
- **Objective**: Address significant inaccuracies in sales predictions during holidays, particularly New Year’s Day.
- **Additions**:
  - Incorporated Ecuadorian holiday data with windows of influence (one day before and two days after most holidays).
  - Treated New Year’s Day as a unique holiday with no influence window due to near-zero sales.
- **Key Outcomes**:
  - Improved predictions for holidays, reducing RMSE and MAPE significantly compared to the baseline model.
  - However, errors persisted for New Year’s Day, where sales are near zero.
- **Performance (Test Set)**:
  - **RMSE**: 33,199.8
  - **MAE**: 21,876.6
  - **MAPE**: 40.2%
  - **Accuracy**: 59.8%


#### Prophet Model 2.2: Model with New Year's As A Multiplicative Holiday
- **Objective**: Further refine holiday predictions by treating New Year’s Day as a multiplicative holiday.
- **Additions**:
  - Excluded **New Year's Day** from additive holiday list to treat it as multiplicative holidays.
  - Added binary flags for this holiday as regressors.
- **Key Outcomes**:
  - Improved predictions for New Year's Day, reducing RMSE significantly and MAPE substantially compared to the previous model.
  - However, large errors persisted for New Year’s Day, where sales are near zero.
  - As the performance of Model 2.3 was far better, New Year's Day was reverted back to an additive holiday.
- **Performance (Test Set)**:
  - **RMSE**: 32,534.5
  - **MAE**: 21,782.7
  - **MAPE**: 25.66%
  - **Accuracy**: 74.34%


#### Prophet Model 2.3: New Year's Day Only Model
- **Objective**: Improve accuracy for near-zero sales on New Year’s Day using a dedicated model.
- **Additions**:
  - Create a standalone Prophet model specifically for **New Year's Day** to handle its unique sales pattern.
  - Replace the **01 January** predictions in **Model 2.2** with outputs from this specialized model.
- **Key Outcomes**:
  - Vastly improved predictions for New Year's Day, reducing RMSE slightly and MAPE enormously compared to the previous model.
- **Performance (Test Set)**:
  - **RMSE**: 31,729.6
  - **MAE**: 21,238.8
  - **MAPE**: 8.28%
  - **Accuracy**: 91.72%


#### Prophet Model 3.1: Additional Regressors for Oil Prices, Earthquake Impact and Payday Weekends
- **Objective**: Account for major outliers and recurring patterns to improve model accuracy.
- **Additions**:
  - Add a specific regressor for the continuous oil price variable.
  - Add specific regressors to account for the sales impact of the **Manabí Earthquake** and its aftermath, rather than relying on imputation.
  - Include regressors for payday-related spikes, that consistently occur on the first weekends after the **1st** and **15th** of each month.
- **Key Outcomes**:
  - Significant improvement in all metrics.
  - Including oil prices alone yielded only a marginal improvement in model performance. Additionally, its practical applicability is limited, as future oil prices are difficult to forecast with certainty, especially in the longer term.
- **Performance (Test Set)**:
  - **RMSE**: 28,133.3
  - **MAE**: 19,643.9
  - **MAPE**: 7.76%
  - **Accuracy**: 92.24%


#### Prophet Model 3.2: Additional Regressors for Key Dates with High Prediction Error
- **Objective**: After additional analysis of training set prediction error, key dates were identified as areas of improvement.
- **Additions**:
  - A regressor for the **first day of the month** was added as this was often significantly underpredicted.
  - A regressor for **28 of December** was added as this was often significantly overpredicted.
- **Key Outcomes**:
  - Significant improvement in all metrics.
- **Performance (Test Set)**:
  - **RMSE**: 26,952.3
  - **MAE**: 18,853.5
  - **MAPE**: 7.49%
  - **Accuracy**: 92.51%


#### Prophet Model 4: Scaling Non-Binary Regressors
- **Objective**: Enhance model optimization by standardizing non-binary regressors to ensure consistent influence during training.
- **Additions**:
  - Standardization and evaluation of all continuous variables. This is to ensure all regressors are treated with equal weight in the modelling process.
- **Key Outcomes**:
  - Standardizing non-binary regressors yielded no change in performance. This step was excluded from the final model.
- **Performance (Test Set)**:
  - **RMSE**: 26,955.3
  - **MAE**: 18,852.6
  - **MAPE**: 7.49%
  - **Accuracy**: 92.51%
- **Outcome**: Standardizing non-binary regressors yielded performance degradation and was ultimately excluded from the final model.


#### Prophet Model 5: Cross-Validation and Hyperparameter Optimization
- **Objective**: Use Model 3.2 and optimize key hyperparameters through cross-validation to improve overall model performance.
- **Additions**:
  - Developed an extensive grid-search strategy to fine-tune hyperparameters.
  - Carefully selected cross-validation parameters to ensure a balance between training data availability and evaluation of future prediction accuracy.
- **Key Outcomes**:
  - Significant improvement in all metrics.
- **Performance (Test Set)**:
  - **RMSE**: 24,679.2
  - **MAE**: 16,990.6
  - **MAPE**: 7.03%
  - **Accuracy**: 92.97%

---

### Summary of Model Evolution

| **Model**  | **RMSE**     | **MAE**       | **MAPE**   | **Accuracy (%)** | **Key Additions**                                                |
|------------|--------------|---------------|------------|------------------|--------------------------------------------------------------------|
| **Pr1**    | 41,012.7    | 24,738.0     | 81.51%      | 18.49%            | Baseline model with general seasonality and trend components.      |
| **Pr2.1**  | 33,199.8    | 21,876.6     | 40.20%      | 59.80%            | Incorporated holiday effects into the model.                       |
| **Pr2.2**  | 32,534.50    | 21,782.7     | 25.66%     | 74.34%           | Treated New Year’s as a multiplicative holiday. Not implemented as Pr2.3's solution was far better.                    |
| **Pr2.3**  | 31,729.6    | 21,238.8     | 8.28%      | 91.72%           | Created a separate model for New Year’s Day.                       |
| **Pr3.1**  | 28,133.3    | 19,643.9     | 7.76%      | 92.24%           | Added regressors for oil prices, weekends after payday and the outlier impact of the earthquake.        |
| **Pr3.2**  | 26,952.3    | 18,853.5     | 7.49%      | 92.51%           | Added regressors for the 1st day of the month and 28 December.     |
| **Pr4**    | 26,955.3    | 18,852.6     | 7.49%      | 92.51%           | Scaled non-binary regressors for consistent influence. This had no effect so was not implemented.             |
| **Pr5**    | 24,679.2    | 16,990.6     | 7.03%      | 92.97%           | Applied cross-validation and hyperparameter tuning.                   |

---

### Key Takeaways
1. **Holiday Effects**: Incorporating holiday-specific data significantly enhanced the model's ability to accurately predict sales fluctuations during holidays, such as Carnaval and New Year’s Day.
<br><br>
2. **Outlier Handling**: 
   - Modelling **New Year’s Day** separately dramatically improved accuracy for extreme outliers with near-zero sales.
   - Treating earthquake-related sales impacts as regressors, rather than relying on imputation, effectively captured these unique anomalies and improved model performance.
<br><br>
3. **Payday Effects**: Adding regressors for the first day of the month and weekends immediately following payday (1st and 15th) significantly enhanced the model's ability to predict recurring sales spikes, leading to better overall accuracy.
<br><br>
4. **Addressing Patterns of Consistently High Prediction Errors**: By identifying dates with consistent high prediction errors in the training set, additional targeted regressors were introduced. These adjustments reduced errors for problematic dates, resulting in improved performance on the test set.
<br><br>
5. **Hyperparameter Optimization**: 
   - Cross-validation fine-tuned the model’s hyperparameters, such as changepoint prior scale and seasonality mode, enabling better generalization to unseen data.
   - Carefully selected cross-validation parameters (e.g., initial, period, and horizon) ensured a robust evaluation and further boosted the model's predictive power.
<br><br>
6. **Practicality**: 
   - Prioritize essential regressors and clearly identifiable patterns to ensure robust and actionable predictions.
   - Avoid reliance on regressors (e.g., oil prices) that are challenging to forecast accurately in real-world scenarios and have a negligible impact on the model’s performance.

This structured approach enabled the models to evolve iteratively, capturing trends, seasonality, and anomalies with greater precision while addressing specific patterns and challenges in the data. The final model provided robust and accurate forecasts for grocery sales across all stores.

---

## <span style="color: darkblue;">Conclusion</span>
This project demonstrates a comprehensive and practical approach to time-series forecasting using Facebook Prophet to predict grocery sales in Ecuador's Favorita supermarkets. By focusing on the "Grocery I" product family, the project emphasized iterative refinement through feature engineering, the incorporation of external factors, and hyperparameter optimization to improve accuracy.

**Key findings include:**
- **Holiday Effects**: Treating New Year’s Day as a unique case and integrating holiday data enhanced the model’s ability to handle extreme outliers and improve predictive performance during holidays.
- **Outlier and Anomaly Handling**: Regressors for events like the Manabí earthquake and payday weekends effectively captured unique sales patterns, reducing errors and improving accuracy.
- **Hyperparameter Optimization**: Cross-validation and systematic tuning of key parameters balanced model complexity with generalization to unseen data, enhancing overall performance.
- **Seasonality and Recurring Patterns**: Prophet’s flexibility in managing weekly, monthly, and yearly seasonality, as well as payday-related effects, addressed complex and recurring patterns in sales data.

This work underscores the effectiveness of Prophet when carefully tuned, showcasing its adaptability in addressing a range of forecasting challenges, including holidays, seasonality, and irregular events. The project highlights how combining domain knowledge, advanced forecasting techniques, and open-source tools can provide actionable insights, optimize operations, and solve real-world business problems.

Future directions include exploring other algorithms such as XGBoost and neural networks such as LSTM (Long-Short Term Memory), to compare to their performance of this dataset. Additionally, modelling other sales categories with more irregular patterns is seen as an exciting challenge that offers opportunities for further learning and development.
