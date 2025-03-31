# DWBI_Groupcw2 
# Group No.11 / Group members: Emir , Quanlin Xu , Nannan Peng

# Task A 
# Data Understanding：
# 1. Identify and remove null values;

[31/03 Nana]
#Missing value percentage：
CustomerDOB	0.32%
CustGender	0.10%
CustLocation	0.01%
CustAccountBalance	0.23%

#The percentage is not really high. So I suggest to delete them directly.
#The amount after deletion is：(1041614, 9)
#Deleted data is less than 0.7% of the total data size. （origional data size:(1048567, 9) ）






   
# 2. Identify and remove invalid transaction amount values;

[31/03 Nana]
#Then, there are lots of age and transaction error there.
#Add new feature 'Age' into df.

df_Fact1['CustomerDOB'] = pd.to_datetime(df_Fact1['CustomerDOB'], dayfirst=True, errors='coerce')
current_year = 2025
df_Fact1['Age'] = current_year - df_Fact1['CustomerDOB'].dt.year


#Check age and transaction error:
age_error_amount = df_Fact1[(df_Fact1['Age'] < 1) | (df_Fact1['Age'] > 100)].shape[0]
print(f'age_error_amount: {age_error_amount}')

transaction_error_amount = df_Fact1[df_Fact1['TransactionAmount (INR)'] <= 1].shape[0]
print(f'transaction_error_amount: {transaction_error_amount}')

#Output:
age_error_amount: 161082
transaction_error_amount: 5704

#According to requirements of assignment, I removed them.
#Remove age and transaction error
df_Fact1_clean = df_Fact1[(df_Fact1['Age'] >= 1) & (df_Fact1['Age'] <= 100) & (df_Fact1['TransactionAmount (INR)'] >= 1)]

#After clean:
(879665, 10)


   
   
# 3. Identify and remove invalid age values;

[31/03 Nana]
#same with section 2



 
# 4. Display the 5 top Locations where the maximum number of transactions occurre.

[31/03 Nana]
#There are some problems of TransactionDate
#some of that after 2025 and between 2025-2029
#some of that before the consumer was born

#convert TransectionDate to datetime type
df_Fact1_clean['TransactionDate'] = pd.to_datetime(df_Fact1_clean['TransactionDate'], dayfirst=True, errors='coerce')

#filter Transectiondate error
future_transactions = df_Fact1_clean[df_Fact1_clean['TransactionDate'] >= '2025-01-01']
pre_birth_transactions = df_Fact1_clean[df_Fact1_clean['TransactionDate'] <= df_Fact1_clean['CustomerDOB']]

#Drop transactiondate error
invalid_transactions = (df_Fact1_clean['TransactionDate'] >= '2025-01-01') | \
                       (df_Fact1_clean['TransactionDate'] <= df_Fact1_clean['CustomerDOB'])

df_Fact1_clean1 = df_Fact1_clean[~invalid_transactions]

#Final data shape((879621, 10))

#Cleaning completed, we can rank location then.





# Perform RFM Segmentation:
# 5. Write a query to define and calculate the RFM values per Customer;






# 6. Check the distribution of Recency, Frequency & Monetary Values;






   
# 7. Briefly discuss the issue of skewness and remove skew from the dat







# Task B
# Customer segmentation with k-means
1. Brief discussion of the appropriateness of K-means as the adopted clustering methodology.







2. It is necessary to discuss the techniques applied to identify the best value of K-number of clusters.






3. Implementation of K-Means using Python via Google Colab






# Data Mart Desing
4. Based on your findings (Task B), suggest the main dimensions and metrics for designing a data mart for the analysis needs of the marketing department.



   






# Task C
# Review Results and CRM as a driver for Sustainability
1. Discuss briefly the business value for marketers of the specific clusters of customers and their behaviour per Location – in terms of maximum number of transactions occurred in the 5 top Locations and cluster descriptive characteristics for RFM values.










2. Discuss how CRM tools contribute to sustainability for banking, highlighting the practical benefits and offering insights into how these systems can be used to create a more sustainable future for businesses.






