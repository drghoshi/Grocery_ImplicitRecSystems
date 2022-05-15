# Content-Based Grocery Recommendation Systems :apple:

Neeraj Joshi & Abhishsek Dendukri \
NYU Center for Data Science

# Introduction:

This repository contains code and data to help identify the best approaches to incorporate content-based features into an implicit-feedback based recommendation system. We'll be using a dataset that Instacart has released, found [here](https://tech.instacart.com/3-million-instacart-orders-open-sourced-d40d29ead6f2),
containing ~3 million online grocery orders. Given a customer's previous orders, our goal is to predict the following order based on historical purchase information as well as product centered information.

# Background

Given how diverse and specific eating habits have become over the past few decades, it is imperative to serve recommendations that take into consideration dietary restrictions, personal preference as well as contextual information on products currently in cart. Since taste and preference in food are highly complex attributes, it makes sense that non-linear methods may learn the relationships more accurately. For this reason, we decided to combine neural-based approaches will collaborative filtering based approaches.

# Data

Our dataset was originally part of a competition to achive best in class performance on classifying re-order. In other words, given a users past purchased products, the goal is to identify if that user will buy those products again in their subsequent order. Our application is slightly different in that we want to not only predict which items are reorders, but also identify new products that the user will buy in their next order. Following are some quick statistics about the orders dataset:
* 206209 Customers
* 49688 Products
* 34569106 Orders
* 10.12 Products/Order
* 16.59 Orders/User


| ![orders_snapshot.png](https://github.com/drghoshi/Grocery_ImplicitRecSystems/blob/main/Images/orders_snapshot.png) | 
|:--:| 
| *A snapshot of the orders dataset. Above you can see the first two orders for user_id 1.* |

# File Structure & Guide

```
Grocery_ImplicitRecSystems/
├─ Data /
│  ├─ order_products__prior.csv (training order set)
│  ├─ order_products__train.csv (validation order set)
│  ├─ np_instacart_product_vectors.pkl (word2vec vectors)
│  ├─ products.csv (product information set)
├─ Images/
│  ├─ final_results.png
│  ├─ n_nearest_neighbors_weighted_auc.png
│  ├─ ncf_architecture.jpg
│  ├─ n_nearest_neighbors.png
│  ├─ orders_snapshot.png
├─ Code/
│  ├─ Pipeline_Main_Stage1.ipynb (1st stage ALS Model)
│  ├─ Pipeline_Main_Stage2_DataPrep.ipynb (Candidate generation data prep)
│  ├─ Pull_NewTrainingData.ipynb (negative sampling)
│  ├─ Examine_ProductVectors (word2vec embedding exploration and clustering)
│  ├─ NCF_ArchitectureTests_NeuMF_Stage2.ipynb (training and evaluation of 2nd stage model)
├─ .gitignore
├─ README.md
```

# Implementation

One of the main difficulties of employing an implicit-based recommendation model is that there is no true
notion of a negative. If there are no historical interactions between a user and product, we’re unable to confirm that
the user did not like the product, since they may not have even known it existed. This makes the challenge of
predicting the propensity of a user buying a particular product extremely difficult, since we can only know what a
user likes. 

To deal with this issue, there are a number of methods one can employ to make sure the negative samples
drawn from the data distribution are more indicative of what a user may not have truly enjoyed. A series of
techniques, called heuristic-based negative sampling methods, involve either randomly sampling from the entire
product distribution, or popularity-based sampling, allowing top-selling products to have a higher probability of
being selected than less-popular products. 

These methods work, but they can be problematic due to the potential of
picking very niche products with no history or picking items that are popular in a global sense and fail to pick up on
various personas of shoppers. The other set of techniques, called model-based negative sampling methods, involve
using machine learning models to predict the propensity of a user purchasing an item, and including items which
score high but are never purchased by the user. These products generally fall in line with other products the user has
bought in the past, but are never purchased, indicating a negative relationship the model should learn to differentiate
from the specific products the user enjoys.

We decided to try 3 techniques to deal with this lack of true negative samples. All three techniques are predicated on going from a singluar traditional Matrix Factorization model to a two-stage model. The first stage, an ALS Matrix Factorization approach, identifies a much smaller candidate set for each user while the more complex second model, a complexfeed-forward neural network, ranks these 100 products for final evaluation. In addition to modifying our approach to a two-stage model, we also tried using popularity-based negative sampling and hard-negative sampling using our word2vec content embeddings. Below you can see the model architecture that was employed for our 2nd stage model. Our word2vec embeddings were custom built using the descriptions and labels tied to each product in the inventory.


| ![ncf_architecture.jpg](https://github.com/drghoshi/Grocery_ImplicitRecSystems/blob/main/Images/ncf_architecture.jpg) | 
|:--:| 
| *The architecture we used for our 2nd stage ranking model. The architecture is based on the Generalized Neural Matrix Factorization model, and is modified by adding our word2vec embeddings to the feed-forward network on the right hand side.* |



# Evaluation

The first test we decided to do was on our hard-negative sampling approach. In this approach we identified positive interactions between user and product, and employed our embedding space to select the n-nearest neighbors and classify them as negatives. In this sense, the model is learning specifically what products a user likes.

| ![n_nearest_neighbors.png](https://github.com/drghoshi/Grocery_ImplicitRecSystems/blob/main/Images/n_nearest_neighbors.png) | 
|:--:| 
| *Effect of including n hard nearest neighbor negatives for each positive instance on the Mean Average Precision @ 15. 0 nearest neighbors just refers to the candidate set generated by our 1st stage model.* |

As we see here, our model is not able to use this information effectively and thus, we decided to stick with our candidate generation set from the ALS model instead of injecting additional negative samples.

It is important to understand that a model's traditional success criteria does not align with recommendation based evaluation criteria. The ranking of products plays a role in recommendations and traditional classification metrics like AUC, Precision and Recall don't tell the full story. Below, you can see, for the same models as our first experiment, a difference in what is deemed the best perfoming model. For this reason, it is always important to look at ranking-based criteria.

| ![n_nearest_neighbors_weighted_auc.png](https://github.com/drghoshi/Grocery_ImplicitRecSystems/blob/main/Images/n_nearest_neighbors_weighted_auc.png) | 
|:--:| 
| *Effect of including n hard nearest neighbor negatives for each positive instance on AUC of the validation set. 0 nearest neighbors just refers to the candidate set generated by our 1st stage model.* |


Our final results are presented below. Adding a two stage model improved results by 11.9% for MAP@15 and 9.8% for NDCG@15. The addition of content-based features improved results by an additional 10.12% on MAP@15 and 3.7% on NDCG@15.

| ![final_results.png](https://github.com/drghoshi/Grocery_ImplicitRecSystems/blob/main/Images/final_results.png) | 
|:--:| 
| *Effect of including n hard nearest neighbor negatives for each positive instance on AUC of the validation set. 0 nearest neighbors just refers to the candidate set generated by our 1st stage model.* |


Future work can focus on improving the product embeddings we obtain from word2vec and trying other architectures which incorporate historical purchases and content information.