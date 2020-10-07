==================================
Creation of Custom Document Layout
==================================

You can create a document layout to customize business documents to fit your branding.

**See extension below for info specific to sales documents, e.g. sale order quotation, invoice, etc.**

1. Start with a bare-bone module.  Manifest is as follows.

.. code-block:: py

    # /{module_name}/__manifest__.py
    {
        ...
        'depends': [
            'account',
            'web',
        ],
        'data': [
            'views/report_templates.xml',
            'data/report_layout.xml',
        ],
        ...
    }

2. Create **report.layout** record

.. code-block:: xml

    <!-- /{module_name}/data/report_layout.xml -->
    <data>
        <record id="report_layout_custom" model="report.layout">
            <field name="view_id" ref="external_layout_custom"/>
            <field name="image">/${module_name}/static/img/icon.png</field>
            <field name="pdf">/${module_name}/static/pdf/example.pdf</field>
        </record>
    </data>


3. Create template with custom **external_layout** view, with id=external_layout_custom  (ref of view_id field)
The basic template structure is as follows.

.. code-block:: xml

    <!-- /{module_name}/views/report_templates.xml -->
    ...
    <template id="external_layout_custom">
        <!-- fixed to the top of the pdf view -->
        <div class="header" t-att-style="report_header_style">
            <!-- customized header content -->
        </div>

        <!-- document content -->
        <div class="article" t-att-data-oe-model="o and o._name" t-att-data-oe-id="o and o.id" t-att-data-oe-lang="o and o.env.context.get('lang')">
            <t t-raw="0"/>
        </div>

        <!-- fixed to the bottom in pdf -->
        <div class="footer">
            <!-- customized footer content -->
        </div>
    </template>


4. important to note, add link to new scss file.

.. code-block:: xml

    <!-- /{module_name}/views/report_templates.xml -->
    ...
    <template id="report_assets_custom" inherit_id="web.report_assets_common">
        <xpath expr="//link[last()]">
            <link rel="stylesheet" type="text/scss"
                  href="/{module_name}/static/src/scss/{scss_file_name}.scss"/>
        </xpath>
    </template>

Extension: Update Sales Document Template
=========================================

1. Update manifest, and create the corresponding folder & files

.. code-block:: py

    # __manifest__.py
    {
        ...
        'depends': [
            'account',
            'web',
            'sale',
        ],
        'data': [
            ...
            'report/sale_report_templates.xml',
            'views/report_invoice.xml',
        ],
        ...
    }

2a. Update sale order and quotation, use xpath tags to make changes to the template

.. code-block:: xml

    <!-- /{module_name}/report/sale_report_template.xml -->
    <template id="report_saleorder_document_custom"
              inherit_id="sale.report_saleorder_document">
        <xpath></xpath>
    </template>

2b. Update invoice

.. code-block:: xml

    <!-- /{module_name}/views/report_invoice.xml -->
    <template id="report_invoice_document_custom"
              name="custom template invoice"
              inherit_id="account.report_invoice_document">
        <xpath></xpath>
    </template>
