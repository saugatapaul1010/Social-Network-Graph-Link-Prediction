In this case study given a directed social graph, the task is to predict missing links to recommend friends/followers to new users. The dataset was taken from the facebook recruiting challenge hosted at kaggle at this link: https://www.kaggle.com/c/FacebookRecruiting. The dataset only has source node information and destinatin node information. Here each node represents an user. Basically a link between source to destination means that a user follows another user. 

In the given data, only those source and destination nodes are given for which an edge exists. There is no information about the nodes whcih does not have an edge between them. So, in order to map this problem to a binary classification problem of whether or not an edge exists in the graph, we need to create training and testing sample which has a class label of 0 (0 means that there are no edges present between source to destination). 

NOTE: In the given dataset, we have roughly 9.43 million edges and 1.93 million nodes (vertices or users). For the given data, all the links are present and hence the class label will be 1. However, for classification we also need 0 class labels. How do we generate the 0 class labels?

1. Create the same number of 0 labeled pairs of vertices that we have for class 1 labeled pairs of vertices.
2. Randomly sample a pair of vertices
3. Check if the path length is greater than 2.
4. Check if no edge connection exists between the pairs of vertices.
5. If both the above conditions are satisfied then we will have a new pair of edges which will have a class label 0.


Coming to business constraints, there are no low latency requirements. We need to use probability estimates to predict edges between two nodes. This will give us more interpretability in terms of which edge connections are more important. The metric we have chosen is F1 score and binary confusion matrix. 

We curated features like number of followers and followees of each node, whether or not a node is followed back by any other nodes, page rank of individual nodes, katz score, jaccard index, preferential attachment,svd features, svd dot features, adar index and so on. There were a total of 59 features on which we train and test our model. 

There is not timestamp provided for this data. Ideally, if you think about it, we have the dataset for a given time stap t. However, the graph is evolving and changing over time. After 30 days the edge connections might change, because people might have new followers and they may even start to follow new people. In the real word, we would split the data according to time. But, since we do not have any information about time stamp, we will split the data randomly in 80:20 ratio. 80% for training data and 20% for cross validation data.

Hyper-parameter tuning using cross validation is done for Random Forest classifier. Feature importance and feature score is calculated for all the final sets of features. A further hyper-parameter tuning was done using cross validation using xgbclassifier, as well as obtaining the best set of hyperparameters using random search cv. The best F1 score we obtained using hyperparameter tuning is 0.932. The train F1 score of 0.97 suggests that there exists a very very small amount of overfitting. However, that's ok. The difference is less than 0.05. 

In order to improve the f1 score, we can add more graph based features and perform more aggresive hyperparameter tuning. 


