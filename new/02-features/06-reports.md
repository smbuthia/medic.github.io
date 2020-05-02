# Reports: Data & Report Management

## Overview

The Reports tab is where you can access any data that was submitted. Depending on how often you anticipate a user needing to access this tab, you can configure it to show in the main tabs list (preferable for admin users) or in the secondary hamburger menu (preferably for CHW users). 

The permissions set for your role and your placement in the hierarchy will determine which reports you’re able to see on this tab. As a rule, you can only view reports submitted by yourself or those below you in the hierarchy. Therefore, CHWs will only see reports that they submitted on this tab, while supervisors will see reports that they submitted as well as those submitted by their CHWs.

## Main List

The first line of bold text is the name of the person whom the report is about. The second line of text is the name of the report, and the third line of text is the hierarchy of place to which that person belongs. In the upper right corner, a timestamp displays when the report was submitted. Reports are sorted by submission date, with the most recent reports at the top. If a report is unread, the timestamp will be bold blue and there will be a horizontal blue line above it. 

We have a “review” feature that allows managers to indicate whether a report has been reviewed and if it contains errors. If a manager has marked a report as “correct,” a green checkmark will show below the timestamp. If a report is marked as “has errors,” a red ‘X’ will show. This same icon is used for invalid SMS messages.

## Filters & Search

The toolbar at the top of the page includes filters and search to help users narrow down the list or search for and find a specific report. These filters are configurable and could include:

- **Report Types** (e.g. pregnancy registration, visits, delivery report)
- **Places** (e.g. districts, health centers or CHW areas)
- **Date of Submission**
- **Status** (e.g. not reviewed, has errors, correct, valid SMS, invalid SMS)

Using the search box, you can search for reports by patient name, phone number, ID number and more. To reset the filters or the search and view  the full list of forms, click on the reset icon on the right side of the toolbar.

## Action Buttons

The action buttons at the bottom of the screen are configurable. Options include adding or completing a care guide, bulk select & export. 

Clicking the “+” button opens a menu of forms a user can choose to complete. “Bulk Select,” represented by a checkmark icon within a circle, allows you to bulk select and delete multiple reports at a time.

**Please Note**: Bulk delete cannot be undone. If in doubt, do not delete! You can restrict a user’s access to this feature in the permissions for their role. 

Clicking on the “Export” button will download a CSV file with all of the data from the reports.

## Detail Pages

You can click on any report to view a report detail page. Here you'll find the name and phone number of the user who submitted the report as well as responses to the questions within it. If the report initiated a schedule of SMS messages, you will see the messages queued to send.

The buttons at the bottom are configurable. The ones you see will depend on your user role, permissions, and hierarchy. 

- **Send a Message**​: Opens the Messages page to send a message to the person who submitted the report
- **Review**: Mark as “correct” or “has errors”
- **Edit**: Opens the form to edit it
- **Delete**: Deletes a report ( cannot be undone)

<details>
  <summary><em>Details for app developers</em></summary>

> **Defining Forms**

