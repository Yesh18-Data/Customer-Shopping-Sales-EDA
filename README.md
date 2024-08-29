# Customer-Shopping-Sales-EDA


Importing Libraries
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
Reading the Customer Sales data from CSV File
customer_shopping_data = pd.read_csv("C:\\Users\\user\\Downloads\\customer_shopping_data.csv\\customer_shopping_data.csv")
customer_shopping_data
invoice_no	customer_id	gender	age	category	quantity	price	payment_method	invoice_date	shopping_mall
0	I138884	C241288	Female	28	Clothing	5	1500.40	Credit Card	5/8/2022	Kanyon
1	I317333	C111565	Male	21	Shoes	3	1800.51	Debit Card	12/12/2021	Forum Istanbul
2	I127801	C266599	Male	20	Clothing	1	300.08	Cash	9/11/2021	Metrocity
3	I173702	C988172	Female	66	Shoes	5	3000.85	Credit Card	16/05/2021	Metropol AVM
4	I337046	C189076	Female	53	Books	4	60.60	Cash	24/10/2021	Kanyon
...	...	...	...	...	...	...	...	...	...	...
99452	I219422	C441542	Female	45	Souvenir	5	58.65	Credit Card	21/09/2022	Kanyon
99453	I325143	C569580	Male	27	Food & Beverage	2	10.46	Cash	22/09/2021	Forum Istanbul
99454	I824010	C103292	Male	63	Food & Beverage	2	10.46	Debit Card	28/03/2021	Metrocity
99455	I702964	C800631	Male	56	Technology	4	4200.00	Cash	16/03/2021	Istinye Park
99456	I232867	C273973	Female	36	Souvenir	3	35.19	Credit Card	15/10/2022	Mall of Istanbul
99457 rows × 10 columns

Checking the datatypes and Changing the data types of column "invoice_date"
customer_shopping_data.dtypes
invoice_no         object
customer_id        object
gender             object
age                 int64
category           object
quantity            int64
price             float64
payment_method     object
invoice_date       object
shopping_mall      object
dtype: object
'object' to 'datetime'

customer_shopping_data['invoice_date'] = pd.to_datetime(customer_shopping_data['invoice_date'],format = '%d/%m/%Y')
customer_shopping_data.dtypes
invoice_no                object
customer_id               object
gender                    object
age                        int64
category                  object
quantity                   int64
price                    float64
payment_method            object
invoice_date      datetime64[ns]
shopping_mall             object
dtype: object
Adding the new columns Sales and Year to the data (Feature Enginnering)
# Adding the Sales column 
customer_shopping_data['Sales'] = customer_shopping_data['quantity']* customer_shopping_data['price']
# Adding the Year column
customer_shopping_data['Year'] = customer_shopping_data.invoice_date.dt.year
# Extract Year and Month from 'invoice_date'
customer_shopping_data['Year'] = customer_shopping_data['invoice_date'].dt.year
customer_shopping_data['Month'] = customer_shopping_data['invoice_date'].dt.month
# Alternative 

# Extracting the year using the assign method
customer_shopping_data.assign(Year=customer_shopping_data['invoice_date'].dt.year)

# Using lambda to extract the year
customer_shopping_data['invoice_date'].apply(lambda x: x.year)
0        2022
1        2021
2        2021
3        2021
4        2021
         ... 
99452    2022
99453    2021
99454    2021
99455    2021
99456    2022
Name: invoice_date, Length: 99457, dtype: int64
customer_shopping_data
invoice_no	customer_id	gender	age	category	quantity	price	payment_method	invoice_date	shopping_mall	Sales	Year	Month
0	I138884	C241288	Female	28	Clothing	5	1500.40	Credit Card	2022-08-05	Kanyon	7502.00	2022	8
1	I317333	C111565	Male	21	Shoes	3	1800.51	Debit Card	2021-12-12	Forum Istanbul	5401.53	2021	12
2	I127801	C266599	Male	20	Clothing	1	300.08	Cash	2021-11-09	Metrocity	300.08	2021	11
3	I173702	C988172	Female	66	Shoes	5	3000.85	Credit Card	2021-05-16	Metropol AVM	15004.25	2021	5
4	I337046	C189076	Female	53	Books	4	60.60	Cash	2021-10-24	Kanyon	242.40	2021	10
...	...	...	...	...	...	...	...	...	...	...	...	...	...
99452	I219422	C441542	Female	45	Souvenir	5	58.65	Credit Card	2022-09-21	Kanyon	293.25	2022	9
99453	I325143	C569580	Male	27	Food & Beverage	2	10.46	Cash	2021-09-22	Forum Istanbul	20.92	2021	9
99454	I824010	C103292	Male	63	Food & Beverage	2	10.46	Debit Card	2021-03-28	Metrocity	20.92	2021	3
99455	I702964	C800631	Male	56	Technology	4	4200.00	Cash	2021-03-16	Istinye Park	16800.00	2021	3
99456	I232867	C273973	Female	36	Souvenir	3	35.19	Credit Card	2022-10-15	Mall of Istanbul	105.57	2022	10
99457 rows × 13 columns

