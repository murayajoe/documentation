Product combos
==============

The "Product Combos" feature enables users to define and manage combination options within a
particular product. This functionality can be utilized to create multiple-choice restaurant menus or
to offer product sets in retail stores.

=============
Configuration
=============

Step one is to create combination choices. To do so:

#. Go to :menuselection:`Point of Sale --> Products --> Product Combos` and click :guilabel:`New`.
#. Name your combo and add the products you want customers to choose from by clicking :guilabel:`Add
   a line`. You can also include an extra price for each option in the :guilabel:`Extra Price`
   column.

As a reference, the selected product's original price is displayed in the :guilabel:`Original Price`
column.

.. image:: combos/combo-form.png
   :scale: 75%

Step two is to create a specific product to gather combo choices. To do this:

#. Go to :menuselection:`Point of Sale --> Products --> Products` and click :guilabel:`New`.
#. Set the :guilabel:`Product Type` to :guilabel:`Combo` and fill in the general information tab.

   .. note::
      - The sales price you enter will be the price of the combo, regardless of the original
        prices of the products within the combo choices.
      - The combo product price is only affected by the extra price optionally defined at the combo
        choice creation or if a variant of one of the items has a specified extra price.
#. Go to the :guilabel:`Combo Choices` tab, click :guilabel:`Add a line`, and select the
   combinations to add. You can also create a new combination at this step by clicking
   :guilabel:`New` on the popup window.

.. image:: combos/combo-product-form.png
   :scale: 75%

Once you have created and added the combo choices into a product, you can sell combos in your retail
store or restaurant.

Practical application
=====================

:ref:`Open a POS session <pos/session-start>` and select the combo product. Choose the options and
click :guilabel:`Add to order`. As a reminder, the extra price appears under the related choices.

.. image:: combos/combo-select.png
   :scale: 75%
