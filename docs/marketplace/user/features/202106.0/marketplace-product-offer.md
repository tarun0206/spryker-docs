---
title: Marketplace Product Offer
description: This document contains concept information for the Product offers feature in the Spryker Commerce OS.
template: concept-topic-template
---

The *Product Offer* entity is created when multiple merchants need to sell the same product on the Marketplace.

Product offer is created per concrete product and contains product-specific information, information about the merchant selling this product, and the offer price. Any concrete product can have one or many offers from different merchants. Therefore, a unique *product offer reference* is defined per each product offer and is used to identify the offer in the system. Offer reference is mandatory and can only be defined once.

Merchants can create product offers in the Merchant Portal <!---LINK TO MERCHANT PORTAL FOR OFFERS--> or [import the product offers](/docs/marketplace/dev/data-import/{{ page.version }}/file-details-merchant-product-offer-csv.html).

 Marketplace administrators can view and approve or deny merchants' product offers in the Back Office. See [Managing merchant product offers](/docs/marketplace/user/back-office-user-guides/{{ page.version }}/marketplace/offers/managing-merchant-product-offers.html) for details.

 Every merchant can have multiple offers for the same concrete product. However, a product offer is related to a single merchant and cannot be shared between other merchants:

![Multiple product offers per product](https://spryker.s3.eu-central-1.amazonaws.com/docs/Features/Marketplace/Products+and+offers/Product+offer+feature+overview/product-offers-per-product.png)

![Product offers on PDP](https://spryker.s3.eu-central-1.amazonaws.com/docs/Features/Marketplace/Products+and+offers/Product+offer+feature+overview/product-offers-on-pdp.png)

{% info_block infoBox "Note" %}

You can retrieve product offer details via Glue API. See [Retrieving product offers](/docs/marketplace/dev/glue-api-guides/{{ page.version }}/retrieving-product-offers.html) and [Retrieving product offers for a concrete product](/docs/marketplace/dev/glue-api-guides/{{ page.version }}/concrete-products/retrieving-product-offers-for-a-concrete-product.html) for details.

{% endinfo_block %}

## Product offer structure
To define visibility of a product offer on the Storefront, the following details are attached to the product offer entity:

| OFFER PARAMETER      | DESCRIPTION           |
| ------------------- | ----------------------------- |
| Concrete product SKU | Defines the concrete product the offer is created for.       |
| Merchant SKU         | Allows the merchant to identify the product offer in the ERP system. |
| Offer Reference      | Unique ID that helps to identify the product offer in the Marketplace. Offer reference is mandatory. |
| Store                | Defines the store where the product offer is available.      |
| Price                | Allows the merchant to set their price for the offer. {% info_block infoBox "Info" %} You can also set [volume prices](https://documentation.spryker.com/docs/volume-prices-overview) for a product offer. For now, you can only [import volume prices for product offers](/docs/marketplace/dev/data-import/{{ page.version }}/file-details-price-product-offer-csv.html). {% endinfo_block %}      |
| Stock                | Allows the merchant to define stock for the product offer. The stock can be reserved and available. |
| Status               | Approval status: <ul><li>Approval status (Waiting for approval, Approved, Denied).</li><li>Visibility: Visibility (Active, Inactive).</li></ul> |
| Validity Dates       | Specifies the period during which the product offer is visible on the Storefront. Concrete product validity dates have higher priority over the Offer validity dates. |


## Product offer status
Product offer status defines whether the offer is active and displayed on the Storefront. Based on this, the product offer may have offer approval status and visibility status.

### Offer approval status

* *Waiting for Approval*: Default status that is applied to the offer after it has been created.

* *Approved*:  The approved offer can be displayed on the Storefront. Only the Marketplace administrator can approve the offer. See [Approving or denying offers](/docs/marketplace/user/back-office-user-guides/{{ page.version }}/marketplace/offers/managing-merchant-product-offers.html#approving-or-denying-offers) for details on how a Marketplace administrator can approve offers in the Back Office.

* *Denied*: If the offer is denied, it cannot be displayed on the Storefront. Only the Marketplace administrator can deny the offer. See [Approving or denying offers](/docs/marketplace/user/back-office-user-guides/{{ page.version }}/marketplace/offers/managing-merchant-product-offers.html#approving-or-denying-offers) for details on how a Marketplace administrator can deny offers in the Back Office.

### Visibility

* *Active*: When an offer is active, it is displayed on the Storefront. Either merchant or Marketplace administrator can make the offer active.

* *Inactive*: When an offer is inactive, it is not displayed on the Storefront. Either merchant or Marketplace administrator can make the offer inactive.

![Offer approval flow](https://spryker.s3.eu-central-1.amazonaws.com/docs/Features/Marketplace/Products+and+offers/Product+offer+feature+overview/offer-approval-flow.png)

## Product offer price

On the product detail page, a customer sees a list of product offers from one or several merchants. Each offer has its own price. This price is represented as the *product offer price* price dimension.
The product offer prices support:

* Mode (Net/Gross)
* Volume
* Store
* Currency

Product offer price follows the [concrete product price inheritance model](https://documentation.spryker.com/docs/price-functionality#price-inheritance). So if the Merchant doesn't set a price in the offer, it is taken from the concrete product. Otherwise, the product offer price has a higher priority and substitutes the concrete product price if it is indicated. If at least one price is defined for the offer (e.g., original), it is valid for this offer even if the concrete product has a default price (sales price), but the offer does not. See [Price types](https://documentation.spryker.com/docs/scheduled-prices-feature-overview#price-types) for details on the price types.

Merchants can define product offer prices in the Merchant Portal when they create product offers,<!---LINK TO MERCHANT PORTAL FOR OFFERS--> or [import the product offer price](/docs/marketplace/dev/data-import/{{ page.version }}/file-details-price-product-offer-csv.html).

## Product offer stores
Merchant product offer is defined per store. Merchants set their own prices per store for the product offer.
However, defining the right store for the product offer affects its visibility. When setting the stores for the product offer, merchants need to pay attention to the stores where their abstract products are available.
The table below illustrates the logic according to which the product offer is displayed in the Storefront.

| Characteristics    | DE   | AT   | US   |
| ----------------------------------------- | ---- | ---- | ---- |
| Store where the abstract product is added | &check;    | &check;    | x    |
| Store where the product offer is added    | x    | &check;    | &check;    |
| Is product offer visible?                 | no   | yes  | no   |

Merchants can define product offer stores in the Merchant Portal when they create product offers,<!---LINK TO MERCHANT PORTAL FOR OFFERS--> or [import the product offer store](/docs/marketplace/dev/data-import/{{ page.version }}/file-details-merchant-product-offer-store-csv.html).

## Product offer stock
A product offer has its own stock in one or many warehouses. A warehouse can hold stock for multiple offers.

The stock per offer in the warehouse is defined by merchant the same way it is defined for concrete product. It means that offer reservation is assigned to every product offer separately. 

In cases when an offer doesn't have any physical stock and can always be purchased, there is the `is_never_out_of_stock` attribute that is added to the offer entity.

When `is_never_out_of_stock` is set to `true`, then this offer is always available in terms of stock.
When the offer is out of stock, it is displayed as an out-of-stock product.

Merchants can define product offer stocks in the Merchant Portal when they create product offers,<!---LINK TO MERCHANT PORTAL FOR OFFERS--> or [import the product offer stock](/docs/marketplace/dev/data-import/202106.0/file-details-product-offer-stock-csv.html).

### Product offer availability
Product offer availability calculation differs from the calculation of concrete products availability:

| Concrete product availability   | Product offer availability   |
| --------------------- | ------------------------ |
| Formula: Concrete product availability = Concrete product quantity – Concrete product reservations | Formula: Offer availability = Offer quantity – Offer reservations |

Thus, the algorithm of calculating offer availability is updated, but the algorithm of calculating reservations is preserved.
Offer availability is considered on the Storefront: 

* On the product details page while adding the offer to cart.
* On the cart page: Product stays in the cart if the attached offer is not available anymore and a hint is shown.
* During the checkout: When pressing **Buy now** the availability is checked one more time.

{% info_block infoBox "Example" %}

Let's assume that a merchant has defined quantity 10 for product offer 1. The customer adds 8 items of the product offer 1 to a shopping cart, and later updates the quantity to 12. In such a situation, the availability of the product offer 1 is checked and the customer is notified to update the quantity of the product offer to the available number to proceed with the purchase. 

{% endinfo_block %}


## Product offers on the Storefront

Merchant product offer with all the related offer information is visible on the product detail page, and further on the shopping cart page and checkout pages when the following conditions are met:

1. The merchant who owns the offer is [*Active*](/docs/marketplace/user/back-office-user-guides/{{ page.version }}/marketplace/merchants/managing-merchants.html#activating-and-deactivating-merchants).
2. The product offer status is:
    * Approved
    * Active
3. The product offer is defined for the current store.
4. The current store is defined for the provided offer.
5. The current day is within the range of the product offer validity dates.

The decision of whether the product offer can be purchased depends on the offer availability. But availability has no influence on offer visibility on the Storefront.

### Product offers on the product details page

All available product offers are listed in the *Sold by* area of the product details page. If there are multiple product offers for a concrete product, there is always a default product offer pre-checked. Currently, a random offer is selected as the default one, however, you can change this logic on the project level.

![Product offers on product details page](https://spryker.s3.eu-central-1.amazonaws.com/docs/Features/Marketplace/Products+and+offers/Product+offer+feature+overview/product-offers-on-pdp.gif)

### Product offers in the shopping cart

Offers from different merchants are added as separate cart items, each with its quantity. You can add a note to the offer on the cart page.
A customer can review the merchant information by clicking the link in the *Sold By* hint.

![Product offers in cart](https://spryker.s3.eu-central-1.amazonaws.com/docs/Features/Marketplace/Products+and+offers/Product+offer+feature+overview/add-offers-to-cart.gif)

### Product offers during checkout

During the checkout, offers from the same merchant are grouped for delivery so that the customer can always know how many shipments to expect and the merchants can smoothly fulfill the orders.

![Product offers during checkout](https://spryker.s3.eu-central-1.amazonaws.com/docs/Features/Marketplace/Products+and+offers/Product+offer+feature+overview/product-offers-during-checkout.gif)

### Product offers in the wishlist

Customers can add product offers to a wishlist for future purchase. Merchant information is kept for the offer when it is added to a wishlist. Further, customers can add the offer from the wishlist to cart.

![Product offers in wishlist](https://spryker.s3.eu-central-1.amazonaws.com/docs/Features/Marketplace/Products+and+offers/Product+offer+feature+overview/add-product-offer-to-wl-and-from-wl-to-cart.gif)

## Current constraints

* B2B Merchant-specific prices do not work with product offer prices.
* All cart-related B2B features (e.g., Quick Order, RFQ, Approval Process, etc.) will be supported later.
* Availability Notification is not supported.

## Related Business User articles

|FEATURE OVERVIEWS  |MERCHANT PORTAL USER GUIDES  |BACK OFFICE USER GUIDES |
|---------|---------|---------|
|[Marketplace Product Offer feature overview](/docs/marketplace/user/features/{{ page.version }}/marketplace-product-offer.html) |<!--link -->  |[Managing merchant product offers](/docs/marketplace/user/back-office-user-guides/{{ page.version }}/marketplace/offers/managing-merchant-product-offers.html)|

{% info_block warningBox "Developer guides" %}

Are you a developer? See [Marketplace Product Offer](/docs/marketplace/dev/feature-walkthroughs/{{ page.version }}/marketplace-product-offer/marketplace-product-offer.html) feature walkthrough for developers.

{% endinfo_block %}