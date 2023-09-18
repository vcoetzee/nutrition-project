# Title: Predicting body composition and blood pressure in a South African student population 
## Brief Introduction
Most research is conducted in WEIRD (Western, Educated, Industrialised, Rich, and Democratic) countries. Less than 1% of the world's research is conducted in Africa. In this study we wanted to explore which demographic and/or lifestyle factors predict aspects of health, specifically body composition and blood pressure, in a Black South African student population. Once these factors are better understood, they can be used to improve health and identify risk factors in this population. The aim of this study is therefore to determine which demographic and/or lifestyle factors predict body composition and blood pressure in a South African student population.

## Methods
### Data
The African Longitudinal Facial Appearance and Health Study (ALFAH) study was approved by research ethics committees at the Faculties of Health Sciences (259/2016), Natural and Agricultural Sciences (EC160429-024) and Humanities (25309995/ HUM030/0819) at the University of Pretoria, South Africa. Two hundred and seventy-one (N=271) Black African participants (53.3% male) were recruited for the study from the University of Pretoria and the Sefako Makgatho University, Pretoria, South Africa. Written informed sent was obtained from all participants. Participants were asked to complete a questionnaire, including questions on basic demographic variables, their health, exercise and diet (in the past 7 days). Each participant's body composition (i.e. body weight, percentage body fat, muscle mass etc.) were measured with a Tanita MC 780 MA Segmental Body Composition Analyser; their standing and sitting height with a wall-mounted stadiometer; and their blood pressure with an automated blood pressure monitor. Two blood pressure measurements were taken with participants in a sitting position after a 5min rest period. The mean of the two blood pressure readings was used. The data was stored in a MySQL database (Server version: 8.0.33 MySQL Community Server - GPL).

### Data analyses
The number of missing values were calculated. Four columns and one row with more than 10% missing data (and other unnecesary colums) were dropped from the dataframe. Missing values in 41 numerical variables (range 0.4-6.7% missing) were replaced by the mean and missing values in two categorical variables (1.1-3.0% missing) were replaced by the most common category. The data format was corrected for all remaining variables and the data visualised. The two exercise variables were combined into a single, weighted exercise variable (exercise = moderate exercise + (2 * strenious exercise)), and the 14 food frequency variables were combined into four food indixes: fruit & veg index (fruit juice, fruit and vegetables), carbs index (bread, pap and samp, rice and pasta), dairy index (dairy) and protein index (red meat, chicken, pork, fish and eggs). There were insufficient data for a 'fat' index, but sufficient data to add a junkfood index (take aways and soft drinks). Two standing height variables that were swapped with sitting height, were corrected. Systolic and diastolic blood pressure were kept as seperate variables. The 32 body composition measures were strongly correlated (up to r=1.0). To reduce their dimensions we first dropped the most highly correlated variables (r>0.97) and later included the rest of the variables in a principal component analysis (PCA). The body composition variables were also moderately to strongly correlated with standing height (~r=0.7), sitting height and average grip strength (~r=0.6), so these variables were also included in the body composition PCA. All numerical variables were standardized using MinMax scaling.

Forward Linear Regression, Regression (Decision) tree, Random Forest, Gradient boosting tree and Support Vector Regression were used to predict body composition and blood pressure. Deep learning models were not included due to the limited sample size. The dataset split into training and testing sets using a 80:20 split. Features were selected for forward linear regression based on the strongest individual correlations and the variance inflation factor (VIF) calculated to detect multicolinearity. Regression trees were pruned based on 5 fold cross validated R2 scores. Random forests estimated using a 1000 estimators.  All analyses were performed in Jupyter notebook 6.4.8 using Python 3.9.12 (main, Apr  4 2022, 05:22:27) [MSC v.1916 64 bit (AMD64)].

