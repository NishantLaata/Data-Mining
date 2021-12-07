# Data-Mining HW3
import pandas as pd;

df = pd.read_csv(r'C:\Users\W10\Desktop\is 733 dATAMINING\Homework 3\faithful.csv')

#scatter plot Q1:
---------------------------------------------------
Q1 a:
import pandas as pd
from matplotlib import pyplot as plt
plt.rcParams["figure.figsize"] = [7.00, 3.50]
plt.rcParams["figure.autolayout"] = True
columns = ["eruptions", "waiting"]
df = pd.read_csv(r'C:\Users\W10\Desktop\is 733 dATAMINING\Homework 3\faithful.csv', usecols=columns)
print("Contents in csv file:\n", df)
plt.scatter(df.eruptions, df.waiting)
plt.show()

#k_means algorithm from scratch Q2:
 
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from sklearn.decomposition import PCA



------------------------------------------------------------------

#Q2a

class KMeans:
    """K-Means Class"""

    def __init__(self, data, num_clusters,init,max_iter,n_init,random_state):
        """K-Means class constructor.
        :param data: training dataset.
        :param num_clusters: number of cluster into which we want to break the dataset.
        """
        self.data = data
        self.num_clusters = num_clusters

    def train(self, max_iterations):
        """Function performs data clustering using K-Means algorithm
        :param max_iterations: maximum number of training iterations.
        """

        # Generate random centroids based on training set.
        centroids = KMeans.centroids_init(self.data, self.num_clusters)

        # Init default array of closest centroid IDs.
        num_examples = self.data.shape[0]
        closest_centroids_ids = np.empty((num_examples, 1))

        # Run K-Means.
        for _ in range(max_iterations):
            # Find the closest centroids for training examples.
            closest_centroids_ids = KMeans.centroids_find_closest(self.data, centroids)

            # Compute means based on the closest centroids found in the previous part.
            centroids = KMeans.centroids_compute(
                self.data,
                closest_centroids_ids,
                self.num_clusters
            )

        return centroids, closest_centroids_ids

    @staticmethod
    def centroids_init(data, num_clusters):
        """Initializes num_clusters centroids that are to be used in K-Means on the dataset X
        :param data: training dataset.
        :param num_clusters: number of cluster into which we want to break the dataset.
        """

        # Get number of training examples.
        num_examples = data.shape[0]

        # Randomly reorder indices of training examples.
        random_ids = np.random.permutation(num_examples)

        # Take the first K examples as centroids.
        centroids = data[random_ids[:num_clusters], :]

        # Return generated centroids.
        return centroids
   
    @staticmethod
    def centroids_find_closest(data, centroids):
        """Computes the centroid memberships for every example.
        Returns the closest centroids in closest_centroids_ids for a dataset X where each row is
        a single example. closest_centroids_ids = m x 1 vector of centroid assignments (i.e. each
        entry in range [1..K]).
        :param data: training dataset.
        :param centroids: list of centroid points.
        """

        # Get number of training examples.
        num_examples = data.shape[0]

        # Get number of centroids.
        num_centroids = centroids.shape[0]

        # We need to return the following variables correctly.
        closest_centroids_ids = np.zeros((num_examples, 1))

        # Go over every example, find its closest centroid, and store
        # the index inside closest_centroids_ids at the appropriate location.
        # Concretely, closest_centroids_ids(i) should contain the index of the centroid
        # closest to example i. Hence, it should be a value in the range 1...num_centroids.
        for example_index in range(num_examples):
            distances = np.zeros((num_centroids, 1))
            for centroid_index in range(num_centroids):
                distance_difference = data[example_index, :] - centroids[centroid_index, :]
                distances[centroid_index] = np.sum(distance_difference ** 2)
            closest_centroids_ids[example_index] = np.argmin(distances)

        return closest_centroids_ids

    @staticmethod
    def centroids_compute(data, closest_centroids_ids, num_clusters):
        """Compute new centroids.
        Returns the new centroids by computing the means of the data points assigned to
        each centroid.
        :param data: training dataset.
        :param closest_centroids_ids: list of closest centroid ids per each training example.
        :param num_clusters: number of clusters.
        """

        # Get number of features.
        num_features = data.shape[1]

        # We need to return the following variables correctly.
        centroids = np.zeros((num_clusters, num_features))

        # Go over every centroid and compute mean of all points that
        # belong to it. Concretely, the row vector centroids(i, :)
        # should contain the mean of the data points assigned to
        # centroid i.
        for centroid_id in range(num_clusters):
            closest_ids = closest_centroids_ids == centroid_id
            centroids[centroid_id] = np.mean(data[closest_ids.flatten(), :], axis=0)

        return centroids
 
