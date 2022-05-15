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
\


| ![orders_snapshot.png](https://github.com/drghoshi/Grocery_ImplicitRecSystems/blob/main/Images/orders_snapshot.png) | 
|:--:| 
| *A snapshot of the orders dataset. Above you can see the first two orders for user_id 1.* |

# File Structure & Guide

(placeholder)

# Implementation

(placeholder)

# Evaluation

(placeholder)

