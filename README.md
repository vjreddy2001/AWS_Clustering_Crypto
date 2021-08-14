
# AWS_Clustering_Crypto

![clust](https://user-images.githubusercontent.com/83671629/128429354-88392356-5b4c-4384-ba1f-c7f3d873c0be.png)

### Background

In this scenario, I am Senior Manager at the Advisory Services team on a [Big Four firm](https://en.wikipedia.org/wiki/Big_Four_accounting_firms). One of my most important clients, a prominent investment bank, is interested in offering a new cryptocurrencies investment portfolio for its customers, however, they are lost in the immense universe of cryptocurrencies. They have asked me to help them make sense of it all by generating a report of what cryptocurrencies are available on the trading market and how they can be grouped using classification.  

To achieve this, I have put my new unsupervivsed learning and Amazon SageMaker skills into action by clustering cryptocurrencies and creating plots to present the results.

Using this tool the following main tasks were accomplished:

* **[Data Preprocessing](#Data-Preprocessing):** Prepared data for dimension reduction with PCA and clustering using K-Means.

* **[Reducing Data Dimensions Using PCA](#Reducing-Data-Dimensions-Using-PCA):** Reduced data dimension using the `PCA` algorithm from `sklearn`.

* **[Clustering Cryptocurrencies Using K-Means](#Clustering-Cryptocurrencies-Using-K-Means):** Predicted clusters using the cryptocurrencies data using the `KMeans` algorithm from `sklearn`.

* **[Visualizing Results](#Visualizing-Results):** Created plots and data tables to present the results.

* **[Optional Challenge](#Optional-Challenge):** Deployed the notebook to Amazon SageMaker.

---

### Files

* [crypto_clustering.ipynb](Starter_Files/crypto_clustering.ipynb)

---

### Instructions

#### Data Preprocessing

To start off I loaded the information about cryptocurrencies and performed data preprocessing tasks.  (this can be done by any one of the following methods to load the data):

1. Using the provided `CSV` file, creating a `Path` object and read the file data directly into a DataFrame named `crypto_df` using `pd.read_csv()` (I used this method).

2. Using the following `requests` library, retreive the necessary data from the following API endpoint from _CryptoCompare_ - `https://min-api.cryptocompare.com/data/all/coinlist`.  __HINT:__ use the 'Data' key from the json response, then transpose the DataFrame. Name your DataFrame `crypto_df`.

With the data loaded into a Pandas DataFrame, the following data preprocessing tasks were completed.

3. Kept only the necessary columns: 'CoinName','Algorithm','IsTrading','ProofType','TotalCoinsMined','TotalCoinSupply'

4. Kept only the cryptocurrencies that are trading.

5. Kept only the cryptocurrencies with a working algorithm.

6. Removed the `IsTrading` column.

7. Removed all cryptocurrencies with at least one null value.

8. Removed all cryptocurrencies that have no coins mined.

9. Dropped all rows where there are 'N/A' text values.

10. Stored the names of all cryptocurrencies in a DataFrame named `coins_name`, use the `crypto_df.index` as the index for this new DataFrame.

11. Removed the `CoinName` column.

12. Created dummy variables for all the text features, and store the resulting data in a DataFrame named `X`.

13. Used the [`StandardScaler` from `sklearn`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html) to standardize all the data of the `X` DataFrame. Remember, this is important prior to using PCA and K-Means algorithms.

#### Reducing Data Dimensions Using PCA

Used the [`PCA` algorithm from `sklearn`](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) to reduce the dimensions of the `X` DataFrame down to three principal components.

Once the data dimensions were reduced, created a DataFrame named `pcs_df` using columns names `"PC 1", "PC 2"` and `"PC 3"`;  used the `crypto_df.index` as the index for this new DataFrame.

The DataFrame looks as the following:

![pcs_df](https://user-images.githubusercontent.com/83671629/129432447-40ad337f-c3bd-403d-a35f-325367e1cd01.png)


#### Clustering Cryptocurrencies Using K-Means

Used the [`KMeans` algorithm from `sklearn`](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html) to cluster the cryptocurrencies using the PCA data.

Performed the following tasks:

1. Created an Elbow Curve to find the best value for `k` using the `pcs_df` DataFrame.

2. Once the best value for `k` was defined, ran the `Kmeans` algorithm to predict the `k` clusters for the cryptocurrencies data. Used the `pcs_df` to run the `KMeans` algorithm.

3. Created a new DataFrame named `clustered_df`, that includes the following columns `"Algorithm", "ProofType", "TotalCoinsMined", "TotalCoinSupply", "PC 1", "PC 2", "PC 3", "CoinName", "Class"`. You should maintain the index of the `crypto_df` DataFrames as is shown bellow.

    ![df2](https://user-images.githubusercontent.com/83671629/129432496-83aec795-2fd0-4f85-9a71-6395528e12cf.png)


#### Visualizing Results

Created some data visualization to present the final results. The following tasks were performed:

1. Created a 3D-Scatter using Plotly Express to plot the clusters using the `clustered_df` DataFrame. The following parameters were included on the plot: `hover_name="CoinName"` and `hover_data=["Algorithm"]` to show this additional info on each data point.
![plot](https://user-images.githubusercontent.com/83671629/129432559-a983d4b4-4b96-4470-aa43-a3389f699569.png)

2. Used `hvplot.table` to create a data table with all the current tradable cryptocurrencies. The table had the following columns: `"CoinName", "Algorithm", "ProofType", "TotalCoinSupply", "TotalCoinsMined", "Class"`
![hvplot](https://user-images.githubusercontent.com/83671629/129432561-ca44b4ff-cace-4033-ba64-acc977028972.png)

3. Created a scatter plot using `hvplot.scatter`, to present the clustered data about cryptocurrencies having `x="TotalCoinsMined"` and `y="TotalCoinSupply"` to contrast the number of available coins versus the total number of mined coins. Used the `hover_cols=["CoinName"]` parameter to include the cryptocurrency name on each data point.
![bokeh_plot](https://user-images.githubusercontent.com/83671629/129432568-473ddfe2-14ac-44fe-9d3e-ed3269b38f11.png)


#### Complementary Resources

* [Altair visualization library website](https://altair-viz.github.io/).

* [Simple line chart using Altair](https://altair-viz.github.io/gallery/simple_line_chart.html).

* [Simple Scatter Plot with Tooltips using Altair](https://altair-viz.github.io/gallery/scatter_tooltips.html)

* [Color customization on Altair](https://github.com/altair-viz/altair/issues/921#issuecomment-395416682)

* [Printing all rows from a DataFrame](https://stackoverflow.com/a/30691921/4325668)

* [Install External Libraries and Kernels in Amazon SageMaker Notebook Instances](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-add-external.html)