customer_shopping_data.dtypes
invoice_no                object
customer_id               object
gender                    object
age                        int64
category                  object
quantity                   int64
price                    float64
payment_method            object
invoice_date      datetime64[ns]
shopping_mall             object
Sales                    float64
Year                       int32
dtype: object
customer_shopping_data.shape
(99457, 12)
Top 5 records of the Customer shopping Data
customer_shopping_data.head() 
invoice_no	customer_id	gender	age	category	quantity	price	payment_method	invoice_date	shopping_mall	Sales	Year
0	I138884	C241288	Female	28	Clothing	5	1500.40	Credit Card	2022-08-05	Kanyon	7502.00	2022
1	I317333	C111565	Male	21	Shoes	3	1800.51	Debit Card	2021-12-12	Forum Istanbul	5401.53	2021
2	I127801	C266599	Male	20	Clothing	1	300.08	Cash	2021-11-09	Metrocity	300.08	2021
3	I173702	C988172	Female	66	Shoes	5	3000.85	Credit Card	2021-05-16	Metropol AVM	15004.25	2021
4	I337046	C189076	Female	53	Books	4	60.60	Cash	2021-10-24	Kanyon	242.40	2021
bottom 5 records of the Customer shopping Data
customer_shopping_data.tail()
invoice_no	customer_id	gender	age	category	quantity	price	payment_method	invoice_date	shopping_mall	Sales	Year
99452	I219422	C441542	Female	45	Souvenir	5	58.65	Credit Card	2022-09-21	Kanyon	293.25	2022
99453	I325143	C569580	Male	27	Food & Beverage	2	10.46	Cash	2021-09-22	Forum Istanbul	20.92	2021
99454	I824010	C103292	Male	63	Food & Beverage	2	10.46	Debit Card	2021-03-28	Metrocity	20.92	2021
99455	I702964	C800631	Male	56	Technology	4	4200.00	Cash	2021-03-16	Istinye Park	16800.00	2021
99456	I232867	C273973	Female	36	Souvenir	3	35.19	Credit Card	2022-10-15	Mall of Istanbul	105.57	2022
Descriptive Statistics of the Data
customer_shopping_data.describe()
age	quantity	price	invoice_date	Sales	Year
count	99457.000000	99457.000000	99457.000000	99457	99457.000000	99457.000000
mean	43.427089	3.003429	689.256321	2022-02-04 02:46:59.783424	2528.789268	2021.629408
min	18.000000	1.000000	5.230000	2021-01-01 00:00:00	5.230000	2021.000000
25%	30.000000	2.000000	45.450000	2021-07-19 00:00:00	136.350000	2021.000000
50%	43.000000	3.000000	203.300000	2022-02-05 00:00:00	600.170000	2022.000000
75%	56.000000	4.000000	1200.320000	2022-08-22 00:00:00	2700.720000	2022.000000
max	69.000000	5.000000	5250.000000	2023-03-08 00:00:00	26250.000000	2023.000000
std	14.990054	1.413025	941.184567	NaN	4222.475781	0.636136
Checking the nul Values
customer_shopping_data.isna().sum()
invoice_no        0
customer_id       0
gender            0
age               0
category          0
quantity          0
price             0
payment_method    0
invoice_date      0
shopping_mall     0
Sales             0
Year              0
dtype: int64
customer_shopping_data['gender'].value_counts()
gender
Female    59482
Male      39975
Name: count, dtype: int64
Analyze Categorical Columns Get a sense of the unique values and their frequencies in the categorical columns (gender, category, payment_method, shopping_mall).
# Unique values and their counts for categorical columns
for col in ['gender', 'category', 'payment_method', 'shopping_mall']:
    print(f"Value counts for {col}:\n", customer_shopping_data[col].value_counts(), "\n")
