# nutrition-project

## Title: Is genetic-based, personalised nutrition a viable strategy to improve health in South Africa?

### Brief Introduction
Personalised nutrition is a promising emerging research field that could substantially improve public health. Whereas dietary advice generally follows a one-size-fits-all approach, personalised nutrition aims to improve individuals’ health with a diet tailored to their individual biology, especially their genetics. Despite the relative immaturity of the field, commercial companies are already offering DNA diet packages to improve dietary recommendations based on genetic testing.

### Aim
The aim of this study is to evaluate whether genetic-based, personalised nutrition could be a viable option to improve health in South Africa.

### Objectives
* Identify the most promising genetic variants associated with diet-associated health outcomes.
* Determine the prevalence of these genetic variants in African and South African populations.
* Determine the association between these genetic variants, diet and diet-associated health outcomes.

### Methods
#### Data
The African Longitudinal Facial Appearance and Health Study (ALFAH) study was approved by research ethics committees at the Faculties of Health Sciences (259/2016), Natural and Agricultural Sciences (EC160429-024) and Humanities (25309995/ HUM030/0819) at the University of Pretoria, South Africa. Two hundred and seventy-one (N=271) African participants (53.3% male) aged 18 to 29 (mean: 20.79) were recruited for the study from the University of Pretoria and the Sefako Makgatho University, Pretoria, South Africa. Written informed sent was obtained from all participants. Participants were asked to complete a questionnaire, including questions on basic demographic variables, their health, exercise and diet (in the past 7 days). Each participant's body weight, percentage body fat and body composition were measured with a Tanita MC 780 MA Segmental Body Composition Analyser; their blood pressure with an automated blood pressure monitor; their grip strength with a JAMAR Hydraulic Hand Dynamometer; and their standing and sitting height with a wall-mounted stadiometer. 

Two saliva samples were collected from each participant and DNA was extracted using the PSP SalivaGene system (Stratec molecular, Berlin Germany) following the manufacturer's instructions. DNA samples of 144 participants were genotyped at the Centre for Proteomic and Genomic Research (CPGR), South Africa, and DNA samples of a further 92 participants were genotyped at the Uppsala University Biomedical Centre, Sweden. Both facilities used the Illumina Infinium H3Africa v1 Assay. Only one sample had an SNP call rate <0.95. Participant data was distributed across three datasets: the main data set (containing questionnaire data, body composition, blood pressure, grip strength, standing and sitting height), genotyping dataset 1 (CPGR results) and genotyping dataset 2 (Uppsala University results). All datasets were uploaded to the mySQL nutrition database (nutrition-load-main-from-csv). Detailed descriptions of the datasets are provided in "nutrition-dataset-description"

#### Project structure
My initial plan is to analyze the data using k-means clustering and linear regression.

#### Data wrangling
Feature engineering was performed in jupyter noteboook using Python 3.

*Main data set* 
<br> The data wrangling steps are detailed in nutrition-data-wrangling1.ipynb

*Missing values:* Initially all variables (attributes), except "id" had one or more missing values. One row (observation/ instance) had no values, except "id", so it was dropped (df.dropna) and the index reset (df.reset_index). Eleven variables were also dropped (df.drop) because they were not important for the analyses. Forty-six (70%) of the remaining variables had one or more missing variables. Missing values were (a) replaced by the mean in forty-two numerical variables and (b) replaced by the most common value in five categorical variables. 

*Dimension reduction:* The strenuous and moderate exercise variables were combined into a single weighted "exercise" variable (df['exercise']=df['exercise_mod']+(2*df['exercise_stren'])). The 14 food frequency (i.e. diet) variables were reduced to five food indexes ('fruitveg_index', 'carbs_index', 'dairy', 'protein_index', 'junkfood_index') and average grip strength ('avg_grip') calculated from the left and right grip values. Seventeen highly correlated (r>0.95) bio-impedance measures were dropped from the dataset.

*Outliers and distribution*  The appropriate method to deal with outliers and distribution is a hotly debated topic. Recommendations vary widely. I therefore decided to conduct the analyses using the (a) minimally transformed, and (b) fully transformed variables to compare the results for myself. Outliers and distributions were evaluated using df.hist, df.skew, the IQR method (df.quantile; Q1-1.5*IQR/ Q3+1.5*IQR) and the Standard deviation (std) method (df.std; mean +- 3std). A variable is considered normally distributed if df.skew is between -1 and 1. The std method of outlier classification is preferred if the distribution is normal, while the IQR method is preferred if the distribution is not normal. 
 * (A) If an outlier is present, but the value is not possible (for instance, systolic blood pressure of 640) the outlier is treated as a missing variable and replaced by mean. Three such outliers were replaced by the mean.
 * (B) If outliers are present (and possible) and the distribution positively skewed, the variable is log-transformed or log-transformed and then capped, depending on what is needed to produce a normal distribution with no major outliers. Outliers were capped at Q1-1.5*IQR or Q3+1.5*IQR if the log distribution was skewed and capped at mean +- 3std if the log distribution was normal. Three variables were log-transformed and four variables were log-transformed and then capped.
 * (C) If outliers are present (and possible) but the distribution normal, the variables were capped at mean +- 3std. Nine variables were capped.
Minimally transformed variables included (A) transformed variables, while fully transformed variables included (A), (B) and (C) transformed variables.

Status: In progress, awaiting the SNP candidate list before further analysis.



