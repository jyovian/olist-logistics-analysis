# Olist Logistics Analysis - Data Model

## 1. Overview

The datasets provided contain multiple types of information, ranging from customer data, geolocation data, order information, and their products.

## 2. Data Warehouse Design

The business process being modelled here is order fulfilment on the Olist marketplace, from order placement through delivery to the customer. Performance is observed as a whole instead of attributed to a single logistics operator.

The following business queries guide the design of the warehouse:

##### 1. _"What is Olist's order fulfilment rate?"_

One of the main indicators that a logistics department was doing well is by seeing their order fulfilment rate. Out of the total delivery they made, how many were on time and how many were late. We will be using the Orders dataset, which contains the estimated and actual delivery dates.

##### 2. _"Which state has the most and least reliable delivery?"_

One of the many factors affecting logistics performance is location. Maybe the the delivery location is in a high-traffic area. Or maybe areas with high theft rate.

##### 3. _"Which product category has the highest and lowest rate of on-time deliveries?"_

Bulky products, such as furnitures, would obviously take longer to ship. However, does it also mean they will have worse delivery performance compared to smaller products, such as electronics? We will be using the Product and Order datasets.

##### 4. _"Is there any relation between review scores with delivery delays?"_

This query connects delivery performance with customer satisfaction. Sometimes a product can be good, but if the delivery was late, the review score would be lower. By comparing these two, we will be able to see whether there are any observed correlation between them. The datasets used will be the Orders and Order Reviews data.

##### 5. _"Are delivery delays concentrated among sellers in certain states or systemic across the platform?"_

This query will help identify if there are delivery delays that come from certain sellers, which will help direct improvement effort. If delays are concentrated, then a targeted intervention towards the worst performers is the highest-leverage move. If it is systemic, then the platform itself needs structural change. We will use the orders and sellers datasets.