Value counts for gender:
 gender
Female    59482
Male      39975
Name: count, dtype: int64 

Value counts for category:
 category
Clothing           34487
Cosmetics          15097
Food & Beverage    14776
Toys               10087
Shoes              10034
Souvenir            4999
Technology          4996
Books               4981
Name: count, dtype: int64 

Value counts for payment_method:
 payment_method
Cash           44447
Credit Card    34931
Debit Card     20079
Name: count, dtype: int64 

Value counts for shopping_mall:
 shopping_mall
Mall of Istanbul     19943
Kanyon               19823
Metrocity            15011
Metropol AVM         10161
Istinye Park          9781
Zorlu Center          5075
Cevahir AVM           4991
Forum Istanbul        4947
Viaport Outlet        4914
Emaar Square Mall     4811
Name: count, dtype: int64 

# Checking for duplicate rows
duplicate_rows = customer_shopping_data[customer_shopping_data.duplicated()]
print("Number of duplicate rows: ", len(duplicate_rows))
Number of duplicate rows:  0
# Minimum and maximum dates
print("Date Range:\n", customer_shopping_data['invoice_date'].min(), "to", customer_shopping_data['invoice_date'].max())

# Frequency of transactions by month and year
customer_shopping_data['Month'] = customer_shopping_data['invoice_date'].dt.month
transactions_per_month = customer_shopping_data.groupby(['Year', 'Month']).size()
print("Transactions per Month and Year:\n", transactions_per_month)
Date Range:
 2021-01-01 00:00:00 to 2023-03-08 00:00:00
Transactions per Month and Year:
 Year  Month
2021  1        3835
      2        3407
      3        3813
      4        3724
      5        3848
      6        3783
      7        3984
      8        3723
      9        3670
      10       3916
      11       3798
      12       3881
2022  1        3847
      2        3447
      3        3947
      4        3763
      5        3849
      6        3798
      7        3893
      8        3912
      9        3683
      10       3848
      11       3765
      12       3799
2023  1        3926
      2        3628
      3         970
dtype: int64
# Calculate the number of transactions per month and year
transactions_per_month = customer_shopping_data.groupby(['Year', 'Month']).size().reset_index(name='Transaction_Count')

# Create a line plot
plt.figure(figsize=(12, 6))
sns.lineplot(data=transactions_per_month, x='Month', y='Transaction_Count', hue='Year', marker='o', palette='tab10')

plt.title('Frequency of Transactions by Month and Year')
plt.xlabel('Month')
plt.ylabel('Number of Transactions')
plt.xticks(range(1, 13))  # Ensuring all months are represented from 1 to 12
plt.legend(title='Year', loc='upper right')
plt.grid(True)
plt.show()
C:\Users\user\anaconda3\Lib\site-packages\seaborn\_oldcore.py:1119: FutureWarning: use_inf_as_na option is deprecated and will be removed in a future version. Convert inf values to NaN before operating instead.
  with pd.option_context('mode.use_inf_as_na', True):
C:\Users\user\anaconda3\Lib\site-packages\seaborn\_oldcore.py:1119: FutureWarning: use_inf_as_na option is deprecated and will be removed in a future version. Convert inf values to NaN before operating instead.
  with pd.option_context('mode.use_inf_as_na', True):
No description has been provided for this image
# Plotting histograms
numerical_columns = ['age', 'quantity', 'price', 'Sales']
customer_shopping_data[numerical_columns].hist(bins=30, figsize=(10, 6), layout=(2, 2))
plt.suptitle('Distribution of Numerical Columns')
plt.show()

# Boxplots to identify outliers
plt.figure(figsize=(10, 6))
for i, col in enumerate(numerical_columns, 1):
    plt.subplot(2, 2, i)
    sns.boxplot(x=customer_shopping_data[col])
    plt.title(f'Boxplot of {col}')
plt.tight_layout()
plt.show()
No description has been provided for this image
No description has been provided for this image
import pandas as pd

# Define age bins
bins = [0, 18, 30, 45, 60, 100]
labels = ['0-18', '18-30', '30-45', '45-60', '60+']

# Create age groups
customer_shopping_data['age_group'] = pd.cut(customer_shopping_data['age'], bins=bins, labels=labels)

