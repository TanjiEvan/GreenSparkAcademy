# DeshDash — Data Dictionary

Plain-language description of each data source and what every column means.

---

## 1. OrderDB — `deshdash.db`
**What it is:** Record of every customer order placed on DeshDash. One row = one order.
Table name: `orders`.

| Column | Description |
|---|---|
| `order_id` | Unique reference for the order |
| `customer_id` | The customer who placed the order |
| `restaurant_id` | The restaurant the order was placed with |
| `zone` | Delivery zone the order was placed in |
| `order_time` | Date and time the order was placed |
| `order_amount` | Food value of the order in Taka, before any discount |
| `delivery_fee` | Delivery fee charged to the customer, in Taka |
| `discount_pct` | Discount applied to the order (0 = none, 0.25 = 25% off) |
| `promo_code` | Promo code used on the order, if any |
| `delivery_minutes` | How long the delivery took, in minutes |
| `status` | Final state of the order: delivered, cancelled, or refunded |

---

## 2. Partner API — `restaurants.json`
**What it is:** Master list of DeshDash's partner restaurants and their details.
One record = one restaurant. (Records are inside the `data` field of the response.)

| Field | Description |
|---|---|
| `restaurant_id` | Unique reference for the restaurant |
| `restaurant_name` | Name of the restaurant |
| `cuisine` | Type of cuisine the restaurant serves |
| `commission_rate` | DeshDash's commission share per order (e.g. 0.20 = 20%) |
| `zone` | Zone the restaurant operates in |
| `onboarded_date` | Date the restaurant joined DeshDash |
| `is_active` | Whether the restaurant is currently active |

---

## 3. Campaigns Log — `campaigns.csv`
**What it is:** List of marketing campaigns run by the growth team. One row = one campaign.

| Column | Description |
|---|---|
| `campaign_id` | Unique reference for the campaign |
| `campaign_name` | Name of the campaign |
| `promo_code` | Promo code used for the campaign |
| `discount_pct` | Discount the campaign offered (e.g. 0.25 = 25%) |
| `start_date` | Campaign start date |
| `end_date` | Campaign end date |
| `target_zones` | Zones the campaign targeted |
| `target_tier` | Restaurant segment the campaign targeted |
| `status` | Whether the campaign is active or completed |

---

## 4. Zone Tracker — `zones.xlsx`
**What it is:** Operations spreadsheet tracking each delivery zone. One row = one zone.
Maintained by the Ops team. (The header is not on the first row.)

| Column | Description |
|---|---|
| `Zone` | Delivery zone name |
| `Monthly Order Target` | Ops target for orders in that zone per month |
| `Riders (April)` | Number of active delivery riders in April |
| `Riders (May)` | Number of active delivery riders in May |
| `Ops Notes (May)` | Free-text notes from the Ops team |
