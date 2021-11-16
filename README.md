# Electric Rotor Temperature Prediction :electric_plug:

![](https://img.shields.io/badge/Author-SinYee-informational?style=flat&logo=<LOGO_NAME>&logoColor=white&color=2bbc8a) ![](https://img.shields.io/badge/Language-R-informational?style=flat&logo=<LOGO_NAME>&logoColor=white&color=2bbc8a)

## Project Overview
The aim of this project is to make prediction on rotor temperature (pm) and find the best model among the other conducted models. The exploratory data analysis (EDA) is presented in the first section of the notebook to anaylise and summarise the characteristic of the variables and the relationship between the variables. Statistical graphics and data visualisation techniques will be used in the section. The second section of the notebook will be the development of regression model. The process in building the model will be presented in this section in detail. Each model will be compared with explaination and the best one will be selected. The models are:

* Least squared regression model,
* Ridge regression model,
* Lasso regression model,
* K-nearest Neighbours Regression Model (KNN), and
* Random forest model

Statistical assessments such as Adjusted R-squared and P-values will be used in the model specification process. Data provided for this assessment is `pmsm_temperature_data_A1_2021.csv`.

## Exploratory Data Analysis EDA

* Summary of attributes table below identifies the type and sub-type of the attributes. It also shows some initial observations about the ranges and mean values of the attributes. 

![](https://github.com/sinyeen/Electric-Rotor-Temperature-Prediction/blob/main/Images/sum%20attr.JPG)

* Analyse variable distribution with **boxplots** and **histograms** (diagram shown in notebook). Finding from the boxplot and histogram:

  * <font color='orange'>pm</font>, <font color='orange'>ambient</font>, <font color='orange'>torque</font>, and <font color='orange'>i_q</font> is distributed normally, with the mean around zero, and the data on the left side of the average is likely to occur as on the right side of the average.
  * <font color='orange'>coolant</font> has a random distribution where it has several peaks and it does not has as apparent pattern.
  * <font color='orange'>u_d</font> and <font color='orange'>motor_speed</font> have quite uniform distribution, indicating the data is quite consistent. The frequency of each value of u_d and motor_speed is very similar to that of the others. However, there are exceptional value of u_d and motor_speed that have high frequency of occurrance. For u_d, value around 0 to 0.2 has very high frequency of occurrance. For motor_speed, value around -1.5 has very high frequenct of occurrance. As shown in the boxplot, these values are able to shift the mean of the distribution of the variable.
  * The histogram of <font color='orange'>i_d</font> shows a skewed distribution to the left. It can alos be said that the distribution is negatively skewed. The upper value fo i_d has high numebr of occurrence and very few in the lower value of i_d.

* Investifate pairs of variables with **Scatterplot Matrix and Levelplot.**

![](https://github.com/sinyeen/Electric-Rotor-Temperature-Prediction/blob/main/Images/matrix.JPG)

Findings from the levelplot:

Note that positive correlation is towards blue, and negative correlation is towards purple. 

**Relationship between the predictor variables and the target variable**

  - <font color='orange'>ambient</font>, <font color='orange'>coolant</font>, and <font color='orange'>motor_speed</font> are positively correlated with the <font color='orange'>pm</font>
  - <font color='orange'>i_d</font>, <font color='orange'>torque</font>, <font color='orange'>u_d</font>, and <font color='orange'>i_q</font> are negatively correlated with the <font color='orange'>pm</font>
  - <font color='orange'>u_q</font> have weak linear positive correlation with the <font color='orange'>pm</font>

![](https://github.com/sinyeen/Electric-Rotor-Temperature-Prediction/blob/main/Images/corr.JPG)

The pair-wise relationship between the variables can be read from the scatterplots generated. According to the pair-wise scatter plot and the correlation matrix:- 

- a **very strong linear positive correlation** between  <font color='orange'>torque</font> and  <font color='orange'>i_q</font>
- a **very strong linear negetive correlation** between  <font color='orange'>torque</font> and  <font color='orange'>u_d</font>
- a **very strong linear negetive correlation** between  <font color='orange'>u_d</font> and  <font color='orange'>i_q</font>
- a **strong linear positive correlation** between  <font color='orange'>u_q</font> and  <font color='orange'>motor_speed</font>
- a **strong linear negative correlation** between  <font color='orange'>i_d</font> and  <font color='orange'>motor_speed</font>
- a **strong curvilinear** relationship between the <font color='orange'>u_d</font> and <font color='orange'>u_q</font>
- a **curvilinear relationship** between the <font color='orange'>u_d</font> and <font color='orange'>i_d</font>
- a **curvilinear** relationship between the <font color='orange'>u_d</font> and <font color='orange'>motor_speed</font>
- a **strong curvilinear** relationship between the <font color='orange'>torque</font> and <font color='orange'>u_q</font>
- a **strong curvilinear** relationship between the <font color='orange'>i_q</font> and <font color='orange'>u_q</font>
- a **strong curvilinear** relationship between the <font color='orange'>motor_speed</font> and <font color='orange'>torque</font>
- a **strong curvilinear** relationship between the <font color='orange'>motor_speed</font> and <font color='orange'>i_q</font>
- a **curvilinear relationship** between the <font color='orange'>i_d</font> and <font color='orange'>i_q</font>

*Analysis of Pairs of Variables*

The rotor temperature, <font color='orange'>pm</font> is determined by the eight feature attributes (e.g., <font color='orange'>ambient</font>, <font color='orange'>u_d</font>, <font color='orange'>u_q</font>, <font color='orange'>i_d</font>, <font color='orange'>i_q</font>, <font color='orange'>torque</font>, <font color='orange'>motor_speed</font>, and <font color='orange'>coolant</font>). These variables are inter-related with higher ambient temperature, higher coolant temperature, faster motor speed, lower current q and d component (i_q and i_d), lower torque which is induced by current, and a lower voltage d-component(u_d).

As mentioned above, the effect of some of the predictor variables can vary depend on the value of another predictor variables. Some are strongly related to other variables, for example, <font color='orange'>torque</font> has strong relation with the <font color='orange'>i_q</font>, <font color='orange'>u_d</font>, and <font color='orange'>motor_speed</font>. There exist a strongly linear relationship between <font color='orange'>torque</font> and <font color='orange'>i_q</font>, in which the increase in i_q will increase the torque, and vice versa. There are also many curvilinear relationship going on in this dataset. For example, looking at <font color='orange'>u_q</font> and <font color='orange'>u_q</font>, with the increase in the <font color='orange'>u_d</font> there is an increase in the <font color='orange'>u_q</font> (positive correlation), however when the <font color='orange'>u_d</font> increase beyond a certain point, then <font color='orange'>u_q</font> starts to fall (negative correlation). Similar effect to the other variables with curvilinear relationship.

## Model Selection
The details of model development and selection steps are clearly explained in the notebook. The R-squared value and RMSE of the training and testing set of the models are shown as below:

|Model  |Computation Time|R-square  |RMSE                                                                            |
|-----------|-----------|----------|--------------------------------------------------------------------------------------|
|least square regression (full model)|0.01s|0.462|0.7096|
|least square regression (with interaction terms)|0.01s  |0.678|0.598|
|Ridge regression    |0.7s  |0.393|0.696| 
|Lasso regression        |0.73s  |0.448|0.709     |
|KNN        |2.73s  |0.832|0.716|
|Random forest|274.42  |-0.372|0.827                                    |

#### Result Overview
It can be seen that least-square regression model with interaction terms is the best model among the models. It has the smallest RMSE which means that it provide higher accuracy and better performance. It also required the least amount of computation time as compared to other models. 

#### Comparing parametric and non-parametric models
As mentioned above, linear regression model is still more suitable for this dataset. It outperforms other models because the given dataset is simple and linear (as the linear model complies with the linearity assumptions). Furthermore, in linear regression, the value of regression coefficients are able to provide assumption on the significance of the predictor variables, thus it provides better prediction. In short, linear regression is more suitable for simple and linear model like this, the non-parametric model like random forest and KNN are more suitable to support non linearity and complex model.

#### Comparing the linear regression models
In Ridge and Lasso regression model, it can be seen that after shrinking the regression coefficients of the model, the r-square on the test set has decreases. This is because the coefficient estimates is minimised in addition to RSS. As the high values of coefficient estimates are panalised, the effect of the predictor variables on the target variable will be underestimated, thus model complexity decreased. This prevent the over-fitting problem, thus the accuracy of the model has improved as compared to the least-sqaure regression model. Furthermore, Lasso regression performs feature selection by taking off variable i_q. This cause the model to be further simplify. However, sometimes simple model will not give high accuracy on the testing set. 

From the EDA above, it can be seen that the effect of some of the variable depends on the value of another variables. In other words, the model may create multicoliinearity. Therefore, try to fit linear regression models with interaction effects in this case provides possibility to fit a wider variety of function, thus the model will fit better and achieve better performance. 

## Conclusion
In conclusion, a balance goodness of fit together with simplicity is the key for a good model selection. Other than the performance and simplicity of the model, the computation time also sometimes need to be considered for working efficiency. It is vital to always apply statistical analytics, theorectical understanding of the variables, and some practical knowledge in the model selection process.
