Training
========

Below are mini-challenges for developers to start familiarize with Odoo's MVC architecture. V12.0.

Challenge 1: Model & View
#########################

Create an installable module, *helpdesk_note*, that adds a field, *note_3*, to an existing modules, *helpdesk_mgmt*, and display it in the coresponding form view!

*Specification*

  * Existing Module: helpdesk_mgmt
  * New Module: helpdesk_note
  * Field Label: Note_3
  * Field Display: Note
  * Field Type: HTML
  * Field position in the Form View: Under channel_id


*Key Learning Points*

  * Inheritance: how to inherit from an existing Model & View
  * Field: how to add a new instance variable to a model.
  * Module: how to set up your module (i.e. manifest.py, etc)



Answer
------




Model: *helpdesk.py* ::

  from odoo import api, fields, models


  class HelpdeskTicket(models.Model):
    _inherit = 'helpdesk.ticket'
        
    note_3 = fields.Html(
        string="Note", 
        required=False, 
        sanitize_style=True
    )

View: *helpdesk_ticket.xml* ::

  <odoo>

      <record id="view_helpdesk_ticket_note" model="ir.ui.view">
          <field name="name">Add Note on Helpdesk Ticket</field>
          <field name="model">helpdesk.ticket</field>
          <field name="inherit_id" ref="helpdesk_mgmt.ticket_view_form"/>
          <field name="arch" type="xml">
              <field name="channel_id" position="after">
                  <field name="note_3"/>
              </field>
          </field>
      </record>

  </odoo>



Challenge 2: Adding Controller
##############################

Build on Challenge 1, add the field, *note_3*, (From *helpdesk_mgmt*) to the Customer Portal.

*Specification*

  * Field: partner_id (to assign the ticket to a client)
  * Module: use the same module as Challenge 1


*Key Learning Points*

  * Inheritance: how to inherit a controller

.. note::
  [TIP] To preview customer's portal view, you can install *smile_website_login_as*. Click on the earth icon on the header & select customer of your choosing.


Answer
------

Controller: *myaccount.py* ::

  from odoo import http
  from odoo.addons.portal.controllers.portal import CustomerPortal


  class CustomerPortalNote(CustomerPortal):

      def _ticket_get_page_view_values(self, ticket, **kwargs):
          values = super(CustomerPortalNote, self)._ticket_get_page_view_values(ticket, **kwargs)
          return values


View: *helpdesk_ticket_templates.xml* ::

  <odoo>

    <template id="portal_my_tickets_note" 
              name="portal tickets table view with note"
              inherit_id="helpdesk_mgmt.portal_my_tickets">
      <xpath expr="//table/thead/tr/th[3]" position="after">
        <th>Note</th>
      </xpath>
      <xpath expr="//table/t/tr/td[3]" position="after">
        <td>
          <t t-raw="ticket.note_3"/>
        </td>
      </xpath>
    </template>
    
    <template id="portal_helpdesk_ticket_page_note"
              name="Ticket Portal Template With Note"
              inherit_id="helpdesk_mgmt.portal_helpdesk_ticket_page">
      <xpath expr="//div[hasclass('pull-left')]" position="inside">
        <b>Note:</b> <t t-raw="ticket.note_3"/><br/>
      </xpath>
    </template>
    
  </odoo>
