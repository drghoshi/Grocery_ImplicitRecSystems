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

(placeholder)

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
being selected than less-popular products. These methods work, but they can be problematic due to the potential of
picking very niche products with no history or picking items that are popular in a global sense and fail to pick up on
various personas of shoppers. The other set of techniques, called model-based negative sampling methods, involve
using machine learning models to predict the propensity of a user purchasing an item, and including items which
score high but are never purchased by the user. These products generally fall in line with other products the user has
bought in the past, but are never purchased, indicating a negative relationship the model should learn to differentiate
from the specific products the user enjoys.

We decided to try 3 techniques to deal with this lack of true negative samples. All three techniques are predicated on going from a singluar traditional Matrix Factorization model to a two-stage model. The first stage, an ALS Matrix Factorization approach, identifies a much smaller candidate set for each user while the more complex second model, a complexfeed-forward neural network, ranks these 100 products for final evaluation. In addition to modifying our approach to a two-stage model, we also tried using popularity-based negative sampling and hard-negative sampling using our word2vec content embeddings. Below you can see the model architecture that was employed for our 2nd stage model.

| ![orders_snapshot.png](https://github.com/drghoshi/Grocery_ImplicitRecSystems/blob/main/Images/orders_snapshot.png) | 
|:--:| 
| *A snapshot of the orders dataset. Above you can see the first two orders for user_id 1.* |


Our word2vec embeddings were custom built using the descriptions and labels tied to each product in the inventory.

# Evaluation