> The Reports tab shows all reports, which are completed and submitted forms. App forms are those that can be completed within the app (eg as a completed task, or an action on a contact's profile or reports tab), and are defined as XForms. Reports coming from other channels (via SMS or interoperability with other tools) are defined as JSON forms. 
> 
> ### App Forms
> Used for forms within the web app, whether accessed in browser or via the Android app. App forms are defined by the following files:
> 
> - A XLSForm form definition, converted to the XForm (optional)
> - A XML form definition using the ODK XForm format
> - Meta information in the `{form_name}.properties.json` file (optional)
> - Media files in the `{form_name}-media` directory (optional)
> 
> #### XForm
> 
> The ODK XForm standard is supported. Data needed during the completion of the form (eg patient's name, prior information) is passed into the `inputs` group. Reports that have at least one of `place_id`, `patient_id`, and `patient_uuid` at the top level will be associated with that contact. Additionally, some custom XLSForm types and appearances are available.
> 
> #### Additional XForm Widgets
> 
> Some XForm widgets have been added or modified for use in the app:
> - **Bikram Sambat Datepicker**: Calendar widget using Bikram Sambat calendar. Used by default for appropriate languages.
> - **Countdown Timer**: A visual timer widget that starts when tapped/clicked, and has an audible alert when done. To use it create a `string` field with an `appearance` set to `countdown-timer`. The duration of the timer is the field's value, which can be set in the XLSForm's `default` column. If this value is not set, the timer will be set to 60 seconds.
> - **Contact Selector**: Select a contact, such as a person or place, and save their UUID in the report. To use create a field of type `db:{{contact_type}}` (eg `db:person`, `db:clinic`) with appearance `db-object`.
> - **Rapid Diagnostic Test capture**: Take a picture of a Rapid Diagnotistic Test and save it with the report. Works with [rdt-capture Android application](https://github.com/medic/rdt-capture/). To use create a string field with appearance `mrdt-verify`.
> - **Simprints registration**: Register a patient with the Simprints biometric tool. To include in a form create a `string` field with `appearance` of `simprints-reg`. Requires the Simprints app connected with hardware, or [mock app](https://github.com/medic/mocksimprints). Demo only, not ready for production since API key is hardcoded.
> 
> The code for these widgets can be found in the [Medic repo](https://github.com/medic/medic/tree/master/webapp/src/js/enketo/widgets).
> 
> #### Additional XPath Functions
> 
> ##### `difference-in-months`
> 
> Calculates the number of whole calendar months between two dates. This is often used when determining a child's age for immunizations or assessments.
> 
> ##### `z-score`
> 
> In Enketo forms you have access to an XPath function to calculate the z-score value for a patient.
> 
> 
> #### App Form Properties
> 
> The meta information in the `{form_name}.properties.json` file defines the form's title and icon, as well as when and where the form should be available.
> 
> | property | description | required |
> |---|---|---|
> | `title`| The form's title seen in the app. Uses a localization array using the 2-letter code, not the translation keys discussed in the [Localization section](#localization). | yes |
> | `icon` | Icon associated with the form. The value is the image's key in the `resources.json` file, as described in the [Icons section](#icons) | yes |
> | `context` | The context defines when and where the form should be available in the app | no |
> | `context.person` | Boolean determining if the form can be seen in the Action list for a person's profile. This is still subject to the `expression`. | no |
> | `context.place` | Boolean determining if the form can be seen in the Action list for a person's profile. This is still subject to the `expression`. | no |
> | `context.expression` | A JavaScript expression which is evaluated when a contact profile or the reports tab is viewed. If the expression evaluates to true, the form will be listed as an available action. The inputs `contact`, `user`, and `summary` are available. By default, forms are not shown on the reports tab, use `"expression": "!contact"` to show the form on the Reports tab since there is no contact for this scenario. | no |
> 
> ### JSON forms
> Used for parsing reports from formatted SMS, SIM applications, and Medic Collect. JSON form definitions are also used for interoperability with third-party systems. Each form is defined as an JSON form object according to the following schema. 
>
> | property | type | description | required |
> |---|---|---|---|
> | `meta` | object | Information about the report. | yes |
> | `meta.code` | string | The unique form identifier, which will be sent with all reports of this form. Must be the same as the form's key. | yes |
> | `meta.icon` | string | Name of the icon resource shown in the app. | no |
> | `meta.translation_key` | string | Name of the form shown in the app. | no |
> | `fields`| object | Collection of field objects included in the form. | yes |
> | `fields.${field}` | object | Field that is part of the form. | yes |
> | `fields.${field}.type` | string | Data type of field:<br>  - `"integer"`: a whole number<br> - `"string"`: any collection of characters<br> - `"date"`: a date in the format `YYYY-mm-dd`, for example "2019-01-28"<br> - `"boolean"`: true or false, represented by the digit `1` and `0` respectively (native JSON booleans are also supported if sending via JSON)<br> - `"custom"`: Only possible for JSON forms that are passed as actual JSON (not an SMS that gets parsed into JSON). Effectively any non-specific data structure, which will be taken without validation. | yes |
> | `fields.${field}.labels` | object | | no |
> | `fields.${field}.labels.short` | string, object with `translation_key` field, or translation object | Label shown for field in the app, seen when report is viewed in Reports tab. If missing, label will default to a translation key of `report.${form_name}.${field_name}` (eg `report.f.patient_id`) which can be translated in the app languages. | no |
> | `fields.${field}.labels.tiny` | string | Unique identifier within the form for this field. Used in form submission to bind values to fields. Not required for all submission formats. | no |
> | `fields.${field}.position` | integer | Zero based order of this field for incoming reports. | no |
> | `fields.${field}.flags` | object | Additional instructions that could be used by form renderers. For instance `{ "input_digits_only": true }` indicated to SIM applications to show the number keyboard. Obsolete. | no |
> | `fields.${field}.length` | array with two integers | Inclusive range accepted for length of the field. | no |
> | `fields.${field}.required` | boolean | Determines if a report without this field is considered valid. | no |
> | `public_form` | boolean | Determines if reports will be accepted from phone numbers not associated to a contact. Set to false if you want to reject reports from unknown senders. Default: true. | no |
> | `facility_reference` | string | The form field whose value is to be used to match the incoming report to a contact's `rc_code`. Useful when reports are sent on behalf of a facility by unknown or various phone numbers. Requires the [`update_clinics` transition](transitions.md#available-transitions). | no |
> 
</details>