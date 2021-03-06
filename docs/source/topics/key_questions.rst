==============================================
Building an e-commerce site: the key questions
==============================================

When building an e-commerce site, there are several components whose
implementation is strongly domain-specific.  That is, every site will have
different requirements for how such a component should operate.  As such, these
components cannot easily be modeled using a generic system - no configurable
system will be able to accurately capture all the domain-specific behaviour
required.

The design philosophy of Oscar is to not make a decision for you here, but to
provide the environment where any domain logic can be implemented, no matter how
complex.

This document lists the components which will require implementation according
to the domain at hand.  These are the key questions to answer when building your
application.  Much of Oscar's documentation is in the form of "recipes" that
explain how to solve the questions listed here.  Each question links to the
relevant recipes.

Catalogue
=========

What are your product types?
----------------------------

Are you selling books, DVDs, clothing, downloads, or fruit and vegetables?  You will
need to capture the attributes of your product types within your models.  Oscar
divides products into 'product classes' which each have their own set of
attributes.  

* :doc:`/howto/how_to_model_your_catalogue`
* :doc:`/howto/importing_a_catalogue`

How is your catalogue organised?
--------------------------------

How are products organised within the site?  A common pattern is to have a
single category tree where each product belongs to one category which sits
within a tree structure of other categories.  However, there are lots of other
options such as having several separate taxonomy trees (e.g., split by brand, by
theme, by product type).  Other questions to consider:

* Can a product belong to more than one category?
* Can a category sit in more than one place within the tree?  (e.g., a "children's fiction" category
  might sit beneath "children's books" and "fiction").

* :doc:`/howto/how_to_customise_an_app`
* :doc:`/howto/how_to_customise_models`
* :doc:`/howto/how_to_override_a_core_class`

How are products managed?
-------------------------

Is the catalogue managed by a admin using a dashboard, or though an automated
process, such as processing feeds from a fulfillment system?  Where are your
product images going to be served from?

* :doc:`/howto/how_to_disable_an_app`


Pricing, stock and availability
===============================

How is tax calculated?
----------------------

What availability messages are shown to customers?
--------------------------------------------------

Based on the stock information from a fulfillment partner, what messaging should be
displayed on the site?  

* :doc:`/howto/how_to_configure_stock_messaging`

Do you allow pre- and back-orders
---------------------------------

An pre-order is where you allow a product to be bought before it has been
published, while a back-order is where you allow a product to be bought that is
currently out of stock.


Shipping
========

How are shipping charges calculated?
------------------------------------

There are lots of options and variations here.  Shipping methods and their
associated charges can take a variety of forms, including:

* A charge based on the weight of the basket
* Charging a pre-order and pre-item charge
* Having free shipping for orders above a given threshold

Recipes:

* :doc:`/howto/how_to_configure_shipping`

Which shipping methods are available?
-------------------------------------

There's often also an issue of which shipping methods are available, as
this can depend on:

* The shipping address (e.g., overseas orders have higher charges)
* The contents of the basket (e.g., free shipping for downloadable products)
* Who the user is (e.g., sales reps get free shipping)

Oscar provides classes for free shipping, fixed charge shipping, pre-order and
per-product item charges and weight-based charges.  It is provides a mechanism
for determining which shipping methods are available to the user.

Recipes:

* :doc:`/howto/how_to_configure_shipping`


Payment
=======

How are customers going to pay for orders?
------------------------------------------

Often a shop will have a single mechanism for taking payment, such
as integrating with a payment gateway or using PayPal.  However more
complicated projects will allow users to combine several different payment
sources such as bankcards, business accounts and gift cards.

Possible payment sources include:

* Bankcard
* Google checkout
* PayPal
* Business account
* Managed budget
* Gift card
* No upfront payment but send invoices later

The checkout app within ``django-oscar`` is suitable flexible that all of these
methods (and in any combination) is supported.  However, you will need to
implement the logic for your domain by subclassing the relevant ``view/util``
classes.

Domain logic is often required to:

* Determine which payment methods are available to an order;
* Determine if payment can be split across sources and in which combinations;
* Determine the order in which to take payment;
* Determine how to handle failing payments (this can get complicated when using
  multiple payment sources to pay for an order).

* :doc:`/howto/how_to_configure_shipping`

When will payment be taken?
---------------------------

A common pattern is to 'pre-auth' a bankcard at the point of checkout then
'settle' for the appropriate amounts when the items actually ship.  However,
sometimes payment is taken up front.  Often you won't have a choice due to
limitations of the payment partner you need to integrate with.

* Will the customer be debited at point of checkout, or when the items are dispatched?
* If charging after checkout, when are shipping charges collected?
* What happens if an order is cancelled after partial payment?
