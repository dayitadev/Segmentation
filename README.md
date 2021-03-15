# Segmentation of Customers with K-Means and PCA
This analysis was done using the following dataset: https://www.kaggle.com/mkechinov/ecommerce-events-history-in-cosmetics-shop?select=2020-Jan.csv

The pupose of this analysis is to understand customers' browsing behaviours on an e-commerce website, and attempt to segment them into groups using the K-Means package from Python's sklearn package.

## Exploratory Data Analysis
First, I attempted to understand the data and the events. The dataset contains a series of events, with user id, the event type, the product associated with the event, and the price of the product. Summarizing the dataset showed that there are roughly 400k unique users (customers), and 970k sessions.

Another key aspect of the exploratory analysis was understanding the key events and the distribution of those events across users. This also allowed a better understanding of the funnel metrics on this website in the given month. I looked at the distribution of the following events:
1. Page Views - user visited a page
2. Cart Events - user added a product to the cart
3. Cart Removals - user removed the product from cart
4. Purchases - user purchased the product

As expected, page views accounts for roughly 48% of the events, and purchases account for 6%. The conversion rate from views to purchase is ~13%.

## Feature Prep
I then aggregated the browsing features to a customer level. I was interested in the following browsing behavious:
*   Page views
*   Sessions per user
*   Number of products purchased
*   Number of products added to shopping car
*   Number of products removed from shopping cart
*   Total spend
*   Spend per visit
*   Total shopping cart actions per visit

I also compiled brand features but soon realized that there are many over 50% of the rows for the brands column are null, so this data would not be sufficient features to apply to the entire customer base.

I noticed the there were a lot of null values in the product and brand columns, so I refrained from leveraging these as part of the features.

## K-Means - First Attempt
I fed the features dataset into the K-Means model with 50,000 rows of data. I ran it across multiple k to generate 2 metrics:
* Silhoutte score
* Within Cluster Sum of Squares 

Since a lot of customers were being classified within one segment, I decided to run the model through by assigning components using PCA.

## K-Means - using PCA
I first assigned components using PCA. I fed the features into the PCA package and generated a variance plot to see how much of the variance is explained with each additional component. This showed that 3 was the sweet spot (~80% of the variance explaned).

I then fed the scored PCA dataset into k-means and repeated the process from the first attempt. This time, I chose to create 4 clusters. The majority of customers were being grouped into one segment, but the spending behaviour varied quite a bit amongst the groups. For instance, Segment 3 appear to browse through the website more frequently, but Segment 2 makes more purchases. In a business context, we could classify Segment 3 as window shoppers and Segment 2 as high spenders.

## Model Results and Conclusion
Looking at the segmentation results, it's clear that a lot of customers are being grouped into a single segment. My hypothesis around this is that the features used in this dataset were not enough to clearly isolate the distinctive groups of customers (particularly within the larger segment). Ideally, I would include a variety of attributes like demographics and types of products purchased. These additional features might help distinguish the groups within the larger segments further.

## Sources used:
A large portion of this analysis was inspired from https://towardsdatascience.com/segmentation-of-online-shop-customers-8c304a2d84b4 (particularly the first attempt at K-Means)
