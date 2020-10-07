==================
Get and Set Params
==================

This provides a basic guide to get and set params in the models, and display them in the corresponding views.



Set params
----------
In an instance method of a model, use ``set_param(key, value)``, as follows.
Be sure to update the ``__init__.py`` files by importing the models

.. code-block:: py

    @api.multi
    def set_values(self):
        res = super(ResConfigSettings, self).set_values()
        param = self.env['ir.config_parameter'].sudo()
        param.set_param('{module}.{field}', self.{field})
        return res


Get params
----------
In an instance method of a model, use ``get_param(key, dafault_value)``, as follows.

.. code-block:: py

    @api.model
    def get_something(self):
        result = self.env['ir.config_parameter'].sudo().get_param(
        '{module}.{field}', default='{default_value}')
        return result

Display in View
_______________

In the view (xml), you can call the method

.. code-block:: xml

    ...
    <t t-set="{var name}}" t-value="record.get_something()"/>

