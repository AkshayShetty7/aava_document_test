# **1. Machine Learning Fundamentals** 

A concise six-page guide to essential machine learning concepts, workflows, algorithms, and MLOps. 

### **What is Machine Learning?** 

Machine learning (ML) is a branch of artificial intelligence in which algorithms learn patterns from data and use those patterns to make predictions, classifications, or decisions. 

### **Core workflow** 

A typical ML workflow is: define the problem → collect data → clean and explore data → engineer features → split data → train a model → evaluate it → tune it → deploy and monitor it. 

### **Features and targets** 

Features are input variables used by a model. The target, label, or response is what the model is trying to predict. For example, house size and location can be features, while house price is the target. 

### **Training vs inference** 

Training is the process of learning model parameters from historical data. Inference is using the trained model to produce predictions for new data. 

### **Common ML tasks** 

Regression predicts continuous values; classification predicts categories; clustering groups similar observations; dimensionality reduction compresses data while preserving useful structure; anomaly detection identifies unusual observations. 

### **Why ML works** 

The model attempts to learn a useful relationship between inputs and outputs that generalizes beyond the examples used during training. Generalization—not memorization—is the central goal. 

Machine Learning Concepts 

Page 1 

## **2. Supervised & Unsupervised Learning** 

### **Supervised learning** 

Supervised learning uses labeled examples. The model learns from pairs of inputs and known targets. Examples include predicting loan default, classifying images, and forecasting sales. 

### **Regression** 

Regression predicts numerical values. Common algorithms include linear regression, decision trees, random forests, gradient boosting, and neural networks. 

### **Classification** 

Classification predicts discrete classes such as fraud/not fraud or positive/negative. Logistic regression, decision trees, support vector machines, and neural networks are common choices. 

### **Unsupervised learning** 

Unsupervised learning works with data without explicit target labels. The goal may be to discover groups, structure, or compact representations. 

### **Clustering** 

K-means, hierarchical clustering, and density-based methods can group observations according to similarity. The usefulness of clusters depends heavily on the chosen representation and distance measure. 

### **Semi-supervised learning** 

Semi-supervised methods combine a smaller amount of labeled data with a larger amount of unlabeled data. This can be useful when labeling is expensive. 

Machine Learning Concepts 

Page 2 

## **3. Data Preparation & Feature Engineering** 

### **Data quality** 

Real-world data may contain missing values, duplicates, inconsistent formats, outliers, measurement errors, and leakage. Data preparation often requires more effort than model selection. 

### **Train, validation, and test sets** 

The training set is used to fit the model. Validation data helps choose models and hyperparameters. The test set should be held back for a final, unbiased estimate of performance. 

### **Scaling** 

Standardization and normalization can help algorithms that depend on feature magnitude, including many linear models, distance-based methods, and neural networks. Tree-based models generally need less scaling. 

### **Categorical variables** 

Categorical features can be represented using methods such as one-hot encoding, ordinal encoding, or learned embeddings, depending on the model and meaning of the categories. 

### **Feature engineering** 

Useful transformations can expose patterns more clearly—for example, extracting day-of-week from a date, creating ratios, aggregating transaction histories, or transforming highly skewed variables. 

### **Data leakage** 

Leakage occurs when information unavailable at prediction time accidentally enters training features or preprocessing. It can produce excellent validation scores and poor real-world performance. 

Machine Learning Concepts 

Page 3 

## **4. Model Training, Loss & Optimization** 

### **Model parameters** 

Parameters are values learned from data, such as coefficients in linear regression or weights in a neural network. Hyperparameters are settings chosen outside the training process, such as tree depth or learning rate. 

### **Loss functions** 

A loss function measures how far predictions are from desired outcomes. Mean squared error is common for regression; cross-entropy is widely used for classification. 

### **Optimization** 

Training attempts to minimize loss. Gradient descent updates parameters in the direction that reduces the loss. Variants such as stochastic gradient descent and Adam are widely used. 

### **Overfitting** 

Overfitting occurs when a model learns training-specific noise or details and performs poorly on unseen data. A model can have low training error but high validation error. 

### **Underfitting** 

Underfitting occurs when the model is too simple or insufficiently trained to capture important patterns. It often produces poor performance on both training and validation data. 

### **Regularization** 

Regularization discourages overly complex solutions. Examples include L1 and L2 penalties, dropout in neural networks, early stopping, data augmentation, and restricting tree complexity. 

Machine Learning Concepts 

Page 4 

## **5. Evaluation & Model Selection** 

### **Why evaluation matters** 

A model should be evaluated using metrics that match the real business or scientific objective. A single accuracy number can hide important failure modes. 

### **Classification metrics** 

Accuracy measures overall correctness. Precision asks how many predicted positives were actually positive. Recall asks how many actual positives were found. F1 balances precision and recall. 

### **Regression metrics** 

MAE measures average absolute error; MSE emphasizes larger errors; RMSE expresses error on the original scale; R² describes variance explained relative to a baseline. 

### **Confusion matrix** 

A confusion matrix summarizes true positives, true negatives, false positives, and false negatives. It is especially useful for understanding asymmetric costs of mistakes. 

### **Cross-validation** 

Cross-validation repeatedly trains and evaluates models on different data partitions. It provides a more robust estimate when datasets are limited, although it must be designed carefully to avoid leakage. 

### **Model selection** 

Consider accuracy, interpretability, latency, memory, training cost, inference cost, fairness, robustness, and maintainability—not only benchmark performance. 

Machine Learning Concepts 

Page 5 

## **6. Deep Learning, Deployment & MLOps** 

### **Neural networks** 

Neural networks consist of layers of learned transformations. Deep learning uses many layers and has achieved strong results in computer vision, speech, natural language processing, and multimodal tasks. 

### **Backpropagation** 

Backpropagation efficiently calculates how the loss changes with respect to network parameters. An optimizer then uses these gradients to update the parameters. 

### **Deployment** 

A trained model can be deployed through batch jobs, APIs, embedded applications, or real-time services. Production design must account for latency, throughput, reliability, security, and cost. 

### **MLOps** 

MLOps applies software engineering and operational practices to ML: versioning data and models, reproducible training, automated testing, deployment pipelines, monitoring, and rollback. 

### **Model monitoring** 

Production performance can change because data distributions, user behavior, or relationships between features and targets change. Monitor input distributions, prediction quality, latency, errors, and drift. 

### **Key takeaway** 

Successful machine learning is an end-to-end discipline. Strong results require appropriate problem definition, reliable data, sound evaluation, suitable models, careful deployment, and continuous monitoring. 

Machine Learning Concepts 

Page 6 

