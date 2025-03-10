# Identifying Key Predictors of High-risk Drug-Overdose Mortality: A Machine Learning Analysis of U.S. Counties

# Abstract
This study employs machine learning methods to identify county-level predictors of drug overdose mortality risk in the United States. 
Using probit regression, LASSO, random forest, and XGBoost models, we analyze the relationship between county characteristics and 
overdose mortality, focusing on health-related and healthcare access, community safety, and sociodemographic factors in the years 
2016- 2019. Random Forest and XGBoost models outperformed traditional approaches in prediction accuracy. Our findings highlight 
frequent mental distress percentage as a critical factor, while healthcare provider metrics showed mixed effects. Demographic 
analysis identifies non-Hispanic white percentage as the strongest predictor, with rural areas showing increased risk. 
Economic indicators, including unemployment and income inequality, consistently correlate with higher risk. Notably, violent crime 
hows minimal association. These insights inform targeted public health interventions at the county level.

# Introduction
Drug overdose mortality continues to escalate in the United States, remaining to be one of the leading causes of injury mortality 
(Cerdá et al., 2023). The epidemic has evolved through waves of different substances, with synthetic opioids now driving an unprecedented
surge in mortality rates. This crisis stems from multiple factors: the historical over-prescription of opioids for pain management, the 
proliferation of low-cost high-potency synthetic drugs, and the increasing accessibility of illicit substances through online markets 
(WHO, 2023b). Monitoring drug-overdose mortality becomes more challenging with the rapidly changing nature of the illicit drug supply 
environment. Collecting drug overdose data has always been in “hindsight”, using traditional surveillance systems that rely on data 
from hospital discharge and emergency medical services data (Schell et al., 2021). Being able to forecast drug overdose rates, even if 
it is for the short term future, and being able to identify key predictors that could point to a high-risk of overdose mortality, can 
be crucial in directing limited public health resources to vulnerable communities. This is particularly important given the heterogeneous nature of the United States, where demographic, socioeconomic, and healthcare access patterns vary substantially across regions. 
County-level analysis provides a lens for understanding these patterns, offering granular insights that can inform targeted interventions while capturing the broader community characteristics that influence substance use risks.Machine learning approaches, while still nascent in drug overdose research, offer powerful tools for detecting complex patterns and  interactions among risk factors. Recent studies, particularly those using individual-level data, have demonstrated the power of integrating socioeconomic and behavioral factors into predictive models. However, the application of these techniques to understand county-level patterns of drug overdose risk remains largely unexplored. This study employs four supervised machine learning approaches to identify key county characteristics associated with elevated drug overdose mortality risk across US counties, aiming to provide actionable insights for public health planning and intervention.

# Literature Review
There are many studies that looked at individual-level risk factors of high risk of drug overdose, abuse and disorders. Lowder et al. 
(2020) used emergency department records to identify individual-level variability in risk factors, and found non-hispanic white males 
in older ages (65-84 years) had a higher risk of overdose mortality. Other studies find factors associated with social support including family history, lack of family involvement, peer pressure, and loneliness as risk factors of drug addiction (Frankenfeld & Leslie, 2019).Regular smokers, not graduated high school, and unmarried individuals are found to be at an increased risk of prescription opioid-overdose (Lanier et al., 2012). Saloner et al.’s  (2020) study included behavioural health and criminal justice variables to their model and found it only marginally improved their predictive model performance for fatal and non-fatal overdose. 

Many of these studies use traditional statistical methods to determine causality. For instance, Pike et al. (2020) used a multivariate 
logistic regression model to study the relationship between drug-use and subsequent nonprescription opioid use among high-school students. Similarly,  Saloner et al. (2020) used separate logistic models to predict risk of fatal and nonfatal opioid overdose using clinical and criminal justice data. Geissert et al. (2017) also used logistic models and found model performance of the simpler model, based on receiver operating characteristics, had better predictive power.
Most of these studies have been conducted in unique individual-level populations, making it difficult to generalize to the US population. Frankenfeld & Leslie (2019) argue that county-level analysis is important to study drug-overdose risks as it captures how economic conditions,and social structure influence health outcomes. Studies examining county-level relationships use spatial and non-spatial approaches. Acharya et al. (2022) studied spatio-temporal patterns in opioid overdose rates and used dynamic time warping to identify temporal patterns between counties and multilevel modelling incorporating socio-economic factors. Other spatial studies used censored Poisson regression and negative binomial regression to account for spatial autocorrelation by modelling spatial coordinates of the counties (Haffajee et al. ,2019; Marks et al., 2021; Bozorgi et al.,2021; Frankenfeld and Leslie 2019; Fink et al. 2023). Haffajee et al. (2019)used a spatial logistic model to identify high-risk opioid-overdose counties with low opioid use disorder treatment, and found low primary care provider density, and high rates of unemployment to be top predictors of high-risk. 

