====================
Upgrade Util package
====================

The `Upgrade Util package <https://github.com/odoo/upgrade-util/>`_ is a library that contains
helper functions to facilitate the writing of upgrade scripts. This package, used by Odoo for the
migration scripts of standard modules, provides reliability and helps speed up the upgrade process:

- The helper functions make sure the data is consitent in your database.
- It takes care of indirect references of the updated records.
- Allows calling functions and avoid writing code, saving time and reducing development risks.
- Helpers allow to focus on what is important for the upgrade and not think of details.


Installation
============

Once you have clone this repository locally, just start `odoo` with the `src` directory prepended to
the `--upgrade-path` option.

.. code-block:: console

   $ ./odoo-bin --upgrade-path=/path/to/upgrade-util/src,/path/to/other/upgrade/script/directory [...]

On platforms where you do not manage Odoo yourself, you can install this package via `pip`:

.. code-block:: console

   $ python3 -m pip install git+https://github.com/odoo/upgrade-util@master

On `Odoo.sh <https://www.odoo.sh/>`_ it is recommended to add it to the :file:`requirements.txt` of
your repository. For this, add the following line inside the file::

   odoo_upgrade @ git+https://github.com/odoo/upgrade-util@master

Using the Util package
======================

Once installed, the following packages are available for the migration scripts:

- :file:`odoo.upgrade.util`: the helper themself.
- :file:`odoo.upgrade.testing`: base TestCase classes.

To use it migration scripts, simply import it:

.. code-block:: python

   from odoo.upgrade import util


   def migrate(cr, version):
      # Rest of the script

Now, the helper functions are available to be called through `util`.

Util functions
==============

The util package provides many useful functions to ease the upgrade process. Here, we describe some
of the most useful ones. Refer to the `upgrade-util repository
<https://github.com/odoo/upgrade-util/tree/master/src/util>`_ for the comprehensive declaration of
helper functions.

Fields
------

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/fields.py#L91>`_
.. method:: remove_field(cr, model, fieldname[, cascade=False][, drop_column=True][, skip_inherit=()])

   Remove a field and its references from the database

   :param cr: Database cursor. Use the one from the migration script
   :param str model: The field's model name
   :param str fieldname: The field's name
   :param bool cascade: If True, removes field's column and inheritance in CASCADE
   :param bool drop_column: If True, drops the field's column
   :param list(str) or str skip_inherit: list of models whose field's inheritance is skipped.
      Use `"*"` to skip all inheritances

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/fields.py#L362>`_
.. method:: rename_field(cr, model, old, new[, update_references=True][, domain_adapter=None][, skip_inherit=()])

   Rename a field and its references from `old` to `new`

   :param cr: Database cursor. Use the one from the migration script
   :param str model: The field's model name
   :param str old: The field's current name
   :param str new: The field's new name
   :param bool update_references: If True, Replace all references of field `old` to `new` in:
      `ir_filters`, `ir_exports_line`, `ir_act_server`, `mail_alias`, `ir_ui_view_custom
      (dashboard)`, `domains (using "domain_adapter")`, `related fields`
   :param function domain_adapter: function that takes three arguments and returns a domain that
      substitutes the original leaf: (leaf: Tuple[str,str,Any], in_or: bool, negated: bool) ->
      List[Union[str,Tuple[str,str,Any]]]
   :param list(str) or str skip_inherit: list of models whose field's inheritance is skipped.
      Use `"*"` to skip all inheritances

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/fields.py#L337>`_
.. method:: move_field_to_module(cr, model, fieldname, old_module, new_module[, skip_inherit=()])

   Move a field's refenrence in `ir_model_data` table from one `old_module` to `new_module`

   :param cr: Database cursor. Use the one from the migration script
   :param str model: The field's model name
   :param str fieldname: The field's name
   :param str old_module: The field's current module name
   :param str new_module: The field's new module name
   :param list(str) or str skip_inherit: list of models whose field's inheritance is skipped.
      Use `"*"` to skip all inheritances

Models
------

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/models.py#L53>`_
.. method:: remove_model(cr, model[, drop_table=True][, ignore_m2m=()]):

   Remove a model and its references from the database

   :param cr: Database cursor. Use the one from the migration script
   :param str model: The model name
   :param bool drop_table: If True, drops the model's table
   :param list(str) or str ignore_m2m: list of m2m tables ignored to remove. Use `"*"` to ignore all
      m2m tables.

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/models.py#L203>`_
.. method:: rename_model(cr, old, new[, rename_table=True])

   Rename a model and its references from `old` to `new`

   :param cr: Database cursor. Use the one from the migration script
   :param str old: The model's current name
   :param str new: The model's new name
   :param bool rename_table: If True, renames the model's table

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/models.py#L323>`_
.. method:: merge_model(cr, source, target[, drop_table=True][, fields_mapping=None][, ignore_m2m=()])

   Merge the references from `source` model into `target` model and removes `source` model and its
   references. By default, only the fields with the same name in both models are mapped.

   .. warning::
      This function does not move the records from `source` model to `target` model.

   :param cr: Database cursor. Use the one from the migration script
   :param str source: The source model's name
   :param str target: The target model's name
   :param bool drop_table: If True, drops the source model's table
   :param dict fields_mapping: Dictionary mapping fields with different names on both models. The
      format of the dictionary is:

      .. code-block:: python

         {
            "source_model_field_1": "target_model_field_1",
            "source_model_field_2": "target_model_field_2",
            ...
         }
   :param list(str) or str ignore_m2m: list of m2m tables ignored to remove from source model.