# Plotting bar plot for age groups
plt.figure(figsize=(8, 5))
customer_shopping_data['age_group'].value_counts().sort_index().plot(kind='bar', color='k', edgecolor='yellow')
plt.title('Frequency of Age Groups')
plt.xlabel('Age Group')
plt.ylabel('Number of Customers')
plt.show()
No description has been provided for this image
Correlation Analysis
from matplotlib import colormaps
list(colormaps)
['magma',
 'inferno',
 'plasma',
 'viridis',
 'cividis',
 'twilight',
 'twilight_shifted',
 'turbo',
 'Blues',
 'BrBG',
 'BuGn',
 'BuPu',
 'CMRmap',
 'GnBu',
 'Greens',
 'Greys',
 'OrRd',
 'Oranges',
 'PRGn',
 'PiYG',
 'PuBu',
 'PuBuGn',
 'PuOr',
 'PuRd',
 'Purples',
 'RdBu',
 'RdGy',
 'RdPu',
 'RdYlBu',
 'RdYlGn',
 'Reds',
 'Spectral',
 'Wistia',
 'YlGn',
 'YlGnBu',
 'YlOrBr',
 'YlOrRd',
 'afmhot',
 'autumn',
 'binary',
 'bone',
 'brg',
 'bwr',
 'cool',
 'coolwarm',
 'copper',
 'cubehelix',
 'flag',
 'gist_earth',
 'gist_gray',
 'gist_heat',
 'gist_ncar',
 'gist_rainbow',
 'gist_stern',
 'gist_yarg',
 'gnuplot',
 'gnuplot2',
 'gray',
 'hot',
 'hsv',
 'jet',
 'nipy_spectral',
 'ocean',
 'pink',
 'prism',
 'rainbow',
 'seismic',
 'spring',
 'summer',
 'terrain',
 'winter',
 'Accent',
 'Dark2',
 'Paired',
 'Pastel1',
 'Pastel2',
 'Set1',
 'Set2',
 'Set3',
 'tab10',
 'tab20',
 'tab20b',
 'tab20c',
 'grey',
 'gist_grey',
 'gist_yerg',
 'Grays',
 'magma_r',
 'inferno_r',
 'plasma_r',
 'viridis_r',
 'cividis_r',
 'twilight_r',
 'twilight_shifted_r',
 'turbo_r',
 'Blues_r',
 'BrBG_r',
 'BuGn_r',
 'BuPu_r',
 'CMRmap_r',
 'GnBu_r',
 'Greens_r',
 'Greys_r',
 'OrRd_r',
 'Oranges_r',
 'PRGn_r',
 'PiYG_r',
 'PuBu_r',
 'PuBuGn_r',
 'PuOr_r',
 'PuRd_r',
 'Purples_r',
 'RdBu_r',
 'RdGy_r',
 'RdPu_r',
 'RdYlBu_r',
 'RdYlGn_r',
 'Reds_r',
 'Spectral_r',
 'Wistia_r',
 'YlGn_r',
 'YlGnBu_r',
 'YlOrBr_r',
 'YlOrRd_r',
 'afmhot_r',
 'autumn_r',
 'binary_r',
 'bone_r',
 'brg_r',
 'bwr_r',
 'cool_r',
 'coolwarm_r',
 'copper_r',
 'cubehelix_r',
 'flag_r',
 'gist_earth_r',
 'gist_gray_r',
 'gist_heat_r',
 'gist_ncar_r',
 'gist_rainbow_r',
 'gist_stern_r',
 'gist_yarg_r',
 'gnuplot_r',
 'gnuplot2_r',
 'gray_r',
 'hot_r',
 'hsv_r',
 'jet_r',
 'nipy_spectral_r',
 'ocean_r',
 'pink_r',
 'prism_r',
 'rainbow_r',
 'seismic_r',
 'spring_r',
 'summer_r',
 'terrain_r',
 'winter_r',
 'Accent_r',
 'Dark2_r',
 'Paired_r',
 'Pastel1_r',
 'Pastel2_r',
 'Set1_r',
 'Set2_r',
 'Set3_r',
 'tab10_r',
 'tab20_r',
 'tab20b_r',
 'tab20c_r',
 'rocket',
 'rocket_r',
 'mako',
 'mako_r',
 'icefire',
 'icefire_r',
 'vlag',
 'vlag_r',
 'flare',
 'flare_r',
 'crest',
 'crest_r']
