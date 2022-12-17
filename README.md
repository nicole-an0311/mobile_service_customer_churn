# mobile_service_customer_churn
# Overview  
The wireless telecommunication industry provides phone call, internet data, and other services to consumers through transmitting signals over radio towers and satellites. The head companies are usually customer-oriented that values consumer volume immensely. Customer churn for a mobile phone company such as Verizon, Mint Mobile, or ATT can cost up to $1,200 per customer. It is a significant cost in profit if the company let these churners leave without any strategies to change their minds. However, if incentivization plans are implemented to potential churners, the company can make $500 for each customer who stays with the current service by providing a $50 voucher and targeted strategies.
Problem Statement 
This challenge is to predict the likelihood of churn using machine learning models on customers’ characteristics and find the least costing strategy to incentivize likely churners.
# Metrics
We will evaluate the models primarily using AUC, the area under the ROC curve. The ROC curve shows the false positive rate (1 – specificity) on the x-axis and the true positive rate (sensitivity) on the y-axis. AUC is the measurement of performance for classification models with a 0.5 threshold. When AUC is very close to 1, the model performs very well in separating class 1 from class 0. Below is the ROC curve of the best model, KNN model, with K=12.

## ROC Curve
 
We also measure the goodness of a model based on accuracy, precision, and recall rates, based on the model’s true positive (TP), true negative (TN), false positive (FP), and false negative (FN).
## 	Accuracy is calculated with the formula:
Accuracy=((TP+TN))/((TP+TN+FP+FN))
Accuracy gives how many times the model was correct overall. However, the fundamental problem with accuracy is that it does not differentiate mistakes on churns and not churns. For example, it charges the company $50 if we identify a non-churner as churner, but it costs $1200 if we failed to identify a true churner. 
##	Precision is calculated with the formula:
Precision=TP/((TP+FP))
Precision evaluates how fit the model is at predicting a specific category. 
##	Recall is calculated with the formula:
Recall=TP/((TP+FN))
Recall evaluates the number of times the model is able to detect a specific category. 
## Explanatory Analysis on Variables

 
Churn (attrition) is our target variable. The bar graph above shows the attrition (i.e. changes service company) rate to be 5.5% while the stay rate to be 94.5%. That is, out of all 90901 rows from the training data, 4975 people churned. The default accuracy would be the majority case which is 94.5%. 
    
The charts above show 4 categorical variables that are interesting. Streaming plan, paperless billing, and payment methods all have factors that are significantly different from other factors in terms of churn rate. For instance, customers without paperless billing have a churn rate compared to those opt for paperless billing. On the other hand, although the companies believe that network speed may influence churn rate, there is no obvious difference in churn rate between network 4Glte and 5G customers. 
 
The scatterplot above shows 2 interesting trends for the numeric variables monthly_minutes and total_billed. The light blue scatter points clustered around the right-hand side of the x-axis, indicating a positive relationship between monthly usage and churn rate. The higher the monthly usage for the customer, the more likely the customer will churn. Similarly, the light blue scatter points clustered around the lower side of the y-axis, indicating a negative relationship between total bill amount and churn rate. The lower the total bill amount for the customer, the more likely the customer will churn.

## Data Dictionary 
Variable	Definition 	Keep	Key
monthly_minutes	Monthly usage in minutes		
customer_service_calls	# of calls to customer services	
streaming_minutes	Monthly streaming in minutes	
total_billed	Total bill amount in $		
prev_balance	Previous balance statement	
late_payments	# of late payments		
ip_address_asn	IP address	Ignore	
phone_area_code	Phone area code	Ignore	
customer_reg_date	Customer registration date	Ignore	
email_domain	Customer email 	Ignore	
phone_model	Phone model	Ignore	
billing_city	Billing city	Ignore	
billing_postal	Billing zip code	Ignore	
billing_state	Billing state	Ignore	
partner	A partnering company who helped initiate the phone contract		Yes, No
phone_service	phone service	Ignore	
multiple_lines	multiple lines	Ignore	
streaming_plan	streaming plan		3GB, 6GB, 12GB, Unlimited
mobile_hotspot	mobile hotspot		
wifi_calling_text	wifi calling text		
online_backup	online backup		
device_protection	device protection 	Ignore	A-Z
number_phones	# of phones obtained		
contract_code	code of the contract with the company	Ignore	A-Z
currency_code	payment currency		cad, eau, usd
maling_code	mailing code	Ignore	A-Z
paperless_billing	opt for paperless billing		
payment_method	payment method		Bank transfer, electronic check, credit card, mailed check
customer_id	customer id	Ignore	
billing_address	full billing address	Ignore	
gender	gender	Ignore	
network_speed	network speed	Ignore	4Glte, 5G
senior_citizen	senior citizen	Ignore	0=No, 1=Yes


