# &#x09;			   Machine Learning Notes



##### 

1. ##### Steps for making machine learning model

&#x09;

* ###### Problem definition
* ###### Data Collection
* ###### Exploratory Data Analysis(EDA)

  * It is a step where we explore the data
  * Understand the data
  * Discover patterns
  * Spot anomalies
  * Generate insight
  * And decide what to do next
  1. Steps of EDA

     * Viewing Data :- Head , Tail , Info , shape , What column I have ? , What type of data ?
     * Summary Statistics :- mean , median , mode etc
     * Value Counts :- How many unique Value in the column? Great for categorical columns
     * Missing Value Analysis :- Where are the gaps? What are the percent of the data is missing?
     * Visualizations :- Histogram , Boxplots, Bar plots , Correlation Heatmap , Scatter plots
     * Target Variable Exploration :- How does your output is connected to my input data
  2. Importance of EDA

     * You can't clear or preprocess what you don't understand
     * It helps you identify mistake, biases or limitation in the data
     * It guides the direction of your data cleaning and feature engineering.
     * It gives your audience or stakeholder a story or overview of what the data is telling you
* ###### &#x20;Data Cleaning
1. Strategies to handle 

   * Drop missing rows / columns (only if very few)
   * Impute with :

     * Mean , Median :- for numerical data 
     * Mode :- for categorical data
     * Advance :- Linear regression , KNN or interpolation (for future learning)

&#x20;  2. Remove Duplicate

       * Detect and drop exact same duplicate rows

&#x20;  3. Fix Data Type 

       * Convert wrong data type in correct one

&#x20;  4. Handles Inconsistent Data 

       * "male" , "Male"  should become "male"

&#x20;  5. Fix logic or Domain Errors 

       * age = -5 is invalid , or BMI = 250  likely an error
       * Can be replaced by mean, median or remove
* ###### Data Preprocessing

  1. Encoding Categorical Variables

     * Covert text labels into number (Because ML algorithms can't able to understand text labels like "male", "Y" etc)
     * &#x20;
* ###### Feature Selection \& Engineering
* ###### Split the Data Set
* ###### Model Selection
* ###### Model Training
* ###### Model Evolution
* ###### Hyperparameter Tuning
* ###### Model Testing / Validation