# Correlation matrix
correlation_matrix = customer_shopping_data[['age', 'quantity', 'price', 'Sales']].corr()
sns.heatmap(correlation_matrix, annot=True, cmap='RdYlBu_r')
plt.title('Correlation Matrix')
plt.show()
No description has been provided for this image
Analyze Sales by Categories and Customer Segments Understand how sales differ by customer demographics and product categories.
# Sales by Gender
sales_by_gender = customer_shopping_data.groupby('gender')['Sales'].sum()
print("Sales by Gender:\n", sales_by_gender)


# Bar plot for Sales by Gender
plt.figure(figsize=(5, 3))
sns.barplot(x=sales_by_gender.index, y=sales_by_gender.values)
plt.title('Total Sales by Gender')
plt.xlabel('Gender')
plt.ylabel('Total Sales')
plt.show()
Sales by Gender:
 gender
Female    1.502071e+08
Male      1.012987e+08
Name: Sales, dtype: float64
No description has been provided for this image
# Sales by Category
sales_by_category = customer_shopping_data.groupby('category')['Sales'].sum()
print("Sales by Category:\n", sales_by_category)

# Bar plot for Sales by Category
plt.figure(figsize=(10, 4))
sns.barplot(x=sales_by_category.index, y=sales_by_category.values)
plt.title('Total Sales by Category')
plt.xlabel('Category')
plt.ylabel('Total Sales')
plt.xticks(rotation=45)
plt.show()
Sales by Category:
 category
Books              8.345529e+05
Clothing           1.139968e+08
Cosmetics          6.792863e+06
Food & Beverage    8.495351e+05
Shoes              6.655345e+07
Souvenir           6.358247e+05
Technology         5.786235e+07
Toys               3.980426e+06
Name: Sales, dtype: float64
No description has been provided for this image
# Sales trends over time
sales_trend = customer_shopping_data.groupby(customer_shopping_data['invoice_date'].dt.to_period('M'))['Sales'].sum()
sales_trend.plot(kind='line', figsize=(10, 6))
plt.title('Sales Trend Over Time')
plt.xlabel('Date')
plt.ylabel('Total Sales')
plt.show()
No description has been provided for this image
# Sales by Age Group
bins = [0, 18, 30, 45, 60, 100]
labels = ['0-18', '18-30', '30-45', '45-60', '60+']
customer_shopping_data['age_group'] = pd.cut(customer_shopping_data['age'], bins=bins, labels=labels)
sales_by_age_group = customer_shopping_data.groupby('age_group')['Sales'].sum()
print("Sales by Age Group:\n", sales_by_age_group)

# Bar plot for Sales by Age Group
plt.figure(figsize=(8, 5))
sns.barplot(x=sales_by_age_group.index, y=sales_by_age_group.values)
plt.title('Total Sales by Age Group')
plt.xlabel('Age Group')
plt.ylabel('Total Sales')
plt.show()
C:\Users\user\AppData\Local\Temp\ipykernel_7056\48382593.py:5: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
  sales_by_age_group = customer_shopping_data.groupby('age_group')['Sales'].sum()
C:\Users\user\anaconda3\Lib\site-packages\seaborn\categorical.py:641: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
  grouped_vals = vals.groupby(grouper)
Sales by Age Group:
 age_group
0-18      4397720.26
18-30    58649681.85
30-45    73134764.61
45-60    71804157.87
60+      43519469.66
Name: Sales, dtype: float64
No description has been provided for this image
What are the most popular categories, and how do they differ by gender?
popular_categories_by_gender = customer_shopping_data.groupby(['gender', 'category']).size()
print(popular_categories_by_gender) 
gender  category       
Female  Books               2906
        Clothing           20652
        Cosmetics           9070
        Food & Beverage     8804
        Shoes               5967
        Souvenir            3017
        Technology          2981
        Toys                6085
Male    Books               2075
        Clothing           13835
        Cosmetics           6027
        Food & Beverage     5972
        Shoes               4067
        Souvenir            1982
        Technology          2015
        Toys                4002
dtype: int64
# Convert the series to a DataFrame for easier plotting
df_popular_categories = popular_categories_by_gender.unstack(fill_value=0)