## Results
Participants were aged 18 to 29 (mean = 20.8; stdev= 1.9)), their BMI's ranging from 15.0 kg/m2 to 42.7 kg/m2 (mean = 23.3; stdev = 4.7), percentage body fat ranging from 3.2% to 49.1% (mean = 23.6; stdev = 10.0) and muscle mass from 30.0 to 73.5kg (mean = 46.7; stdev = 8.5). Their systolic blood pressure ranged from 62.0 to 159.0 (mean = 119.4; stdev = 13.0) and their diastolic blood pressure from 36.5 to 107.5 (mean = 74.6; stdev = 8.93). Age and BMI were very similar between the sexes, but the women had a higher percentage body fat, while the men had a higher muscle mass (Table 1). Men had a slightly higher systolic blood pressure on average, while women had a slightly higher diastolic blood pressure on average (Table 1).

Table 1. Descriptive statistics
| Descriptive | # Women   | # Men   |
| :---:   | :---: | :---: |
| Age | 20.8 (1.9)   | 20.8 (1.9)  |
| bmi | 23.6 (5.1)   | 22.9 (4.4)  |
| body fat | 26.9 (9.3)   | 20.8 (9.6)  |
| muscle mass | 43.6 (8.1)   | 49.4 (7.8)  |
| systolic bp | 118.5 (12.7)   | 120.3 (13.2)  |
| dia bp | 75.6 (9.1)   | 73.8 (8.7)  |

The first two body composition principal components (PCs) explained 86.7% of the variance, while a third PC only explained an additional 3.3%. Only the first two PCs were therefore included in downstream analyses. PC1 can be referred to as the fat component, with higher values on this component indicating body types that are higher in body fat & metabolic age and lower in strength, muscle and height. PC2 can be referred to as the muscular component with higher values indicating more muscle, height, and a stronger grip, and lower in body fat. I also examined whether PCA would be a sensible approach for the food frequency data, but even four PCs only explained 53.8% of the data. The food frequency indices were therefore used in subsequent analyses.

### Predicting Body composition:
PC1 (the fat component) was most strongly correlated with sex (r=-0.27), exercise_scaled (r=-0.19) and fruitveg_index_scaled (r=0.10). All the regression models predicted PC1 very poorly. The linear regression and regression tree models produced the highest R2 and lowest MSE (R2=0.04; MSE=0.35). In both cases sex was the only explanatory variable, with women having a higher fat component than men.

PC2 (the muscular component) was most strongly correlated with sex (r=0.32), age_scaled (r=0.17), income (r=0.14) and protein_index_scaled (r=0.12). PC2 was predicted somewhat better than PC1, with the linear regression model producing the highest R2 and lowest MSE (R2=0.24; MSE=0.14). The final linear regression model included sex, age_scaled, income, protein_index_scaled, dairy_scaled and fruitveg_index_scaled. Men had a higher muscular component than women. In addition, older, richer people who consumed more protein, dairy and fruit and vegetables had a higher muscular component. All VIFs were in the acceptable range.

Overall, the regression models did not predict PC1 and PC2 very well. I therefore determined whether individual body composition measures would not be predicted better. The three most useful individual body composition measures from a theoretical perspective are body fat, visceral fat and muscle mass. Of the three muscle mass was the most promising, showing stronger correlations with a variety of directly observable features (sex, standing height etc) and systolic blood pressure. 

Muscle mass was most strong correlated with standing height (r=0.72), sex (r=0.34), income (r=0.14), age_scaled (r=0.14), protein_index_scaled (r=0.13), exercised_scaled (r=0.12) and dairy_scaled (r= 0.10). Linear regression models produced the highest R2 and lowest MSE values. The step 7 linear regression model (standing_height_scaled, sex, income, age_scaled, protein_index_scaled, exercise_scaled, dairy_scaled) produced very similar, but slightly higher R2 and lower MSE (R2=0.63; MSE=20.29) than the step 4 linear regression model (standing_height_scaled, sex, income, age_scaled; R2=0.63; MSE=20.32). The step 4 model is simpler and the variables more easily obtainable. Men had a higher muscle mass than women. People with a higher muscle mass were also generally taller, older, had a higher income, and consumed relatively more protein, dairy and fruit and vegetables.  All VIFs were in the acceptable range.

