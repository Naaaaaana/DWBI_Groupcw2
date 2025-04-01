# DWBI_Groupcw2 
# Group No.11 / Group members: Emir , Quanlin Xu , Nannan Peng

# Task A 
## Data Understanding：
### 1. Identify and remove null values;

[31/03 Nana]

**#Missing value percentage：**  
CustomerDOB	0.32%  
CustGender	0.10%  
CustLocation	0.01%  
CustAccountBalance	0.23%  

**#The percentage is not really high. So I suggest to delete them directly.**  
**#The amount after deletion is：(1041614, 9)**  
**#Deleted data is less than 0.7% of the total data size. （origional data size:(1048567, 9) ）**  






   
### 2. Identify and remove invalid transaction amount values;

[31/03 Nana]

**#Then, there are lots of age and transaction error there.  
#Add new feature 'Age' into df.**  

```python 
df_Fact1['CustomerDOB'] = pd.to_datetime(df_Fact1['CustomerDOB'], dayfirst=True, errors='coerce') current_year = 2025 df_Fact1['Age'] = current_year - df_Fact1['CustomerDOB'].dt.year
```


**#Check age and transaction error:**  
```python
age_error_amount = df_Fact1[(df_Fact1['Age'] < 1) | (df_Fact1['Age'] > 100)].shape[0]  
print(f'age_error_amount: {age_error_amount}')
```

transaction_error_amount = df_Fact1[df_Fact1['TransactionAmount (INR)'] <= 1].shape[0]  
print(f'transaction_error_amount: {transaction_error_amount}')  

**#Output:**  
age_error_amount: 161082  
transaction_error_amount: 5704  

**#According to requirements of assignment, I removed them.**  
**#Remove age and transaction error**  
df_Fact1_clean = df_Fact1[(df_Fact1['Age'] >= 1) & (df_Fact1['Age'] <= 100) & (df_Fact1['TransactionAmount (INR)'] >= 0.01)]  

**#After clean:**  
(879905, 10)


   
   
### 3. Identify and remove invalid age values;

[31/03 Nana]

**#same with section 2**  



 
### 4. Display the 5 top Locations where the maximum number of transactions occurre.

[31/03 Nana]  
**#There are some problems of TransactionDate**  
**#some of that after 2025 and between 2025-2029**  
**#some of that before the consumer was born**  

**#convert TransectionDate to datetime type**  
df_Fact1_clean['TransactionDate'] = pd.to_datetime(df_Fact1_clean['TransactionDate'], dayfirst=True, errors='coerce')  

**#filter Transectiondate error**  
future_transactions = df_Fact1_clean[df_Fact1_clean['TransactionDate'] >= '2025-01-01']  
pre_birth_transactions = df_Fact1_clean[df_Fact1_clean['TransactionDate'] <= df_Fact1_clean['CustomerDOB']]  

**#Drop transactiondate error**  
invalid_transactions = (df_Fact1_clean['TransactionDate'] >= '2025-01-01') | \  
                       (df_Fact1_clean['TransactionDate'] <= df_Fact1_clean['CustomerDOB'])  

df_Fact1_clean1 = df_Fact1_clean[~invalid_transactions]  

**#Final data shape((879621, 10))**  

**#Cleaning completed, we can rank location then.**  

