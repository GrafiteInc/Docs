# Billing

Utilizing Laravel's Cashier Scaffold comes with a fully setup billing system for use with Stripe.

## Requires

Some familiarity and an accout with Stripe.

## Outline

The billing system handles the stripe integration with cashier as mentioned. The remainder of integrations with Stripe occur through PHP components. It contains the `domain.com/stripe/webhook` endpoint for Stripe feedback as well as a simple config file for handling various "plans" your app may offer.

## Config

```
billing.php
```

Within the billing config you can set the details for your invoices as well as the various plans you offer their names as per Stripe configuration which you can set in your `.env`. These plans are what is listed to the end user when they view the billing screen.

## User Data

As with all Stripe integrations including Cashier all you recieve is the last 4 digits of a credit card when the subscription is generated. The billing UI provides the user with which card their subscription is on, as well as the name of the plan they are on.

## Webhooks and testing

The webhook setup is key to having everything function well with the portal system. Ideally, we don't want to touch any customer data so its best to be able to direct accordingly. Webhooks will provide us with the state changes that happen in the Stripe UI.

You can review how to test your webhooks here:

https://stripe.com/docs/stripe-cli

Some commands you will need to run locally:

```sh
stripe login
stripe listen --forward-to https://{local-domain}/stripe/webhook
```

## Change Plan

This should be handled in the customer portal configuration.

## Coupons / Promotional Codes

This should be configured within the customer portal configuration. These are added when a person subscribes or updates a subscription.

## Billing Portal

Rather than maintain a complex set of forms and views for payment methods, cancelling, renewing, and invoices, Scaffold by default lets Stripe handle all that with the billing portal.

In order to ensure you have everything set up correctly you need to have a Stripe account and some ENVs set.

```
PLAN_MONTHLY={price_token}
PLAN_YEARLY={price_token}

STRIPE_KEY={dev_public}
STRIPE_SECRET={dev_secret}
STRIPE_WEBHOOK_SECRET={webhook_secret}

CASHIER_CURRENCY=USD
```

## Stripe Dashboard

Within Stripe's dashboard you need to set up your products, some pricing the `billing` config handles the two cases of monthly and yearly. Within products you create prices to ensure you do that properly. You then need to configure the webhook which can be done either directly in the UI: please follow Laravel's Cashier docs for those details OR you can use their command `php artisan cashier:webhook`. Lastly, you need to set up your "Customer Billing Portal" and ensure its branding matches what you want for your product.

### Tax

> These links are to the test endpoints, you will need to do the same on live/prod mode.

If you enable tax you need to set an origin location for your business:

https://dashboard.stripe.com/test/settings/tax

You need to also ensure that your billing portal is configured or "enabled"

https://dashboard.stripe.com/test/settings/billing/portal