# Plotting the heatmap
plt.figure(figsize=(10, 6))
sns.heatmap(df_popular_categories, cmap='Oranges', annot=True, fmt='d', linewidths=0.5)
plt.title('Heatmap of Transactions by Category and Gender')
plt.xlabel('Category')
plt.ylabel('Gender')
plt.xticks(rotation=45)
plt.yticks(rotation=0)
plt.show()
No description has been provided for this image
What are the most common payment methods, and how do they vary across shopping malls?
payment_method_by_mall = customer_shopping_data.groupby(['shopping_mall', 'payment_method']).size().unstack(fill_value=0)
print(payment_method_by_mall)
payment_method     Cash  Credit Card  Debit Card
shopping_mall                                   
Cevahir AVM        2228         1779         984
Emaar Square Mall  2114         1696        1001
Forum Istanbul     2183         1750        1014
Istinye Park       4436         3422        1923
Kanyon             8853         6916        4054
Mall of Istanbul   8894         7019        4030
Metrocity          6625         5347        3039
Metropol AVM       4559         3521        2081
Viaport Outlet     2231         1721         962
Zorlu Center       2324         1760         991
# Reset index for plotting
payment_method_df = payment_method_by_mall.reset_index()

# Melt the DataFrame for seaborn
payment_method_melted = payment_method_df.melt(id_vars='shopping_mall', var_name='Payment Method', value_name='Number of Transactions')

# Plotting the grouped bar plot
plt.figure(figsize=(10, 6))
sns.barplot(data=payment_method_melted, x='shopping_mall', y='Number of Transactions', hue='Payment Method')
plt.title('Payment Methods by Shopping Mall')
plt.xlabel('Shopping Mall')
plt.ylabel('Number of Transactions')
plt.xticks(rotation=45)
plt.legend(title='Payment Method')
plt.show()
No description has been provided for this image
Which customers are repeat buyers, and how much do they contribute to total sales?
repeat_customers = customer_shopping_data['customer_id'].value_counts()
repeat_customers_contribution = customer_shopping_data[customer_shopping_data['customer_id'].isin(repeat_customers[repeat_customers > 1].index)].groupby('customer_id')['Sales'].sum()
print(repeat_customers_contribution)
Series([], Name: Sales, dtype: float64)
How does the average quantity purchased vary across different product categories?
avg_quantity_by_category = customer_shopping_data.groupby('category')['quantity'].mean()
print(avg_quantity_by_category)
category
Books              3.007830
Clothing           3.002813
Cosmetics          3.011525
Food & Beverage    2.996548
Shoes              3.011461
Souvenir           2.974795
Technology         3.006605
Toys               3.005948
Name: quantity, dtype: float64
What is the average age of customers by shopping mall?
avg_age_by_mall = customer_shopping_data.groupby('shopping_mall')['age'].mean()
print(avg_age_by_mall)
shopping_mall
Cevahir AVM          43.172511
Emaar Square Mall    43.561630
Forum Istanbul       43.537497
Istinye Park         43.383601
Kanyon               43.498966
Mall of Istanbul     43.440455
Metrocity            43.499301
Metropol AVM         43.212873
Viaport Outlet       43.298942
Zorlu Center         43.532217
Name: age, dtype: float64
customer_shopping_data
invoice_no	customer_id	gender	age	category	quantity	price	payment_method	invoice_date	shopping_mall	Sales	Year	Month	age_group
0	I138884	C241288	Female	28	Clothing	5	1500.40	Credit Card	2022-08-05	Kanyon	7502.00	2022	8	18-30
1	I317333	C111565	Male	21	Shoes	3	1800.51	Debit Card	2021-12-12	Forum Istanbul	5401.53	2021	12	18-30
2	I127801	C266599	Male	20	Clothing	1	300.08	Cash	2021-11-09	Metrocity	300.08	2021	11	18-30
3	I173702	C988172	Female	66	Shoes	5	3000.85	Credit Card	2021-05-16	Metropol AVM	15004.25	2021	5	60+
4	I337046	C189076	Female	53	Books	4	60.60	Cash	2021-10-24	Kanyon	242.40	2021	10	45-60
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
99452	I219422	C441542	Female	45	Souvenir	5	58.65	Credit Card	2022-09-21	Kanyon	293.25	2022	9	30-45
99453	I325143	C569580	Male	27	Food & Beverage	2	10.46	Cash	2021-09-22	Forum Istanbul	20.92	2021	9	18-30
99454	I824010	C103292	Male	63	Food & Beverage	2	10.46	Debit Card	2021-03-28	Metrocity	20.92	2021	3	60+
99455	I702964	C800631	Male	56	Technology	4	4200.00	Cash	2021-03-16	Istinye Park	16800.00	2021	3	45-60
99456	I232867	C273973	Female	36	Souvenir	3	35.19	Credit Card	2022-10-15	Mall of Istanbul	105.57	2022	10	30-45
99457 rows × 14 columns

 
