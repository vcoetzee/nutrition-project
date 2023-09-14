# nutrition-project (Currently in progress/ to be updated)

# Title: Predicting body composition and blood pressure in a South African student population 
## Brief Introduction
Most research is conducted in WEIRD (Western, Educated, Industrialised, Rich, and Democratic) countries. Less than 1% of the world's research is conducted in Africa. In this study we wanted to explore which demographic and/or lifestyle factors predict aspects of health, specifically body composition and blood pressure, in a Black South African student population. Once these factors are better understood, they can be used to improve health and identify risk factors in this population. The aim of this study is therefore to determine which demographic and/or lifestyle factors predict body composition and blood pressure in a South African student population.

## Methods
### Data
The African Longitudinal Facial Appearance and Health Study (ALFAH) study was approved by research ethics committees at the Faculties of Health Sciences (259/2016), Natural and Agricultural Sciences (EC160429-024) and Humanities (25309995/ HUM030/0819) at the University of Pretoria, South Africa. Two hundred and seventy-one (N=271) African participants (53.3% male) aged 18 to 29 (mean: 20.79) were recruited for the study from the University of Pretoria and the Sefako Makgatho University, Pretoria, South Africa. Written informed sent was obtained from all participants. Participants were asked to complete a questionnaire, including questions on basic demographic variables, their health, exercise and diet (in the past 7 days). Each participant's body composition (i.e. body weight, percentage body fat, muscle mass etc.) were measured with a Tanita MC 780 MA Segmental Body Composition Analyser; their blood pressure with an automated blood pressure monitor and their standing and sitting height with a wall-mounted stadiometer. The data was stored in a MySQL database (Server version: 8.0.33 MySQL Community Server - GPL).

### Data analyses
The number of missing values were calculated. Four columns and one row with more than 10% missing data (and other unnecesary colums) were dropped from the dataframe. Missing values in 41 numerical variables (range 0.4-6.7% missing) were replaced by the mean and missing values in two categorical variables (1.1-3.0% missing) were replaced by the most common category. The data format was corrected for all remaining variables and the data visualised. The two exercise variables were combined into a single, weighted exercise variable (exercise = moderate exercise + (2 * strenious exercise)), and the 14 food frequency variables were combined into four food indixes: fruit & veg index (fruit juice, fruit and vegetables), carbs index (bread, pap and samp, rice and pasta), dairy index (dairy) and protein index (red meat, chicken, pork, fish and eggs). There were insufficient data for a 'fat' index, but sufficient data to add a junkfood index (take aways and soft drinks). These food indices were only weakly correlated with each other (all r<0.48). Two standing height variables that were swapped with sitting height, were corrected. Systolic and diastolic blood pressure were moderately correlated (r=0.53), after the replacement of one outlier with the mean, and kept as seperate variables. The 32 body composition measures were strongly correlated (up to r=1.0). To reduce their dimensions we first dropped the most highly correlated variables (r>0.97) and later included the rest of the variables in a principal component analysis (PCA). The body composition variables were also moderately to strongly correlated with standing height (~r=0.7), sitting height and average grip strength (~r=0.6), so these variables were also included in the body composition PCA. All numerical variables were standardized using MinMax scaling.

Forward Linear Regression, Regression (Decision) tree, Random Forest, Gradient boosting tree and Support Vector Regression were used to predict body composition and blood pressure. Deep learning models were not included due to the limited sample size. Features were selected using individual correlations. All analyses were performed in Jupyter notebook 6.4.8 using Python 3.9.12 (main, Apr  4 2022, 05:22:27) [MSC v.1916 64 bit (AMD64)].

## Results
(Continue here)

The first two body composition principal components (PCs) explained 87.1% of the variance, while a third PC only explained an additional 3.3%. Only the first two PCs were therefore included in downstream analyses. PC1 can be referred to as the fat component, with higher values on this component indicating body types that are higher in body fat & metabolic age and lower in strength, muscle and height. PC2 can be referred to as the muscular component with higher values indicating more muscle, height, and a stronger grip, and lower in body fat.

I also examined whether PCA would be a sensible approach for the food frequency data, but even four PCs only explained 53% of the data. I therefore rather used the food frequency indices in subsequent analyses.

#### Feature selection and Prediction
I implemented several models to predict body composition and blood pressure: Forward Linear Regression, Regression (Decision) tree, Random Forest, Gradient boosting tree and Support Vector Regression. I did not include deep learning models due to the limited sample size. Features were selected using individual correlations and visualised with scatter and boxplots. 