Use of machine-learning techniques in identifying high-risk drug-overdose has been mostly used in individual-level risk stratification 
studies. For instance, Lo-Ciganic et al. (2022) used multivariate logistic regression, penalized regression, random forest and 
gradient-boost techniques to predict opioid overdose in Medicare beneficiaries. They identified gradient-boost to be the best predictor,
based on C-statistic and precision-recall curves, similar to other studies (Lo-Ciganic et al. 2019; Lo-Ciganic et al. 2021).  One of 
very few studies that used machine learning techniques using more generalizable data, Bozorgi et al.,(2021) used linear regression,
sequential minimal optimization, random forest and extra gradient boosting to identify neighborhood-level predictors of drug-overdose
and found that the XGBoost ensembled model of socio-demographic, drug-related and protective resources outperformed all other models,
explaining 73% of variation. 

While these studies have advanced our understanding of drug overdose risk factors and prediction, several gaps remain. First, 
most machine learning applications have focused on individual-level risk prediction, with limited exploration of county-level patterns. 
Second, studies examining county-level relationships have primarily employed spatial statistical approaches, potentially missing 
complex non-linear relationships between county characteristics and overdose risk. Third, existing research has often focused on
specific geographical regions or populations, limiting generalizability. Our study addresses these gaps by employing multiple 
machine learning approaches to identify county-level characteristics associated with drug overdose mortality risk across the 
United States. We combine traditional statistical methods probit and LASSO with ensemble learning techniques Random Forest and 
XGBoost to analyze a comprehensive set of county characteristics from 2016-2019.

# Methodology
This study analyzes county-level data (2016-2019) from the County Health Rankings & Roadmaps program to identify predictors of drug overdose mortality in the United States. The dataset consolidates information across three main categories: health-related factors (mental distress, uninsured rates, provider density, smoking, drinking), community safety (violent crime rates), and socioeconomic vulnerability (demographics, income, unemployment, education, housing). The final dataset comprises 12,563 observations across 1,856 counties and 23 variables over the four-year period. Counties in the 75th percentile of drug overdose mortality rates, derived from CDC Wonder data, were classified as high-risk.

## Missing Data Strategy
A significant challenge was handling missing values in the drug overdose mortality data, which followed CDC's suppression policy for counts fewer than 10, creating a Missing Not at Random (MNAR) pattern. To address this, we employed a two-step imputation approach: first using multiple imputation with predictive mean matching (PMM) for independent variables, then applying k-Nearest Neighbor (k=5) for the dependent variable. All numeric variables were standardized using z-score transformation, with high-skew variables undergoing log transformation to reduce right skew.

## Models
We implemented four supervised learning methods to identify key predictors of high-risk counties. First, a probit regression model served as our baseline due to its interpretable coefficients, with diagnostic tests including residual plots and VIF analysis (mental distress showed highest VIF at 6.41). Second, we employed LASSO regularization for feature selection, using the more conservative lambda.1se for parsimony (AUC = 0.6557), which eliminated household income, dentist rate, and violent crime variables. Third, Random Forest (200 trees, max leaf nodes = 20, min node size = 10) captured non-linear relationships missed by linear models. Finally, XGBoost with sequential tree building (eta = 0.1, max depth = 5) was optimized via 5-fold cross-validation with early stopping.
For evaluation, we split the data 70:30 for training and testing, with stratification by the binary risk outcome. To address the class imbalance (3:1 ratio of low-risk to high-risk counties), we applied upsampling to the training set after comparing it with SMOTE and finding better generalization. Our evaluation metrics prioritized sensitivity (ability to correctly identify high-risk counties) and precision (reliability of identified characteristics), complemented by ROC curves and F1-scores for cross-model comparison.

