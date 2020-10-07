========================
Customize Email template
========================

You can customize email layout to fit your branding.

**Let's use sales documents and invoice email as an example.**

1. Start with a bare-bone module.  Manifest is as follows.

.. code-block:: py

    # /{module_name}/__manifest__.py
    {
        ...
        'depends': [
            'mail',
        ],
        'data': [
            'data/mail_data.xml',
        ],
        ...
    }

2. Update template **mail.mail_notification_paynow**. Use xpath tags to make the update.

.. code-block:: xml

    <!-- /{module_name}/data/mail_data.xml -->
    <template id="mail_notification_paynow_custom"
              name="Mail: Pay Now mail notification template - custom"
              inherit_id="mail.mail_notification_paynow">

        <!-- example: change the styling of an a tag -->
        <xpath expr="//tbody/tr[2]/td/div[1]/a" position="attributes">
            <attribute name="t-att-style">'padding: 8px 16px 8px 16px;'</attribute>
        </xpath>

    </template>
