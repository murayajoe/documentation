=====================
In-App Purchase (IAP)
=====================

In-App Purchases (IAP) are optional services that unlock new functionality in Odoo databases. For
example, the **SMS** service sends text messages to contacts directly from the database, and the
**Documents Digitization** service digitizes scanned or PDF vendor bills, expenses, and resumes with
optical character recognition (OCR) and Artifical Intelligence.

In order to use IAP services, an admin must first create IAP accounts for each service, and then
users must purchase IAP credits to spend on those services. Each service has its own pricing and
pack options listed on the `Odoo IAP website <https://iap.odoo.com/iap/all-in-app-services>`_.

.. image:: in_app_purchase/iap.png
   :align: center
   :alt: Visit IAP.Odoo.com to review what services are available.

.. _in_app_purchase/portal:

.. _in_app_purchase/accounts:

IAP accounts
============

IAP accounts regulate which :ref:`services <in_app_purchase/services>` are available in an Odoo
database. Each account is tied to a specific service, and can only be created or edited by an admin.

.. _in_app_purchase/create:

Create accounts
---------------

Admins create IAP accounts using the database's :guilabel:`Technical Settings`. Once an account is
created, admins can :ref:`adjust the account's settings <in_app_purchase/settings>` or :ref:`buy
credits <in_app_purchase/credits>` for the IAP service associated with the account.

To create an IAP account:

#. Activate Odoo's :ref:`developer mode <developer-mode>`.
#. On the main apps dashboard, type :guilabel:`IAP`.
#. In the pop-up menu that appears, click :guilabel:`Settings / Technical / IAP / IAP Accounts`.
#. On the :guilabel:`IAP Account Settings` page, click the :guilabel:`New` button in the top-left.
#. Fill out all fields on the :guilabel:`Account Information` form (see below):

   - :guilabel:`Name`: the name of the IAP account.
   - :guilabel:`Service Name`: the name of the IAP service. This field cannot be edited.
   - :guilabel:`Description`: the description of the IAP service. This field cannot be edited.
   - :guilabel:`Company`: defines which companies in the database have access to the IAP service.

     - If the field is **blank**, the service is available to **all companies**.
     - If the field lists **specific companies**, the service is available to **only the listed
       companies**.
   - :guilabel:`Account Token`: an automatically generated token used to identify this IAP account.
   - :guilabel:`Warn me`: check this box to be notified when IAP credits are low for this account.
     When checked, the following fields appear:

     - :guilabel:`Threshold`: Odoo will send a notification email when credits for the service fall
       below the amount listed in this field.
     - :guilabel:`Email`: the email address that will receive the low-credit notification.
   - :guilabel:`Buy Credit`: click this button to go to the IAP portal and purchase credits for this
     service.
   - :guilabel:`Balance`: displays the current balance of IAP credits for this account.
#. Click the :guilabel:`Manually Save` icon in the top-left.

.. _in_app_purchase/settings:

Account settings
----------------

After creating an IAP account, admins can manage its usage and make changes using the database's
technical settings.

To access IAP account settings:

#. Activate Odoo’s :ref:`developer mode <developer-mode>`.
#. On the main apps dashboard, type :guilabel:`IAP`, and in the pop-up menu that appears, click
   :guilabel:`Settings / Technical / IAP / IAP Accounts`.
#. On the :guilabel:`IAP Account Settings` page, click the account to make changes.
#. When the :guilabel:`Account Information` form appears, follow the steps below for the
   desired changes.

.. tabs::

   .. tab:: Restrict companies

      By default, an IAP account applies to all companies in the database, but this can be adjusted
      to only apply to certain companies.

      To change which companies have access to the IAP service for an account:

      #. On the :guilabel:`Account Information` form, click the :guilabel:`Companies` field.
      #. In the resulting drop-down menu, type the name of the company, and click on the name when
         it appears.
      #. Repeat steps 1-2, as needed, until all desired companies are added.
      #. Click the :guilabel:`Save Manually (cloud)` icon on the top-left of the form.

      .. image:: in_app_purchase/companies.png
         :align: center
         :alt: The Company field restricts access to the IAP service to only the listed companies.

      .. note::
         - If this field is **blank**, then the IAP service is available to **all companies** in the
           database.
         - If the field is **not blank**, then the IAP service is available to **only the companies
           listed** in the field.

   .. tab:: Disable accounts

      To disable an IAP account:

      #. On the :guilabel:`Account Information` form, click the :guilabel:`👁️ (eye)` icon next to
         the :guilabel:`Account Token` field to reveal the token.
      #. Click the token, and type :guilabel:`+disabled` at the end.
      #. Click the :guilabel:`Save Manually (cloud)` icon on the top-left of the form.

      .. image:: in_app_purchase/disable.png
         :align: center
         :alt: Click on the account token and type "+disabled" at the end.

   .. tab:: Re-enable accounts

      If an IAP account has been disabled, an admin can re-enable the account using the following
      steps:

      #. On the :guilabel:`Account Information` form, click the :guilabel:`👁️ (eye)` icon next to
         the :guilabel:`Account Token` field to reveal the token.
      #. Delete :guilabel:`+disabled` from the end of the token.
      #. Click the :guilabel:`Save Manually (cloud)` icon on the top-left of the form.

      .. image:: in_app_purchase/re-enable.png
         :align: center
         :alt: Click on the account token and delete "+disabled" from the end.

   .. tab:: Low-credit notification

      Credits are required to use IAP services.

      To be notified when an account's credits are low:

      #. On the :guilabel:`Account Information` form, click the :guilabel:`Warn Me` box. This will
         cause two more fields to appear.
      #. In the :guilabel:`Threshold` field, type an amount. Odoo will send a notification email
         when credits for this service fall below this amount.
      #. In the :guilabel:`Email` field, type the email address that should receive the
         notificaiton.
      #. Click the :guilabel:`Save Manually (cloud)` icon on the top-left of the form.

      .. image:: in_app_purchase/low-credits.png
         :align: center
         :alt: Odoo will send an email alert when credits for this service fall below the threshold.

