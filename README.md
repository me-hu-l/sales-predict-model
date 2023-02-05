# sales-predict-model

### Kaggle Competition: [Predict Future Sales](https://www.kaggle.com/c/competitive-data-science-predict-future-sales)

### Data Description

#### File descriptions
```
- sales_train.csv - the training set. Daily historical data from January 2013 to October 2015.
- test.csv - the test set. You need to forecast the sales for these shops and products for November 2015.
- sample_submission.csv - a sample submission file in the correct format.
- items.csv - supplemental information about the items/products.
- item_categories.csv  - supplemental information about the items categories.
- shops.csv- supplemental information about the shops.
```

### Data fields
```
- ID - an Id that represents a (Shop, Item) tuple within the test set
- shop_id - unique identifier of a shop
- item_id - unique identifier of a product
- item_category_id - unique identifier of item category
- item_cnt_day - number of products sold. You are predicting a monthly amount of this measure
- item_price - current price of an item
- date - date in format dd/mm/yyyy
- date_block_num - a consecutive month number, used for convenience. January 2013 is 0, February 2013 is 1,..., October 2015 is 33
- item_name - name of item
- shop_name - name of shop
- item_category_name - name of item category
```

### Exploratory Data Analysis

More information can be found on [EDA notebook](https://github.com/me-hu-l/sales-predict-model/blob/main/predict-future-sales-EDA.ipynb)
Basic data analysis is done, including plotting sum and mean of item_cnt_day for each month to find some patterns, exploring missing values, inspecting test set …

Here are few things interesting I found from doing EDA:

- Number of sold items declines over the year
- There are peaks in November and similar item count zic-zac behaviors in June-July-August. This inspires me to look up Russia national holiday and create a Boolean holiday features. More information can be found in ‘Feature Engineering’ section
- Data has no missing values
- Some interesting information from test set analysis:
  - Not all shop_id in training set are used in test set. Test set excludes following shops (but not vice versa): [0, 1, 8, 9, 11, 13, 17, 20, 23, 27, 29, 30, 32, 33, 40, 43, 51, 54]
  - Not all item in train set are in test set and vice versa
  - In test set, a fixed set of items (5100) are used for each shop_id, and each item only appears one per each shop. This possibly means that items are picked from a generator, which will result in lots of 0 for item count. Therefore, generating all possible shop-item pairs for each month in train set and assigning missing item count with 0 makes sense.
  
 ### Feature Engineering
 
#### 1. Generate all shop-item pairs and Mean Encoding

Item counts for each shop-item pairs per month (‘target’). I also generated sum and mean of item counts for each shop per month (‘shop_block_target_sum’,’shop_block_target_mean’), each item per month (‘item_block_target_sum’,’item_block_target_mean’, and each item category per month (‘item_cat_block_target_sum’,’item_cat_block_target_mean’)

#### 2. Generate lag features

Lag features are values at prior time steps. I am generating lag features based on ‘item_cnt’ and grouped by ‘shop_id’ and ‘item_id’ . Time steps are: 1,2 and 3 months.

#### 3. Holiday Boolean features

I look up few Russia national holidays and created few 5 more features: December (to mark December), Newyear_Xmas (for January), Valentine_Menday (February), Women_Day (March), Easter_Labor (April). This might help boosting my score a little since December feature seems to be helpful

### Training:

#### LightGBM:

yet to be done
