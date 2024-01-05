=================
Migration scripts
=================

A migration script is a Python file containing a function called :file:`migrate`, which the upgrade
process invokes at the appropriate time.

The :file:`migrate` function receives two parameters:

- :file:`cr`: The current database cursor.
- :file:`version`: The installed version of the module.

Typically, this function executes one or multiple SQL queries and can also access Odoo's ORM, as
well as the :doc:`../upgrade/upgrade_util`.

As described in :ref:`upgrade_custom/upgraded_database/migrate_data`, you might have to use
migration scripts to reflect changes from the source code to their corresponding data.


Writing migration scripts
=========================

To write what we refer as a **custom migration script**, the path of the file should follow this
template: :file:`<module_name>/migrations/<major_version>.<minor_version>/{pre|post|end}-*.py`.

Follow this steps to create the directory's structure and the Python file:

   #. Create a :file:`migrations` directory inside the custom module.
   #. Create a :file:`version` directory with the format: :file:`<major_version>.<minor_version>`
      inside the :file:`migrations` directory.

      - :file:`<major_version>` corresponds to the major version of Odoo (e.g., :file:`16.0` for
        Odoo 16).
      - :file:`<minor_version>` corresponds to the updated version declared on the module's
        manifest.

      Migration scripts are only executed when the module is being updated. Therefore, the
      :file:`minor_version` set in the directory needs to be higher than the module's installed
      version on the database and equal or lower to the updated version of the module.
      For example, in an `Odoo 16` database, with a custom module installed in version `1.1`, and an
      updated module version equal to `1.2`; the version directory should be `16.0.1.2`.
   #. Create a :file:`Python file` inside the :file:`version` directory named according to the
      :ref:`Phase <upgrade/migration-scripts/phases>` on which it needs to be executed. It should
      follow the template `{pre|post|end}-*.py`. The file name will determine the order in which it
      is executed for that module, version and phase.
   #. Create a :file:`migrate` function in the Python file that receives as parameters
      :file:`(cr, version)`.
   #. Add the logic needed inside the Python file.

.. example::

   Directory structure of a migration script for a custom module named `awesome_partner` upgraded
   to version `2.0.0` on Odoo 17

   .. code-block:: text

      awesome_partner/
      |-- migrations/
      |   |-- 17.0.2.0.0/
      |   |   |-- pre-exclamation.py

   Two migration scripts examples with the content of the :file:`pre-exclamation.py`, file adding
   "!" at the end of partners' names:

   .. code-block:: python

      import logging

      _logger = logging.getLogger(__name__)


      def migrate(cr, version):
          cr.execute("UPDATE res_partner SET name = name || '!'")
          _logger.info("Updated %s partners", cr.rowcount)

   .. code-block:: python

      import logging
      from odoo.upgrade import util

      _logger = logging.getLogger(__name__)


      def migrate(cr, version):
          env = util.env(cr)

          partners = env["res.partner"].search([])
          for partner in partners:
              partner.name += "!"

          _logger.info("Updated %s partners", len(partners))

   Note that in the second example, the script takes advantage of the :doc:`../upgrade/upgrade_util`
   to access the ORM. Check the documentation to find out more about this library.


Running and testing migration scripts
=====================================

.. tabs::

   .. group-tab:: Odoo Online

      As the instalation of custom modules containing Python files is not allowed on Odoo Online
      databases, it is not possible to run migration scripts on this platform. 

   .. group-tab:: Odoo.sh

      As explained on the `Odoo.sh` tab of :ref:`upgrade/request-test-database`, Odoo.sh is
      integrated with the upgrade platform.

      Once the upgrade of a staging branch is on "Update on commit" mode, each time a commit is
      pushed on the branch, the upgraded backup is restored and all the custom modules are updated.
      This update includes the execution of the migration scripts.

      When upgrading the production database, the execution of the migration scripts is also part of
      the update of the custom modules done by the platform when the upgraded database is restored.

   .. group-tab:: On-premise

      Once you receive the upgraded dump of the database from the `Upgrade platform
      <https://upgrade.odoo.com>`_, deploy the database and update all the custom modules by
      invoking the command :doc:`odoo-bin </developer/reference/cli>` in the shell.
      To update the custom modules, use the option: `-u <modules>,
      --update <modules>`.

      .. important::
         As mentioned in the :doc:`CLI documentation </developer/reference/cli>`, the command used
         to call the CLI depends on how you installed Odoo.


.. _upgrade/migration-scripts/phases:

Phases of migration scripts
===========================

The upgrade process consists of three phases for each version of each module:

  #. The pre-phase, before the module is loaded.
  #. The post-phase, after the module and its dependencies are loaded and upgraded.
  #. The end-phase, after all modules have been loaded and upgraded for that version.

Migration scripts are grouped according to the first part of their filenames into the corresponding
phase. Within each phase, the files are executed according to their lexical order.

.. note::
   If you are unsure which phase to use, use the end-phase.

.. spoiler:: Execution order of example scripts for one module in one version

   - :file:`pre-10-do_something.py`
   - :file:`pre-20-something_else.py`
   - :file:`post-do_something.py`
   - :file:`post-something.py`
   - :file:`end-01-migrate.py`
   - :file:`end-migrate.py`