#Load Data
    data = pd.read_csv(r'C:\Users\W10\Desktop\is 733 dATAMINING\Homework 3\faithful.csv')
    pca = PCA(2)
  
    #Transform the data
    df = pca.fit_transform(data)
 
    #Applying our function
    label = kmeans(df,2,1000)
 
    #Visualize the results
 
    u_labels = np.unique(label)
    for i in u_labels:
        plt.scatter(df[label == i , 0] , df[label == i , 1] , label = i)
        plt.scatter(centroids[:,0] , centroids[:,1] , s = 80, color = 'k')
    plt.legend()
    

    plt.show()
    
------------------------------------------------------------------------------------------------

Q2c:
import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
import random as rd
import matplotlib.pyplot as plt
from math import sqrt

# Input data files are available in the "../input/" directory.
# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory

import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

# Any results you write to the current directory are saved as output.

data = pd.read_csv(r'C:\Users\W10\Desktop\is 733 dATAMINING\Homework 3\faithful.csv')
data.head()

X = data[["eruptions", "waiting"]]
# Visualize data point
plt.scatter(X["eruptions"], X["waiting"], c="blue")
plt.xlabel("eruptions")
plt.ylabel("waiting")
plt.show()


K=2

# select random observation as a centriod
Centroids = (X.sample(n=K))
plt.scatter(X["eruptions"], X["waiting"], c="blue")
plt.scatter(Centroids["eruptions"], Centroids["waiting"], c="red")
plt.xlabel("eruptions")
plt.ylabel("waiting")
plt.show()


Centroids

diff = 1
j=0

while(diff!=0):
    XD=X
    i=1
    for index1, row_c in Centroids.iterrows():
        ED=[]
        for index2, row_d in XD.iterrows():
            d1 = (row_c["eruptions"]-row_d["eruptions"])**2
            d2 = (row_c["waiting"]-row_d["waiting"])**2
            d = sqrt(d1+d2)
            ED.append(d)
        X[i] = ED
        i = i+1
   
    C = []
    for index, row in X.iterrows():
        min_dist=row[1]
        pos=1
        for i in range(K):
            if row[i+1] < min_dist:
                min_dist = row[i+1]
                pos = i+1
        C.append(pos)
    X["Cluster"]=C
    Centroids_new = X.groupby(["Cluster"]).mean()[["waiting", "eruptions"]]
    if j == 0:
        diff = 1
        j = j+1
    else:
        diff = (Centroids_new['waiting'] - Centroids['waiting']).sum() + (Centroids_new['eruptions'] - Centroids['eruptions']).sum()
        print(diff.sum())
    Centroids = X.groupby(["Cluster"]).mean()[["waiting","eruptions"]]
   
   
color=['blue','green','cyan']
for k in range(K):
    data=X[X["Cluster"]==k+1]
    plt.scatter(data["eruptions"],data["waiting"],c=color[k])
plt.scatter(Centroids["eruptions"],Centroids["waiting"],c='red')
plt.xlabel('eruptions')
plt.ylabel('waiting')
plt.show()


wcss = []
for i in range(1,11):
    kmeans = KMeans(n_clusters = i, init='k-means++', max_iter=100, n_init = 10, random_state = 0)
    kmeans = KMeans(n_clusters = i, init='k-means++', max_iter=100, n_init = 10, random_state = 0)
    kmeans.fit(X)
    wcss.append(kmeans.inertia_)

# converting the results into a dataframe and plotting them
plt.plot(range(1, 11), wcss)
plt.title('objective vs iteration')
plt.xlabel('iteration')
plt.ylabel('objective')