## Methodology 
	Data partitioning
	The holdout dataset is splitted randomly by 80/20 where 80% were training data and 20% were testing data.
	Data preprocessing
	Formula for KNN Model
	churn ~ monthly_minutes + customer_service_calls + streaming_minutes + total_billed + prev_balance + late_payments + partner + streaming_plan + mobile_hotspot + wifi_calling_text + number_phones + currency_code + paperless_billing + payment_method
	K = 12
	Formula for Reduced Logistic Model
	churn ~ monthly_minutes + streaming_minutes + total_billed + prev_balance + late_payments + partner + phone_service + multiple_lines +  streaming_plan + mobile_hotspot + wifi_calling_text + number_phones + currency_code + paperless_billing + payment_method
	Numeric Predictor Pre-Processing 
	Replaced missing numeric variables with median 
	Centered and scaled numeric predictors to have a mean of 0 and standard deviation of 1 because we are using a KNN which is sensitive to varying scales of data
	Categorical Predictor Pre-Processing 
	Replaced missing categorical variables with a new level called “unknown”
	Encoded categorical variables with dummy variables of 0s and 1s
	Model specification  
	Trained 1 K-Nearest Neighbors (KNN) with K=12
	Variables are selected based on the significance in the logistic model and explanatory analysis on all predictor variables
	Trained a reduced logistic model 
## Model Metrics & Evaluation 
### KNN Model 
### Model Summary 

.estimator	part	accuracy	roc_auc	precision	recall
Knn = 12	training	0.9817794	0.9989029	0.9763378	0.6803653
Knn = 12	testing	0.9752489	0.9186088	0.9370315	0.6050339
Confusion Metrices

  

Logistic Model
Model Summary 
.estimator	part	accuracy	roc_auc	precision	recall
Logistic	training	0.9653603	0.9140977	0.8238507	0.4591578
Logistic	testing	0.9627083	0.9181768	0.8076256	0.4511133

### Confusion Metrices 
  


### Analysis of the 2 Models
From model summary, KNN model yields a better AUC for both training and testing data, so the KNN model performs better. The testing accuracy of 97.5% means that 97.5% of the time the model made correct predictions. The default accuracy is 94.5% so the testing accuracy is way above that, indicating this model to be a good predicting model. The difference between precision and recall shows that both models are better at predicting churned customers compared to detecting churned customers. The training performance for KNN model is significantly higher than the testing performance, indicating a sign of overfitting. 
## Key Findings
*	From the graph below, the most important variable in the logistic model is total_billed and it has a negative coefficient. That is, for every dollar decrease in customer’s total bill, there is an increase of 0.04 in chance of churn rate.
*	From the graph below, paperless billing is also a very important variable in the logistic model and it has a negative coefficient. That is, if the customer opts for paperless billing, there is a decrease in chance of churn rate. 
*	Customers choosing payment methods of credit card, mailed check, and electronic check all have low churn rate, compared to those choosing bank transfer.
Top 10 Most Important Variables
 
*	Network speed does not influence the churn rate according to one of the bar charts from explanatory analysis. It means that customers will churn regardless of the speed of their internet data.
*	With the implementation of incentivizing plan, the companies will suffer a less loss of total profit. According to the testing performance of the KNN model, if the companies do nothing, they will lose $1,239,600. If implementing the plan, the company would lose $210,450. 
## Recommendations
*	Utilizing the predictions from KNN model, the companies should implement the incentivizing plan of vouchers. This plan saves $1,029,150 of potential loss due to churned customers.
*	The companies should target on customers with less total bills and high monthly usage because they are customers who look for good deals, and they are easily churned by other competitors in the industry. Consistently delivering promotions on services will keep customers from churning.
*	The companies should promote paperless billing because customers with paperless billing are less likely to churn. 
*	Also, the companies should look out the customers paying with bank transfers because they are likely to churn. Incentivizing them to use checks or credit cards would keep them from changing services.
*	The customers should target on incentivizing customers with high streaming plans because they are likely to churn. The companies could bundle streaming with other services so that these customers will stay with the current service. 