.. _in_app_purchase/services:

IAP services
============

The `Odoo IAP website <https://iap.odoo.com/iap/all-in-app-services>`_ lists all IAP Services
currently available. To utilize a service, an admin first needs to :ref:`create an account
<in_app_purchase/create>` for it in the Odoo database.

Services are then unlocked by :ref:`purchasing packs of IAP credits <in_app_purchase/credits>`
specific to each service, and those credits are spent whenever the service is used.

The IAP services currently offered include:

- :guilabel:`AI generated call summary and transcript service`: call recording transcript service
  for the Asterisk Plus application.
- :guilabel:`Amazon Odoo Connector`: establishes seamless integration between an Amazon Seller
  Central Account and Odoo, allowing users to effectively manage their products and orders.
- :guilabel:`Avatax Proxy`: computes taxes in the Avalara Tax Engine for Brazil.
- :guilabel:`Documents Digitization`: digitizes scanned or PDF vendor bills, expenses and resumes
  with OCR and Artifical Intelligence.
- :guilabel:`GSTR India eFiling`: **NEED CONTENT.**
- :guilabel:`Hungarian Online Invoice`: Hungarian invoicing. **NEED CONTENT.**
- :guilabel:`Indian EDI`: an electronic data interchange service for sending electronic documents to
  the Indian government.
- :guilabel:`Lead Generation`: generates leads based on a set of criteria and enriches the company
  data of opportunities. Converts web visitors into quality leads and opportunities.
- :guilabel:`MNB Exchange Rates for Invoices and Sales Odoo Apps`: downloads MNB (Hungarian National
  Bank) current exchange rates automatically and connects with Odoo's payment / sales / e-commerce
  activities to update currencies according to the Central Bank of Hungary.
- :guilabel:`OdAS2 Rapsodoo`: OdAS2 Gateway. **NEED CONTENT.**
- :guilabel:`PLE Reports`: PLE report generator in .txt and .xls file formats.
- :guilabel:`Partner Autocomplete`: automatically enriches contact base with corporate data.
- :guilabel:`Peruvian EDI`: an electronic data interchange service for sending electronic documents
  to the SUNAT.
- :guilabel:`Perú data`: extracts the contact data by RUC and DNI to get the exchange rate daily.
- :guilabel:`Pleo`: pulls expenses from Pleo.
- :guilabel:`SMS`: sends SMS Text Messages to contacts directly from the database.
- :guilabel:`Signer identification with itsme®️`: adds an identification step in Odoo Sign's
  signature flows and asks signatories to provide their identity through the itsme®️ identity
  platform and their mobile device.
- :guilabel:`Snailmail`: sends customer invoices and follow-up reports by post, worldwide.
- :guilabel:`eInvoice Service`: submits invoices to Invoice Registration Portal (IRP).
- :guilabel:`eWayBill Service`: **NEED CONTENT.**

.. _in_app_purchase/credits:

.. _Buy credits:

Buy credits
===========

Every time an IAP services is used, prepaid credits for that service are spent. After :ref:`creating
an IAP account <in_app_purchase/create>`, packs of credits can be purchased for that account through
the database.

.. example::
   The SMS service has four packs available for purchase, in denominations of **10** credits,
   **100** credits, **500** credits, and **1,000** credits. The number of credits consumed each time
   an SMS message is sent depends on the length of the SMS and the country of destination.

   For more information on SMS pricing, click `here <https://sms.api.odoo.com/iap/sms/pricing>`_.

To buy credits:

#. Open the :guilabel:`Settings app`, and search for :guilabel:`IAP`.
#. Click :guilabel:`View My Services`.
#. On the :guilabel:`IAP Account Settings` page, click the account that needs more credits.
#. On the :guilabel:`Account Information` page, click the :guilabel:`Buy Credits` button.
#. **NEED CONTENT - SERVER ERROR**

.. image:: in_app_purchase/settings.png
   :align: center
   :alt: General Settings and the In-App Purchases heading showing the View My Services button.

.. tip::
   Users who have the Enterprise version of Odoo Online get free credits to test IAP features.

.. _in_app_purchase/purchase:
