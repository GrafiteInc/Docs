# Billing

Utilizing Laravel's Cashier Scaffold comes with a fully setup billing system for use with Stripe.

## Requires

Some familiarity and an accout with Stripe.

## Outline

The billing system handles the stripe integration with cashier as mentioned. For the front-end elements scaffold comes with some vanilla javascript files which handle the setup of credit cards which adhere to North American and European laws. The remainder of integrations with Stripe occur through PHP components. It contains the `webhook` endpoint for Stripe feedback as well as a simple config file for handling various "plans" your app may offer.

## Config

```
billing.php
```

Within the billing config you can set the details for your invoices as well as the various plans you offer their names as per Stripe configuration which you can set in your `.env`. These plans are what is listed to the end user when they view the billing screen.

## User Data

As with all Stripe integrations including Cashier all you recieve is the last 4 digits of a credit card when the subscription is generated. The billing UI provides the user with which card their subscription is on, as well as the name of the plan they are on. It also shows their billing email, state and country. This can be helpful for any form of tax filings you need to do.

## Payment Method

Within the payment method tab the user can change their payment method.

## Change Plan

This allows the user to change their subscription plan which is then prorated as per Stripe's default methods.

## Coupons

We've integrated Stripe coupons into this as well. You can generate coupons on Stripe and enable you users to enter them on the 'Coupons' tab in the billing portion of the app.

## Invoices

Invoices are downloadable PDF files, and the upcoming invoice data is listed on the main billing tab.