**#check top 100 cities**  
top_100 = df_Fact1_clean1['CustLocation'].value_counts().head(100)  
print(top_100)  
**#output**  
CustLocation  
MUMBAI               86281  
BANGALORE            70687  
NEW DELHI            66170  
GURGAON              62749  
DELHI                60478  
NOIDA                28598  
CHENNAI              23297  
PUNE                 21992  
HYDERABAD            19797  
THANE                18895  
KOLKATA              15292  
GHAZIABAD            12712  
NAVI MUMBAI          10400  
AHMEDABAD            10036  
FARIDABAD             9511  
JAIPUR                8723  
CHANDIGARH            7954  
LUCKNOW               6657  
MOHALI                5244  
SURAT                 4502  
NASHIK                4224  
LUDHIANA              3906  
VISAKHAPATNAM         3521  
DEHRADUN              3420  
INDORE                3248  
AMRITSAR              3058  
KANPUR                3019  
MEERUT                3011  
VADODARA              3009  
NAGPUR                2880  
AGRA                  2829  
GREATER NOIDA         2698  
COIMBATORE            2667  
AURANGABAD            2644  
RANGA REDDY           2614  
PANCHKULA             2578  
GUNTUR                2531  
GUWAHATI              2445  
JALANDHAR             2380  
BHOPAL                2283  
PATNA                 2217  
UDAIPUR               2119  
REWARI                2018  
RANCHI                1982  
BENGALURU             1885  
HOWRAH                1876  
PATIALA               1826  
ALWAR                 1797  
ERNAKULAM             1736  
NORTH 24 PARGANAS     1721  
SONIPAT               1711  
ALLAHABAD             1674  
PANIPAT               1655  
SECUNDERABAD          1654  
BAMBOLIM              1630  
ZIRAKPUR              1515  
SILIGURI              1503  
KOLHAPUR              1451  
RAJKOT                1445  
KANGRA                1418  
BAREILLY              1406  
ALIGARH               1390  
RAIPUR                1372  
JAMSHEDPUR            1343  
WARANGAL              1293  
KARNAL                1287  
AMBALA                1267  
MANGALORE             1230  
KOTTAYAM              1205  
KHARAR                1198  
SAHARANPUR            1178  
MORADABAD             1176  
SAS NAGAR             1167  
HOSUR                 1155  
SHIMLA                1106  
VARANASI              1105  
KOTA                  1087  
BHUBANESHWAR          1080  
BURDWAN               1076  
VAPI                  1071  
JAMMU                 1054  
JODHPUR               1045  
SALEM                 1013  
NELLORE               1010  
VELLORE               1002  
ROHTAK                 989  
BATHINDA               975  
RUDRAPUR               971  
VIJAYAWADA             971  
BILASPUR               970  
GANDHINAGAR            965  
GORAKHPUR              956  
KOCHI                  919  
HOOGHLY                902  
SOUTH 24 PARGANAS      895  
YAMUNANAGAR            887  
DHANBAD                885  
KARIMNAGAR             880  
KANCHEEPURAM           873  
THRISSUR               847  
Name: count, dtype: int64  

**#check transactions number of top 100， that is almost 80% of total data**  
total_customers = top_100_df['TransactionCounts'].sum()  
print(total_customers)  
**#output: 687151**  

**#according to cities list check, there are two cities name repeated. They are "BANGALORE（70687）" & "BENGALURU（1885）" where are New and old names**  
**#Unify the same city name**  
df_Fact1_clean2['CustLocation'] = df_Fact1_clean2['CustLocation'].replace('BENGALURU', 'BANGALORE')  

**#Display the 5 top Locations**  
query = """  
SELECT CustLocation, COUNT(*) AS transaction_count  
FROM transactions  
GROUP BY CustLocation  
ORDER BY transaction_count DESC  
LIMIT 5  
"""  

top_locations = pd.read_sql_query(query, conn)  
print(top_locations)  

**#output**  
  CustLocation  transaction_count  
0       MUMBAI              86281  
1    BANGALORE              72572  
2    NEW DELHI              66170  
3      GURGAON              62749  
4        DELHI              60478  




## Perform RFM Segmentation:
### 5. Write a query to define and calculate the RFM values per Customer;

[01/04 Nana]
**#Dedine RFM in report**





### 6. Check the distribution of Recency, Frequency & Monetary Values;






   
### 7. Briefly discuss the issue of skewness and remove skew from the dat







# Task B
## Customer segmentation with k-means
### 1. Brief discussion of the appropriateness of K-means as the adopted clustering methodology.







### 2. It is necessary to discuss the techniques applied to identify the best value of K-number of clusters.






### 3. Implementation of K-Means using Python via Google Colab






## Data Mart Desing
### 4. Based on your findings (Task B), suggest the main dimensions and metrics for designing a data mart for the analysis needs of the marketing department.



   






# Task C
## Review Results and CRM as a driver for Sustainability
### 1. Discuss briefly the business value for marketers of the specific clusters of customers and their behaviour per Location – in terms of maximum number of transactions occurred in the 5 top Locations and cluster descriptive characteristics for RFM values.










### 2. Discuss how CRM tools contribute to sustainability for banking, highlighting the practical benefits and offering insights into how these systems can be used to create a more sustainable future for businesses.






