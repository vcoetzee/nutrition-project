# nutrition-project

## Title: Is genetic-based, personalised nutrition a viable strategy to improve health in South Africa?

### Brief Introduction
Personalised nutrition is a promising emerging research field that could substantially improve public health. Whereas dietary advice generally follows a one-size-fits-all approach, personalised nutrition aims to improve individuals’ health with a diet tailored to their individual biology, especially their genetics. Despite the relative immaturity of the field, commercial companies are already offering DNA diet packages to improve dietary recommendations based on genetic testing.

### Aim
The aim of this study is to evaluate whether genetic-based, personalised nutrition could be a viable option to improve health in South Africa.

### Objectives
* Identify the most promising genetic variants associated with diet associated health outcomes.
* Determine the prevalence of these genetic variants in African and South African populations.
* Determine the association between these genetic variants, diet and diet associated health outcomes.

### Methods
*Data*
<br>The African Longitudinal Facial Appearance and Health Study (ALFAH) study was approved by research ethics committees at the Faculties of Health Sciences (259/2016), Natural and Agricultural Sciences (EC160429-024) and Humanities (25309995/ HUM030/0819) at the University of Pretoria, South Africa. Two hundred and seventy-one (N=271) African participants (53.3% male) aged 18 to 29 (mean: 20.79) were recruited for the study from the University of Pretoria and the Sefako Makgatho University, Pretoria, South Africa. Written informed sent was obtained from all participants. Participants were asked to complete a questionnaire, including questions on basic demographic variables, their health, exercise and diet (in the past 7 days). Each participant's body weight, percentage body fat and body composition were measured with a Tanita MC 780 MA Segmental Body Composition Analyser; their blood pressure with an automated blood pressure monitor; their grip strength with a JAMAR Hydraulic Hand Dynamometer; and their standing and sitting height with a wall-mounted stadiometer. 

Two saliva samples were collected from each participant and DNA was extracted using the PSP SalivaGene system (Stratec molecular, Berlin Germany) following the manufacturer's instructions. DNA samples of 144 participants were genotyped at the Centre for Proteomic and Genomic Research (CPGR), South Africa, and DNA samples of a further 92 participants were genotyped at the Uppsala University Biomedical Centre, Sweden. Both facilities used the Illumina Infinium H3Africa v1 Assay. Only one sample had an SNP call rate <0.95. Participant data was distributed across three datasets: the main data set (containing questionnaire data, body composition, blood pressure, grip strength, standing and sitting height), genotyping dataset 1 (CPGR results) and genotyping dataset 2 (Uppsala University results). All datasets were uploaded to the mySQL nutrition database (nutrition-load-main-from-csv). Detailed descriptions of the datasets are provided in "nutrition-dataset-description"

*Project structure*
<br>

*Feature Engineering*
<br> *Feature engineering* was performed in jupyter noteboook using Python 3.
    <br>*Main data set* (nutrition-data-wrangling1)
    <br>*Missing values:* Initially all variables, except "id" had one or more missing values. One row had no values, except "id". I dropped the row (df.dropna) and reset the index (df.reset_index). Eleven variables were also dropped (df.drop) because they were not important for the analyses. Forty-six (70%) of the remaining variables had one or more missing variables. Missing values were (a) replaced by the mean in forty-two numerical variables and (b) replaced by the most common value in four categorical variables. 
  <br>*Dimension reduction*
  <br>*Data transformation:* Outliers and distributions were evaluated using df.hist, df.skew, the IQR method (df.quantile; Q1-1.5*IQR/ Q3+1.5*IQR) and the Standard deviation (std) method (df.std; +- 3std). A variable is considered normally distributed if df.skew is between -1 and 1. The std method of outlier classification is preferred if the distribution is normal, while the IQR method is preferred if the distribution is not normal. 
  * Where the outlier is not possible (for instance, age of 405) the outlier is treated as a missing variable and replaced by mean. One outlier in avg_systbp, avg_diabp and avg_pulse was replaced by the mean.
  * Where outliers are present (and possible) and the distribution is positively skewed I first try log-transform



Status: In progress, awaiting the SNP candidate list before further analysis.



