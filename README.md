# Customer Segmentation and Analysis

![Tableau Customer Analysis Dashboard](https://github.com/DarkoMonzioCompagnoni/media/blob/main/Customer%20analysis%20_%20Tableau%20Public.gif)
[See the dashboard on Tableau Public](https://public.tableau.com/app/profile/darko.monzio.compagnoni/viz/Customeranalysis_16866582899760/Customeranalysis)


The dataset appears to contain information about various orders placed by customers. Here are the columns in the dataset:

- **order_id**: The ID of the order
- **order_date**: The date of the order
- **status**: The status of the order (e.g., 'received', 'complete')
- **item_id**: The ID of the item in the order
- **sku**: The Stock Keeping Unit, which is a unique identifier for each distinct product and service that can be purchased
- **qty_ordered**: The quantity of the item ordered
- **price**: The price of the item
- **value**: Not sure, needs further investigation (possibly the total value of the item ordered, calculated as price * qty_ordered)
- **discount_amount**: The amount of discount on the order
- **total**: Not sure, needs further investigation (possibly the final total value of the order after applying the discount)
- **...:** There are more columns, including customer-related information such as Name, Address, City, State, Zip code, Region, and User Name, and product-related information such as product name, brand, category, subcategory, etc. Also, some columns are related to shipping and billing.

For customer segmentation, we often use RFM analysis - Recency, Frequency, and Monetary value of the purchases. However, we can also include other factors such as the preferred category of products, average discount received, and region.

Here are the steps we will follow:

- **Data Cleaning**: Check for missing values and outliers.
- **Feature Engineering**: Create new features that capture customer behavior.
- **Data Standardization**: Standardize the data so that all features have equal weight.
- **Determine the optimal number of clusters**: Use the Elbow Method.
- **Apply Clustering**: We will use the K-Means clustering algorithm, which is commonly used for customer segmentation.
- **Analyze the Clusters**: Understand the characteristics of each customer group.

Let's start with data cleaning and feature engineering.

The dataset does not have any missing values which is great.

From the summary statistics, we can see that there are some outliers in the data. For instance, the maximum quantity ordered is 501 and the maximum price is 101,262.59, which are significantly higher than their respective 75th percentiles. These outliers could potentially skew our clustering algorithm. However, in this case, these outliers might represent a certain segment of customers (e.g., bulk buyers or customers who buy expensive items), so we will not remove these outliers.

Now, let's move on to feature engineering. We will create the following features for each customer:

- **Recency**: Number of days between the customer's latest order and the last date in the dataset.
- **Frequency**: Number of orders made by the customer.
- **Monetary**: Total money spent by the customer.
- **Average Discount**: Average discount received by the customer.
- **Preferred Region**: The most frequent region where the customer orders from.
- **Preferred Category**: The most frequent product category that the customer orders.

### Data Standardization
Next, we need to standardize the data. Standardization is necessary for clustering because it puts different variables on the same scale. This is important because clustering algorithms like k-means use distance-based measures to determine the similarity between points. If we don't standardize, the variable with a larger scale will dominate the others.

However, before standardizing, we need to handle the categorical variables in our data. The k-means algorithm isn't directly applicable to categorical data, as it relies on calculating distances. We'll transform the categorical variables using a method called one-hot encoding. This will create binary (0 or 1) columns indicating the presence of each category.

Then, we will perform the standardization and clustering.

### Determining the perfect number of clusters

![](https://github.com/DarkoMonzioCompagnoni/media/blob/main/Elbow%20method%20for%20clustering.png)

The elbow method plot above shows the sum of squared distances of samples to their closest cluster center for different values of k (the number of clusters).

In the plot, we're looking for the "elbow" where the rate of decrease sharply shifts, indicating the optimal number of clusters. This is often somewhat subjective, but it appears that the "elbow" in this plot is at k=3.

Therefore, we will proceed with dividing our customers into 3 segments. Let's apply the k-means clustering with k=3 to our customer data.

### Clustering

The customers have been successfully segmented into 3 clusters. Now, let's analyze these clusters to understand the characteristics of each customer group. We'll look at the mean values of the features and the distribution of categorical features across each cluster. This will help us profile the customer segments and understand their behavior.

Here are the characteristics of the three customer segments (clusters) we identified:

- **Cluster 0 (161 customers)**: These customers are the highest spenders with an average total spend of approximately 271,863. They also have the highest average number of orders (frequency), with about 103 orders on average. They have the most recent purchases (average of about 57 days ago), but they get a comparatively smaller average discount (about 3.92%). Their preferred shopping category is 'Mobiles & Tablets' and the majority of them are from the South region.
- **Cluster 1 (48,480 customers)**: This is the largest group of customers. They have the lowest average total spend (about 1,768) and the lowest average number of orders (about 2.16). Their last purchase was about 188 days ago on average. These customers get the lowest average discount (about 0.50%). Their preferred shopping category is 'Men's Fashion' and the majority of them are also from the South region.
- **Cluster 2 (15,607 customers)**: These customers have a moderate average total spend (about 6,673) and a moderate average number of orders (about 5.14). Their last purchase was about 210 days ago on average. However, these customers get the highest average discount (about 15.31%). Like Cluster 0, their preferred shopping category is 'Mobiles & Tablets' and the majority are from the South region.

These customer segments can be loosely described as follows:

- **Segment 0**: High-spending, frequent shoppers - They make a lot of purchases, spend a lot of money, and have shopped recently. They buy mostly from 'Mobiles & Tablets' category.
- **Segment 1**: Low-spending, infrequent shoppers - They make fewer purchases, spend less money, and have not shopped recently. They buy mostly from 'Men's Fashion' category.
- **Segment 2**: Moderate-spending, discount lovers - They make a moderate number of purchases, spend a moderate amount of money, get the highest discounts, and have not shopped recently. They buy mostly from 'Mobiles & Tablets' category.

Please note that these are simplified interpretations. Each cluster represents a multidimensional segment in the customer space. Also, the 'South' region appears to be the most common for all clusters, which may be due to the overall distribution of customers in the dataset.

