# Olist Logistics Analysis - Data Model

## 1. Overview

The datasets provided contain multiple types of information, ranging from customer data, geolocation data, order information, and their products.

## 2. Data Warehouse Design

The business process being modelled here is order fulfilment on the Olist marketplace, from order placement through delivery to the customer. Performance is observed as a whole instead of attributed to a single logistics operator.

### 2.1. Identifying Business Process

The following business queries guide the design of the warehouse:

###### 1. _"What is Olist's order fulfilment rate?"_

One of the main indicators that a logistics department was doing well is by seeing their order fulfilment rate. Out of the total delivery they made, how many were on time and how many were late. We will be using the Orders dataset, which contains the estimated and actual delivery dates.

###### 2. _"Which state has the most and least reliable delivery?"_

One of the many factors affecting logistics performance is location. Maybe the the delivery location is in a high-traffic area. Or maybe areas with high theft rate.

###### 3. _"Which product category has the highest and lowest rate of on-time deliveries?"_

Bulky products, such as furnitures, would obviously take longer to ship. However, does it also mean they will have worse delivery performance compared to smaller products, such as electronics? We will be using the Product and Order datasets.

###### 4. _"Is there any relation between review scores with delivery delays?"_

This query connects delivery performance with customer satisfaction. Sometimes a product can be good, but if the delivery was late, the review score would be lower. By comparing these two, we will be able to see whether there are any observed correlation between them. The datasets used will be the Orders and Order Reviews data.

###### 5. _"Are delivery delays concentrated among sellers in certain states or systemic across the platform?"_

This query will help identify if there are delivery delays that come from certain sellers, which will help direct improvement effort. If delays are concentrated, then a targeted intervention towards the worst performers is the highest-leverage move. If it is systemic, then the platform itself needs structural change. We will use the orders and sellers datasets.

### 2.2. Determining Grain

The grain will be designed to represent **one order item per row**. This item-level specifity is chosen because the business queries that we have determined above require analysis at the product level, such as item delivery performance and item reviews.An order-level grain would aggregate the data too much and we will lose detail.

### 2.3. Dimension Tables

- `dim_customer` - `customer_key`, `customer_id`, `customer_unique_id`, `customer_city`, `customer_state`
- `dim_date` - `date_key`, `day`, `day_name`, `quarter`, `month`, `year`
- `dim_product` - `product_key`, `product_id`, `product_category_name`, `product_weight_g`, `product_length_cm`, `product_height_cm`, `product_width_cm`, `product_category_english`, `product_vol_cm3`
- `dim_seller` - `seller_key`, `seller_id`, `seller_city`, `seller_state`

### 2.4. Fact Tables

There are 2 transactions that are recorded, here. Deliveries and reviews. Hence, we will have 2 fact tables

#### `fact_deliveries`

Grain: **one order item**

###### Foreign Keys

- `customer_key`
- `seller_key`
- `product_key`
- `purchase_date_key`
- `delivered_date_key`
- `estimated_delivery_date_key`

###### Degenerate dimensions

Keys without dimension tables

- `order_id`
- `order_item_id`
- `order_status`

###### Measures

- `price`
- `freight_value`
- `delivery_delay_days`  (derived: actual delivery - estimated)
- `is_late` (derived)
- `shipping_days` (derived: purchase to delivery)

#### `fact_reviews`

Grain: **one review**

###### Foreign Keys

- `customer_key`
- `review_creation_date_key`

###### Degenerate dimensions

- `review_id`
- `order_id`

###### Measures

- `review_score`
- `response_time_hours` (derived: answer to creation)
- `has_comment` (derived boolean)