Modules
-------

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/modules.py#L218>`_
.. method:: remove_module(cr, module):

   Uninstall and remove a module and its references from the database

   :param cr: Database cursor. Use the one from the migration script
   :param str module: The module name

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/modules.py#L263>`_
.. method:: rename_module(cr, old, new)

   Rename a module and its references from `old` to `new`

   :param cr: Database cursor. Use the one from the migration script
   :param str old: The module's current name
   :param str new: The module's new name

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/modules.py#L323>`_
.. method:: merge_module(cr, old, into, update_dependers=True)

   Move all references of module `old` into module `into`

   :param cr: Database cursor. Use the one from the migration script
   :param str old: The source model's name
   :param str into: The target model's name
   :param bool update_dependers: If True, updates the dependencies of modules that depends on `old`

ORM
---

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/orm.py#L43>`_
.. method:: env(cr)

   Create a new environment from cursor.

   .. warning::
      This function does NOT empty the cache maintained on the cursor for superuser with and empty
      environment. A call to invalidate_cache will most probably be necessary every time you
      directly modify something in database

   :param cr: Database cursor. Use the one from the migration script
   :return: The new environment
   :rtype: odoo.api.Environment

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/orm.py#L218>`_
.. method:: recompute_fields(cr, model, fields[, ids=None][, logger=_logger][, chunk_size=256][, strategy="auto"])

   Recompute field values.

   :param cr: Database cursor. Use the one from the migration script
   :param str model: The fields' model name
   :param list(str) fields: List of fields to recompute
   :param list(int) ids: List of records' IDs to recompute
   :param Logger logger: Logger used to print the progress of the function
   :param int chunk_size: Size of the chunk used to split the records fof better processing
   :param str strategy: Strategy used to process the recomputation. Default: `auto`.
      Possible values:

      - `flush`: Flush the recomputation as when it's finished
      - `commit`: Commit the recomputation as when it's finished
      - `auto`: The function chooses the best alternative for the recomputation based on the number
        of records to recompute and the fields traceability.

Records
-------

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/records.py#L612>`_
.. method:: ref(cr, xmlid)

   Return the id corresponding to the given `xml_id`.

   :param cr: Database cursor. Use the one from the migration script
   :param str xml_id: Record xml_id, under the format `<module.id>`
   :return: Found record id or None
   :rtype: int

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/records.py#L281>`_
.. method:: remove_record(cr, name)

   Remove a record and its references corresponding to the given `xml_id`.

   :param cr: Database cursor. Use the one from the migration script
   :param str name: record xml_id, under the format `<module.id>`

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/records.py#L548>`_
.. method:: rename_xmlid(cr, old, new[, noupdate=None][, on_collision="fail"])

   Rename the external Identifier of a record.

   :param cr: Database cursor. Use the one from the migration script
   :param str old: Record's current xml_id, under the format `<module.id>`
   :param str new: Record's new xml_id, under the format `<module.id>`
   :param bool noupdate: Value to set on the ir_model_data record `noupdate` field. Default: `None`
   :param str on_collision: Action to take if the new xml_id already exists. Default: `fail`

      - `fail`: Flush the recomputation as when it's finished
      - `merge`: Commit the recomputation as when it's finished

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/records.py#L652>`_
.. method:: ensure_xmlid_match_record(cr, xmlid, model, values)

   Match a record with an xmlid. Create or update the xmlid of a record. Useful when migrating
   in-database records into a custom module, to create the record's xmlid before the module is
   updated and avoid duplication.

   :param cr: Database cursor. Use the one from the migration script
   :param str xmlid: Record xml_id, under the format `<module.id>`
   :param str model: Record's model name
   :param dict values: Dictionary mapping fields and values to search for the record update.
      The format of the dictionary is:

      .. code-block:: python

         {
            "fieldname_1": "value_1",
            "fieldname_2": "value_2",
            ...
         }

      For example:

      .. code-block:: python

         values = {"id": 123}
         values = {"name": "INV/2024/0001", company_id: 1}

   :return: The record's xmlid.
   :rtype: str

.. `[source] <https://github.com/odoo/upgrade-util/blob/master/src/util/records.py#L720>`_
.. method:: update_record_from_xml(cr, xmlid[, reset_write_metadata=True][, force_create=False][, from_module=None][, reset_translations=()])

   Update a record based on its definition on the :doc:`../../reference/backend/data`. Useful to
   update `no update` records, in order to reset them for the upgraded version.

   :param cr: Database cursor. Use the one from the migration script
   :param str xmlid: The record's current xml_id, under the format `<module.id>`
   :param bool reset_write_metadata: If True, the metadata before the record update is kept.
      Default: `True`
   :param bool force_create: If True, creates the record if it does not exist. Default: `False`
   :param str from_module: Update the record base on a module different than the one referenced in
      the xml_id. Useful when the record is inherited in another module.
   :param set of str reset_translations: Set of field names which translations get reset.