Body fat was most strongly correlated with standing_height_scaled (r=-0.44), sex (r=-0.31) and exercise_scaled (r=-0.19). The linear regression model produced the highest R2 and lowest MSE values (R2=0.21; MSE=70.47). The final linear regression model included standing_height_scaled and sex. Women had a higher body fat than men, and shorter people had a higher body fat than taller people. All VIFs were in the acceptable range. All the regression models explained very little variance in visceral fat (Best model R2=0.07; MSE=2.57).

### Predicting Blood Pressure
Systolic blood pressure was most strongly correlated with muscle_mass_scaled (r=0.51), dairy_scaled (r=0.15) and alcohol (r=0.10). The linear regression model produced the highest R2 and lowest MSE (R2=0.26; MSE=91.14). The final linear regression model included muscle_mass_scaled, dairy_scaled, alcohol and junkfood_index_scaled. More muscular people who consumed more dairy, alcohol and junkfood had a higher systolic blood pressure. The removal of one multivariate outlier did not improve the model further. All regression models explained very little variance in diastolic blood pressure (Best model R2=0.07; MSE=86.23)

## Discussion

The aim of this study was to determine which demographic and/or lifestyle factors predict body composition and blood pressure in a South African student population. I evaluated different measures of body composition, ranging from two principal components (PC1: fat component; PC2: Muscle component) to muscle mass, body fat and visceral fat. The muscle mass linear regression explained the most variance, explaining up to 63% of the variance in muscle mass. Height, sex and age played major roles in predicting muscle mass. Men generally have a higher muscle mass than women, and taller, older people also tend to have a higher muscle mass. Modifiable diet/ lifestyle factors played less of a role in muscle mass, but increased wealth tended to increase muscle mass, as did increased protein consumption, dairy consumption and exercise. The PC2 (muscle component) regression models also explained more variance than the PC1 (fat component) regression models, explaining up to 24% of the variance in the muscle component. Sex and age again played major roles in predicting the muscle component, but modifiable factors such as increased wealth, protein, dairy and fruit and vegetable consumption also tended to increase the muscle component. Wealthier people tend to have better access to expensive food types, such as protein and dairy.

Interestingly, muscle mass (and not percentage body fat) was the main predictor for systolic blood pressure. More muscular students also had higher systolic blood pressure. Students with a higher dairy, alcohol and junkfood consumption had higher systolic blood pressure. Our findings are consistent with previous research showing that exercise, modest alcohol intake and a DASH (Dietary Approaches to Stop Hypertension) diet are low-risk factors for hypertension (Singh et al, 2006; Forman, Stampfer and Curhan, 2009). The DASH diet, consists of high intake of fruits, vegetables, nuts and legumes, low-fat dairy products, and whole grains, and low intake of sodium, sweetened beverages, and red and processed meats (Singh et al, 2006; Forman et al. 2009). Indeed, high dairy, alcohol and junkfood consumption were directly associated with higher systolic blood pressure in our study. While previous studies mostly focus on increased BMI and obesity as risk factors for hypertension (e.g. Ford, 1991; Singh et al, 2006; Forman et al. 2009), our study found a stronger association between muscle mass and systolic blood pressure, than between body fat and systolic blood pressure in South African students. Future studies should examine this relationship in a larger cohort.

## References
Ford, E.S. and Cooper, R.S., 1991. Risk factors for hypertension in a national cohort study. Hypertension, 18(5), pp.598-606.
Forman, J.P., Stampfer, M.J. and Curhan, G.C., 2009. Diet and lifestyle risk factors associated with incident hypertension in women. Jama, 302(4), pp.401-411.
Singh, A.K., Maheshwari, A., Sharma, N. and Anand, K., 2006. Lifestyle associated risk factors in adolescents. The Indian Journal of Pediatrics, 73, pp.901-906.








