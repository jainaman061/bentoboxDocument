**Food Delivery App Backend Documentation**
**Overview**
This backend service supports a food delivery app with three primary user roles: Customer, Chef, and Rider. The system enables secure OTP authentication, payment processing, meal subscriptions, order management, and delivery tracking.

**1. Customer App**
Authentication
OTP Authentication: Customers can log in/register using OTP verification.

Payments
Razorpay Integration: Supports payments and refunds via Razorpay gateway.

Ordering and Subscription
Order Types:

One-time order

Subscription-based tiffin meal plans

Default Meal Selection:

Customers must select a default meal daily by 10 AM or 12 Noon.

If not selected, the system auto-assigns a default meal from the restaurant's available menu.

Notifications:

Customer receives notification daily at 10 AM to select their default meal.

Subscription Management
Pause Subscription:

Customers can pause their subscription.

Paused days are added to the end of the subscription period upon resumption.

Cancel Subscription:

Partial refunds are calculated based on days consumed.

Refund is processed against the closest matching meal plan.

Modification:

Customers can change delivery time slots and address daily.

History & Records
View subscription history and order history.

View meals served under subscription for current and previous weeks.

**2. Chef App**
Authentication
OTP Authentication: Chefs authenticate using OTP.

Restaurant & Meal Plan Setup
On login, chef creates or manages their restaurant profile.

Define meal plans for subscriptions, e.g., 7, 15, 30 days.

Subscription & Orders
Customers select a meal plan and make payment.

Subscription start time depends on order timing:

If ordered before 12 noon, subscription starts immediately.

If ordered after 12 noon, subscription starts next day.

Batch Management & Rider Assignment
Create delivery batches containing both one-time and subscription orders.

Assign batches to riders.

Every order has an OTP visible only to customers for verification.

Rider Management
Register riders under the restaurant.

Rider registration requires admin approval.

Rider details are shared with the admin for approval.

**3. Rider App**
Authentication
OTP-based login and registration.

Rider accounts can only be created/registered by restaurant chefs.

Order Delivery
Riders view assigned batches.

Optimized delivery routes provided on integrated map.

Orders require OTP verification upon delivery; delivery not allowed if OTP is incorrect.

Once the last order in the batch is marked delivered, the entire batch is considered delivered.

Notifications
Riders receive notifications on batch status changes.

Customers receive corresponding notifications for batch updates (e.g., rider assigned, order out for delivery, order delivered).

Additional Notes
Notifications Summary
Customers: Meal selection reminder, order status, batch status.

Riders: Batch assignments, route updates, order status.

Chefs: Rider registration status, batch status, order status.

Refund Calculations
Refunds on subscription cancellation are prorated based on days used.

System finds the closest meal plan for refund processing.

Security & Access Control
OTP verification is used for all roles.

Order OTP protects delivery authenticity.

Admin approval controls rider registrations.

API Endpoints (High Level Overview)
Module	Endpoint	Description
Customer	POST /auth/otp-login	Login/Register via OTP
Customer	POST /orders/one-time	Place a one-time order
Customer	POST /subscriptions/start	Start subscription with payment
Customer	GET /subscriptions/history	Fetch subscription history
Customer	POST /subscriptions/pause	Pause subscription
Customer	POST /subscriptions/cancel	Cancel subscription with refund
Customer	POST /meal/select-default	Select daily default meal
Chef	POST /auth/otp-login	Chef login/register via OTP
Chef	POST /restaurants/create	Create restaurant profile
Chef	POST /mealplans/create	Create meal plans
Chef	POST /batches/create	Create delivery batch
Chef	POST /riders/register	Register a rider
Rider	POST /auth/otp-login	Rider login/register via OTP
Rider	GET /batches	View assigned batches
Rider	POST /orders/verify-delivery	Verify OTP and mark order delivered
Notifications	(Various endpoints for push notifications)