# Results
## Descriptive Statistics
Table 1. Descriptive Statistics 
![Descriptive Statistics](https://github.com/user-attachments/assets/e393f672-f59f-4546-8223-0cecc2c92856)

Drug overdose mortality varies substantially across counties (mean: 19.3 deaths per 100,000; max: 111.5). Mental distress affects an average of 11.8% of adults (range: 6.6-22.2%). Healthcare resources show dramatic disparities - Mental Health Providers range from 0 to over 2,000 per 100,000 population (mean: 136), with similar patterns for PCPs (mean: 550) and dentists (mean: 439). Insurance coverage varies widely, with uninsured rates averaging 16.5% but reaching 51.3% in some counties. Demographics reveal an average 76.7% Non-Hispanic White population (range: 2.8-98.6%) and 58.6% rural population (range: 0-100%). Economic indicators show household incomes from $21,658 to $136,191 (mean: $49,081) with an average income inequality ratio of 4.5. Unemployment averages 5.4%, reaching 24% in hardest-hit areas. Correlation analysis shows healthcare providers cluster together and correlate with household income. Drug overdose mortality correlates positively with smoking (r=0.33) and mental distress (r=0.37), but negatively with excessive drinking (r=-0.28).
Geographically, West Virginia consistently had the highest mortality rates, increasing from 34.4 to 42.1 deaths per 100,000 from 2016-2019. Extreme concentrations appeared in specific counties, with Cabell County, WV, reaching 112.0 deaths per 100,000 by 2019, highlighting significant within-state variations.

## Prediction Performance
Table 2. Model Comparison using Performance Metrics 
![Model Comparison using Performance Metrics](https://github.com/user-attachments/assets/8b814c09-1b2f-4582-b3ae-e3e336a8f060)

Our analysis employed four different models to identify county characteristics associated with high drug overdose mortality risk. The probit and LASSO models demonstrate notably consistent performance between training and testing sets, with accuracy hovering around 0.66 for both. The Random Forest model shows stronger base performance while maintaining reasonable consistency between training and testing. Its training accuracy of 0.722 decreases modestly to 0.707 on the test set, indicating good generalization. The model's sensitivity of 0.719 on training data carries over well to the test set at 0.719, suggesting reliable identification of high-risk counties. XGBoost demonstrates the highest training performance with 0.886 accuracy and particularly strong specificity (0.911). However, it shows the largest gap between training and testing performance, with test accuracy dropping to 0.769. Despite this decrease, it maintains strong sensitivity (0.805) and precision (0.878) on the test set, suggesting robust predictive power for identifying high-risk counties.Notably, all models show relatively strong precision on the test set (ranging from 0.851 to 0.878), indicating reliable positive predictions when counties are classified as high-risk. The ensemble methods (Random Forest and XGBoost) achieve higher F1-scores on the test set compared to the linear models, suggesting better overall balance between precision and sensitivity in their predictions.The ROC curves (see Figure 1) illustrate the superior discriminative ability of the ensemble methods compared to the linear models. We can see that the ROC curves of the linear models are overlapping, while we see a clear dominance of the XGBoost model.

Figure 1. Comparison of Receiving Operating Characteristic Curves (Testing)
![ Comparison of Receiving Operating Characteristic Curves](https://github.com/user-attachments/assets/3d7f10fc-e1aa-4e70-b282-8b1fc0a2f8a2)
## Top Predictors identified by the Models
All models demonstrated consistent performance in identifying key county characteristics associated with high drug overdose mortality risk. Examining results (See Appendix for model results) across all models reveals consistent patterns of important predictors within each category. In the health-related and healthcare access domain, frequent mental distress emerges as a strong predictor, ranking among the top three important variables in all models and showing a significant positive association in the probit model (AME = 0.063, p < 0.001). Healthcare provider accessibility demonstrated mixed effects across all models: mental health provider rates consistently showed positive associations with risk (featured prominently in RF and XGBoost importance rankings), while primary care physician rates showed inverse relationships. Both LASSO and RF identified dentist rates as having minimal importance, statistical insignificance in the probit model, and dropped by the LASSO model. Interestingly, excessive drinking demonstrates a consistent negative association. Uninsured population percentage is positively associated with risk.

In the socio-demographic domain, non-Hispanic white population percentage shows the strongest association across all models for high-risk of drug overdose, ranking first in importance for RF and XGBoost and showing the largest marginal effect in the probit model (AME = 0.104, p < 0.001). Rural population percentage is also associated positively with risk, while female population percentage shows a protective negative effect. In terms of economic factors, unemployment rate appeared among the top predictors in all models' importance rankings and showed significant positive association in the probit model (AME = 0.038, p < 0.001) , and income ratio similarly demonstrated consistent positive relationships across all models. Population was also among the top predictors in all models, showing a positive relationship with high-risk (AME= 0.053, p< 0.001). 

In the community safety domain, violent crime rate showed a consistently low and insignificant association with high-risk drug overdose. Notably, all models indicated a significant temporal trend, with risk of drug overdose mortality increasing over the study period (2016-2019). This temporal effect ranked among the top predictors in importance across models.

#  Discussion
In recent years, the risk of drug-overdose mortality rates in the U.S has been escalating. We see a clear temporal trend– from 2016, the national average of drug overdose mortality increased from 17.4 deaths per 100,000 to 21.0 in 2019. It is evident that the crisis is getting worse with time. This growth is largely driven by synthetic opioids, the latest wave of drug overdose mortality. Although the opioid crisis was officially announced in 2017, the third wave of the opioid crisis started growing in 2013 with the emergence of synthetic opioids (Zhao et al., 2024). Ciccarone (2019) suggests that this growth is an outcome of the combined forces of demand (social, cultural and technological elements) and supply (new illicit sources of drugs).  

We identified non-hispanic white population percentage as a top predictor of a high-risk drug-overdose county. This aligns with CDC's 2017 findings that counties with higher non-Hispanic white populations have higher opioid prescription rates. The relationship likely reflects documented disparities in prescribing practices: despite evidence that racial and ethnic minorities experience similar or higher pain levels, non-Hispanic white patients receive more frequent prescriptions, higher doses, and longer duration of opioid medications (Singhal et al., 2016; Alexander et al., 2018). These prescribing patterns may contribute to increased overdose risk in predominantly white communities.

Frequent mental distress percentage being a high predictor of high-risk drug overdose aligns with recent research on poor mental health status being inherently linked to substance abuse disorders (SUD) and drug overdose risk, which is a sequence of SUDs (Kedia et al., 2022). Research by Foley and Schwab-Reese (2018) find a relationship between depression and associated increase in fatal opioid-overdose deaths in the US. Gicquelais et al. (2020) find similar trends finding links between suicidal ideation associated with higher risk of opioid overdose hospitalization. SUDs and drug-overdose rates are inextricably linked with income inequality, and poverty, with lower-income communities being less likely to have access to healthcare resources and access to treatment. We find that counties with higher uninsured populations have associated high-risk for drug-overdose, a finding in line with research (Wu & Ringwalt, 2005).

Our study finds an inverse relationship between PCPs and drug-overdose risk, in line with research by Haffajee et al., (2019) finding that opioid high-risk counties are characterised by lower proportions of PCPs and MHPs . Our finding that the density of MHPs have a positive relationship with drug-overdose warrants careful consideration. We argue that the positive association doesn't imply that mental health providers contribute to drug overdose risk, but rather might reflect complex patterns of healthcare resource allocation and underlying community needs. For instance, increased mental health providers density could be in response to counties having higher drug-overdose cases. Moreover, counties with higher density of MHPs would mean the county has better detection and reporting of drug-overdose cases. Ruhm (2017) finds that one fourth of all drug-overdose death certificates fail to note the specific drug that caused the fatality, and this underreporting is found substantially in counties with poorer medical infrastructure. 

There are also clear geographic variations that are evident. West Virginia consistently dominates the list of highest-risk counties, with rural counties like McDowell, Wyoming, and Boone repeatedly appearing among the top rates. By 2019, we also see the emergence of an urban center in the list - Baltimore City, Maryland - suggesting the crisis may be evolving to affect both rural and urban areas more severely. Our analysis finds that countries with that rural percentage is a top predictor of having higher risk of overdose. Studies (King et al., 2014; García et al., 2019) show that rural populations are more likely to have opioid-related mortality and have higher opioid prescription rates. Population, another top predictor of drug-overdose from our analysis, is consistent with other research indicating denser counties having higher drug-overdose rates(Bozorgi et al., 2021). 

Despite studies (Cano et al., 2022; Krawczyk et al., 2019) finding smoking and alcohol consumption to be associated with drug-abuse, we find no significance for percentage of smoking, and an inverse relationship with excessive drinking, suggesting a possible substitution effect. To better understand the inverse relationship between excessive drinking and drug-overdose risk, further exploration is warranted. 

Methodologically, the Random Forest and XGBoost demonstrated superior overall performance compared to the traditional linear models, Probit and LASSO. While previous studies in drug overdose risk prediction have found gradient boosting methods to be optimal (Lo-Ciganic et al., 2022; Lo-Ciganic et al. 2019; Lo-Ciganic et al. 2021), our Random Forest model showed more balanced and robust performance across metrics. Although XGBoost achieved higher accuracy (0.769) and sensitivity (0.805) on the test set, it showed a notable drop in specificity (0.659), suggesting potential overfitting despite its high training performance (accuracy: 0.886). In contrast, the Random Forest model maintained more consistent performance between training and testing sets (accuracy: 0.722 and 0.708 respectively), with balanced sensitivity (0.720) and the highest test set F1-score (0.788) among all models. Random Forest's more balanced sensitivity suggests more reliable overall predictions, because for our specific goal of identifying county characteristics, model stability is crucial. Both Probit and LASSO models showed comparable but lower performance metrics (test accuracy around 0.66), though they offer advantages in interpretability of coefficient estimates. 

# Conclusion
Our analysis reveals several crucial insights for addressing the drug overdose crisis at the county level. Mental health emerges as a critical factor, with the positive association with mental health provider rates indicating that current mental health resources are being allocated reactively rather than preventively. Public health professionals should place increasing importance to strengthen integration of mental health and substance use treatment services, and develop early intervention programs in counties with high mental distress rates. Given primary care services’ protective effect on drug overdose mortality rate risk, increasing access to substance use treatment through primary care settings, with a focus on creating innovative healthcare delivery models for rural counties seem crucial. 

It is important to note that while non-hispanic white population percentage emerges as a key predictor, it reflects historical prescribing patterns, and masks the underlying factors associated with high-risk. Future research should focus on underlying systemic factors rather than racial demographics to better understand and address the root causes of overdose risk. The clear geographic patterns in our findings suggest that incorporating spatial analysis could provide additional insights by examining regional clusters and accounting for spatial autocorrelation.
In terms of machine learning models, random forest emerges as the best model in terms of striking a balance between generalizability, consistency, and interpretability. However, future research could enhance these findings through more extensive hyperparameter optimization and exploration of complex models like Deep Neural Networks. 

# Appendix
Figure A.1 Probit Model Diagnostic Plots
![Probit Model Diagnostic Plots](https://github.com/user-attachments/assets/6eefd7bb-bc9f-473f-86dc-ab7a8c2228f2)

Figure A.1 Probit Model Diagnostic Plots
![Probit Model Diagnostic Plots](https://github.com/user-attachments/assets/8c6b83ef-9759-4a85-9f20-434e29ae4d2d)

Table A.1. Variance Inflation Factor for Probit Model ![ Variance Inflation Factor for Probit Model](https://github.com/user-attachments/assets/22d7a4c4-0acb-41a4-8b40-60694395afd8)

Figure A.2 Correlation Heatmap
![ Correlation Heatmap](https://github.com/user-attachments/assets/1bc06d63-e8b9-4482-9b79-d6aa1189c488)

Table A.2. Probit Results with Average Marginal Effects
![ Probit Results with Average Marginal Effects](https://github.com/user-attachments/assets/0fd587b5-e7e7-4368-8430-1cd1a18b0c3c)

Figure A.3 LASSO Coefficients Plot
![LASSO Coefficients Plot](https://github.com/user-attachments/assets/c9ea7534-5665-48aa-8c27-1ffc232fd372)

Figure A.4 Random Forest’s Error Rate
![Random Forest’s Error Rate](https://github.com/user-attachments/assets/f18783fd-b1b9-4435-9b60-3b6f72c38be6)

Figure A.5 Random Forest’s Feature Importance Rankings
![Random Forest’s Feature Importance Rankings](https://github.com/user-attachments/assets/aa42f27b-8948-4f20-b81e-ff87356f6902)

Figure A.5 XGBoost Feature Importance 
![XGBoost Feature Importance ](https://github.com/user-attachments/assets/cfbdef73-831b-4c02-bf12-0453b84e4377)

# References
References
Acharya, A., Izquierdo, A. M., Gonçalves, S. F., Bates, R. A., Taxman, F. S., Slawski, M. P., Rangwala, H. S., & Sikdar, S. (2022). Exploring county-level spatio-temporal patterns in opioid overdose related emergency department visits. PLoS ONE, 17(12), e0269509. https://doi.org/10.1371/journal.pone.0269509
Bozorgi, P., Porter, D. E., Eberth, J. M., Eidson, J. P., & Karami, A. (2021). The leading neighborhood-level predictors of drug overdose: A mixed machine learning and spatial approach. Drug and Alcohol Dependence, 229, 109143. https://doi.org/10.1016/j.drugalcdep.2021.109143
Cano, M., Oh, S., Osborn, P., Olowolaju, S. A., Sanchez, A., Kim, Y., & Moreno, A. C. (2022). County-level predictors of US drug overdose mortality: A systematic review. Drug and Alcohol Dependence, 242, 109714. https://doi.org/10.1016/j.drugalcdep.2022.109714
Cerdá, M., Krawczyk, N., & Keyes, K. (2023). The future of the United States Overdose crisis: challenges and opportunities. Milbank Quarterly, 101(S1), 478–506. https://doi.org/10.1111/1468-0009.12602
Ciccarone, D. (2019). The triple wave epidemic: Supply and demand drivers of the US opioid overdose crisis. International Journal of Drug Policy, 71, 183–188. https://doi.org/10.1016/j.drugpo.2019.01.010
Fink, D. S., Keyes, K. M., Branas, C., Cerdá, M., Gruenwald, P., & Hasin, D. (2023). Understanding the differential effect of local socio‐economic conditions on the relation between prescription opioid supply and drug overdose deaths in US counties. Addiction, 118(6), 1072–1082. https://doi.org/10.1111/add.16123
Foley, M., & Schwab-Reese, L. M. (2018). Associations of state-level rates of depression and fatal opioid overdose in the United States, 2011–2015. Social Psychiatry and Psychiatric Epidemiology, 54(1), 131–134. https://doi.org/10.1007/s00127-018-1594-y
Frankenfeld, C. L., & Leslie, T. F. (2019). County-level socioeconomic factors and residential racial, Hispanic, poverty, and unemployment segregation associated with drug overdose deaths in the United States, 2013–2017. Annals of Epidemiology, 35, 12–19. https://doi.org/10.1016/j.annepidem.2019.04.009
García, M. C., Heilig, C. M., Lee, S. H., Faul, M., Guy, G., Iademarco, M. F., Hempstead, K., Raymond, D., & Gray, J. (2019). Opioid prescribing rates in nonmetropolitan and metropolitan counties among primary care providers using an Electronic Health Record System — United States, 2014–2017. MMWR Morbidity and Mortality Weekly Report, 68(2), 25–30. https://doi.org/10.15585/mmwr.mm6802a1
Geissert, P., Hallvik, S., Van Otterloo, J., O’Kane, N., Alley, L., Carson, J., Leichtling, G., Hildebran, C., Wakeland, W., & Deyo, R. A. (2017). High-risk prescribing and opioid overdose: prospects for prescription drug monitoring program–based proactive alerts. Pain, 159(1), 150–156. https://doi.org/10.1097/j.pain.0000000000001078
Gicquelais, R. E., Jannausch, M., Bohnert, A. S., Thomas, L., Sen, S., & Fernandez, A. C. (2020). Links between suicidal intent, polysubstance use, and medical treatment after non-fatal opioid overdose. Drug and Alcohol Dependence, 212, 108041. https://doi.org/10.1016/j.drugalcdep.2020.108041
Guy, G. P., Zhang, K., Bohm, M. K., Losby, J., Lewis, B., Young, R., Murphy, L. B., & Dowell, D. (2017). Vital signs: Changes in opioid prescribing in the United States, 2006–2015. MMWR Morbidity and Mortality Weekly Report, 66(26), 697–704. https://doi.org/10.15585/mmwr.mm6626a4
Haffajee, R. L., Lin, L. A., Bohnert, A. S. B., & Goldstick, J. E. (2019). Characteristics of US counties with high opioid overdose mortality and low capacity to deliver medications for opioid use disorder. JAMA Network Open, 2(6), e196373. https://doi.org/10.1001/jamanetworkopen.2019.6373
Haq, N., McMahan, V. M., Torres, A., Santos, G., Knight, K., Kushel, M., & Coffin, P. O. (2021). Race, pain, and opioids among patients with chronic pain in a safety-net health system. Drug and Alcohol Dependence, 222, 108671. https://doi.org/10.1016/j.drugalcdep.2021.108671
Kedia, S., Dillon, P. J., Schmidt, M., Entwistle, C., & Arshad, H. (2022). Public health impacts of drug overdose and mental health. In Springer eBooks (pp. 1–24). https://doi.org/10.1007/978-3-030-67928-6_14-1
King, N. B., Fraser, V., Boikos, C., Richardson, R., & Harper, S. (2014). Determinants of Increased Opioid-Related Mortality in the United States and Canada, 1990–2013: A Systematic review. American Journal of Public Health, 104(8), e32–e42. https://doi.org/10.2105/ajph.2014.301966
Knowlton, A., Weir, B. W., Hazzard, F., Olsen, Y., McWilliams, J., Fields, J., & Gaasch, W. (2013). EMS runs for suspected opioid overdose: Implications for surveillance and prevention. Prehospital Emergency Care, 17(3), 317–329. https://doi.org/10.3109/10903127.2013.792888
Krawczyk, N., Eisenberg, M., Schneider, K. E., Richards, T. M., Lyons, B. C., Jackson, K., Ferris, L., Weiner, J. P., & Saloner, B. (2019). Predictors of Overdose Death among High-Risk Emergency Department Patients with Substance-Related Encounters: A Data Linkage Cohort study. Annals of Emergency Medicine, 75(1), 1–12. https://doi.org/10.1016/j.annemergmed.2019.07.014
Lanier, W. A., Johnson, E. M., Rolfs, R. T., Friedrichs, M. D., & Grey, T. C. (2012). Risk Factors for Prescription Opioid-Related Death, Utah, 2008–2009. Pain Medicine, 13(12), 1580–1589. https://doi.org/10.1111/j.1526-4637.2012.01518.x
Lo-Ciganic, W., Donohue, J. M., Hulsey, E. G., Barnes, S., Li, Y., Kuza, C. C., Yang, Q., Buchanich, J., Huang, J. L., Mair, C., Wilson, D. L., & Gellad, W. F. (2021). Integrating human services and criminal justice data with claims data to predict risk of opioid overdose among Medicaid beneficiaries: A machine-learning approach. PLoS ONE, 16(3), e0248360. https://doi.org/10.1371/journal.pone.0248360
Lo-Ciganic, W., Donohue, J. M., Yang, Q., Huang, J. L., Chang, C., Weiss, J. C., Guo, J., Zhang, H. H., Cochran, G., Gordon, A. J., Malone, D. C., Kwoh, C. K., Wilson, D. L., Kuza, C. C., & Gellad, W. F. (2022). Developing and validating a machine-learning algorithm to predict opioid overdose in Medicaid beneficiaries in two US states: a prognostic modelling study. The Lancet Digital Health, 4(6), e455–e465. https://doi.org/10.1016/s2589-7500(22)00062-0
Lo-Ciganic, W., Huang, J. L., Zhang, H. H., Weiss, J. C., Wu, Y., Kwoh, C. K., Donohue, J. M., Cochran, G., Gordon, A. J., Malone, D. C., Kuza, C. C., & Gellad, W. F. (2019). Evaluation of Machine-Learning Algorithms for predicting opioid overdose risk among Medicare beneficiaries with opioid prescriptions. JAMA Network Open, 2(3), e190968. https://doi.org/10.1001/jamanetworkopen.2019.0968
Lowder, E. M., Amlung, J., & Ray, B. R. (2020). Individual and county-level variation in outcomes following non-fatal opioid-involved overdose. Journal of Epidemiology & Community Health, 74(4), 369–376. https://doi.org/10.1136/jech-2019-212915
Ly, D. P. (2021). Association of patient race and ethnicity with differences in opioid prescribing by primary care physicians for older adults with new low back pain. JAMA Health Forum, 2(9), e212333. https://doi.org/10.1001/jamahealthforum.2021.2333
Marks, C., Abramovitz, D., Donnelly, C. A., Carrasco-Escobar, G., Carrasco-Hernández, R., Ciccarone, D., González-Izquierdo, A., Martin, N. K., Strathdee, S. A., Smith, D. M., & Bórquez, A. (2021). Identifying counties at risk of high overdose mortality burden during the emerging fentanyl epidemic in the USA: a predictive statistical modelling study. The Lancet Public Health, 6(10), e720–e728. https://doi.org/10.1016/s2468-2667(21)00080-3
Pike, J. R., Fadardi, J. S., Stacy, A. W., & Xie, B. (2020). The prospective association between illicit drug use and nonprescription opioid use among vulnerable adolescents. Preventive Medicine, 143, 106383. https://doi.org/10.1016/j.ypmed.2020.106383
Ruhm, C. J. (2017). Geographic variation in opioid and heroin involved drug poisoning mortality rates. American Journal of Preventive Medicine, 53(6), 745–753. https://doi.org/10.1016/j.amepre.2017.06.009
Saloner, B., Chang, H., Krawczyk, N., Ferris, L., Eisenberg, M., Richards, T., Lemke, K., Schneider, K. E., Baier, M., & Weiner, J. P. (2020). Predictive modeling of opioid overdose using linked statewide medical and criminal justice data. JAMA Psychiatry, 77(11), 1155. https://doi.org/10.1001/jamapsychiatry.2020.1689
Schell, R. C., Allen, B., Goedel, W. C., Hallowell, B. D., Scagos, R., Li, Y., Krieger, M. S., Neill, D. B., Marshall, B. D. L., Cerda, M., & Ahern, J. (2021). Identifying predictors of opioid overdose death at a neighborhood level with machine learning. American Journal of Epidemiology, 191(3), 526–533. https://doi.org/10.1093/aje/kwab279
World Health Organization: WHO. (2023a, August 29). Opioid overdose. https://www.who.int/news-room/fact-sheets/detail/opioid-overdose#:~:text=The%20number%20of%20opioid%20overdoses,on%20the%20illicit%20drug%20market.
World Health Organization: WHO. (2023b, August 29). Opioid overdose. https://www.who.int/news-room/fact-sheets/detail/opioid-overdose#:~:text=The%20number%20of%20opioid%20overdoses,on%20the%20illicit%20drug%20market.
Wu, L., & Ringwalt, C. (2005). Use of substance abuse services by young uninsured American adults. Psychiatric Services, 56(8), 946–953. https://doi.org/10.1176/appi.ps.56.8.946
Zedler, B., Xie, L., Wang, L., Joyce, A., Vick, C., Kariburyo, F., Rajan, P., Baser, O., & Murrelle, L. (2014). Risk Factors for Serious Prescription Opioid-Related Toxicity or Overdose among Veterans Health Administration Patients. Pain Medicine, 15(11), 1911–1929. https://doi.org/10.1111/pme.12480
Zhao, Y., Liu, Y., Lv, F., He, X., Ng, W. H., Qiu, S., Zhang, L., Xing, Z., Guo, Y., Zu, J., Yeo, Y. H., & Ji, F. (2024). Temporal trend of drug overdose-related deaths and excess deaths during the COVID-19 pandemic: a population-based study in the United States from 2012 to 2022. EClinicalMedicine, 74, 102752. https://doi.org/10.1016/j.eclinm.2024.102752





