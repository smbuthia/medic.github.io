# Application Basics 

## Care Guides

We use forms to build “Care Guides” that take health workers through care protocols and provide decision support for their interactions with patients. App designers can use basic form building functionality in a variety of ways. 

Care Guides also allow CHWs to register new families and people, assess a sick child, and enroll a new pregnancy into an antenatal care schedule. Care Guides can live in many parts of the app including the Tasks, People, and Reports tabs. 

During app development, Care Guides can be written from scratch or based on those from a reference application. These variations are often necessary due to different local requirements, government protocols, etc.

### Functionality

Care Guides consists of questions grouped into pages. They are capable of presenting many different types of questions, skip logic, images, and videos. Validation rules can require certain questions to be answered or restrict answers to a specified type or range. 

It’s possible to reference previous information that was submitted about the person or household from within the care guide. The interaction can also conclude with a summary that includes assessment results, treatment recommendations, and referral info. 

Care Guides can include images for instructional purposes and can access a user’s camera to take a photo if needed.

### Summary

After all of the required questions have been answered, a summary page can be displayed. 

Here, health workers can review the information they entered, receive instructions for treatment, care, and referrals, and relay detailed education to the patient.

**Please Note:** The form is not submitted until the user scrolls to the very end of the summary and clicks the “Submit” button.

### Examples

- While a health worker is going through the form during the care visit, you can include a family planning question only if the person who the form is about is a woman and not pregnant.
- You can include on-the-spot conversational prompts and advice for the CHW based on how they answer questions in the form. For instance, if a CHW answers “yes” to the question about a woman’s interest in family planning, text can automatically appear to provide information on her options.
- An image showing how to read a rapid test can be displayed within a form, to help health workers to correctly interpret their test results.

## Build Workflows

Forms are the main building block of workflows, because they can be configured to initiate a schedule of tasks, such as follow-up visits for the CHW or a referral to be completed by a nurse. 

Data submitted in one form can generate several tasks at once, e.g., multiple ANC visits following one pregnancy registration. Some workflows involve a series of sequential forms and tasks, e.g., a child health assessment form, a follow up task scheduled 48 hours later, a referral form (only if the child’s condition hasn’t improved), and then a referral follow up task. Tasks are accessible on the Tasks tab, as well as the Tasks section of profiles. 

Workflows can also include SMS notifications and interactions with CHWs, nurses, supervisors, and patients. A submitted form can trigger an SMS to be sent immediately or upon a set schedule. Responses via SMS or the app can update the workflows.

<details>
  <summary><em>Details for app developers</em></summary>

> **Building Workflows**
> 
> <!-- TODO: Revise this to be about workflows and forms -->
> Whether using your application in the browser or via an Android app, all Contact creation/edit forms, care guides for decision support, and surveys are defined using [ODK XForms](https://opendatakit.github.io/xforms-spec/) -- a XML definition of the structure and format for a set of questions. Since writing raw XML can be tedious, we suggest creating the forms using the [XLSForm standard](http://xlsform.org/), and using the [medic-conf](https://github.com/medic/medic-conf) command line configurer tool to convert them to XForm format. The instructions on this site assume some knowledge of XLSForm, and distinguish between Contact forms (those used to create and edit contacts), other App forms (which are used for care guides and surveys). Information about Medic-specific XForm features, and form definitions for Medic Collect and SMS can be found in the broader [Forms documentation](https://github.com/medic/medic-docs/blob/master/configuration/forms.md). <!-- TODO: Link to migrated content on Contact forms, App forms, and JSON forms-->
>
> ### Building workflows with Tasks
> 
> Tasks in the app can drive a workflow, ensuring that the right actions are taken for people at the right time. Tasks indicate a recommended action to the user. They indicate who the user should perform the action with and the recommended timeframe of that action. When the user taps the task, they are directed to a form where the details of the action are captured.
> 
> Tasks can be triggered by a set of conditions, such as contact details or submitted reports. Tasks are accessible in the Tasks tab and the profile in the Contact tab, and initiate a follow up action to complete a form. More information on building app workflows is available in the [Tasks section]().
>
> ### Building workflows with SMS
> 
> The Medic platform can be set up to send automated messages at specificied times in the future. To set this up a form must be defined in `app_settings.json` as a registration form, then trigger a particular set of scheduled messages. Forms can also be configured to clear the schedule, or silence it for a period of time.
> 
> #### Scheduled messages
> 
> Scheduled messages are defined under the `schedules` key as an array of schedule objects. The definition takes the typical form below:
> 
> ```json
>   "schedules": [
>     {
>       "name": "ANC Visit Reminders",
>       "summary": "",
>       "description": "",
>       "start_from": "lmp_date",
>       "start_mid_group": true,
>       "messages": [
>         {
>           "translation_key": "messages.schedule.registration.followup_anc_pnc",
>           "group": 1,
>           "offset": "4 weeks",
>           "send_day": "monday",
>           "send_time": "09:00",
>           "recipient": "reporting_unit"
>         }
>       ]
>     }
> ]
> ```
> 
> |property|description|required|
> |-------|---------|----------|
> |`name`|A unique string label that is used to identify the schedule. Spaces are allowed.|yes|
> |`summary`|Short description of the of the schedule.|no|
> |`description`|A narrative for the schedule.|no|
> |`start_from`|The base date from which the `messages[].offset` is added to determine when to send individual messages. You could specify any property on the report that contains a date value. The default is `reported_date`, which is when the report was submitted.|no|
> |`start_mid_group`|Whether or not a schedule can start mid-group. If not present, the schedule will not start mid-group. In other terms, the default value is `false`|no|
> |`messages`|Array of objects, each containing a message to send out and its properties.|yes|
> |`messages[].translation_key`|The translation key of the message to send out. Available in 2.15+.|yes|
> |`messages[].messages`| Array of message objects, each with `content` and `locale` properties. From 2.15 on use `translation_key` instead.|no|
> |`messages[].group`|Integer identifier to group messages that belong together so that they can be cleared together as a group by future reports. For instance a series of messages announcing a visit, and following up for a missed visit could be grouped together and cleared by a single visit report. |yes|
> |`messages[].offset`| Time interval from the `start_from` date for when the message should be sent. It is structured as a string with an integer value followed by a space and the time unit. For instance `8 weeks` or `2 days`. The units available are `seconds`, `minutes`, `hours`, `days`, `weeks`, `months`, `years`, and their singular forms as well. Note that although you can specify `seconds`, the accuracy of the sending time will be determined by delays in the processing the message on the server and on the gateway.|yes|
> |`messages[].send_day`| String value of the day of the week on which the message should be sent. For instance, to send a message at the beginning of the week setting it to `"Monday"` will make sure the message goes out on the closest Monday _after_ the `start_date` + `offset`. |no|
> |`messages[].send_time`| Time of day that the message should be sent in 24 hour format.|no|
> |`messages[].recipient`| Recipient of the message. It can be set to `reporting_unit` (sender of the form), `clinic` (clinic that the sender of the form is attached to), `parent` (parent of the sender of the form), or a specific phone number.|no|
> 
> #### Registrations
> 
> Under the `registrations` key in app_settings, we can setup triggers for scheduled messages. A trigger for the schedule above can be defined as shown below:
> 
> ```json
> 
> "registrations": [
>   {
>     "form": "PR",
>     "events": [
>       {
>         "name": "on_create",
>         "trigger": "assign_schedule",
>         "params": "ANC Visit Reminders",
>         "bool_expr": "doc.fields.last_menstrual_period"
>       }
>     ],
>     "validations": {},
>     "messages": []
>   }
> ]
> 
> ```
> 
> |property|description|required|
> |-------|---------|----------|
> |`form`|Form ID that should trigger the schedule.|yes|
> |`events`|An array of event object definitions of what should happen when this form is received.|yes|
> |`event[].name`|Name of the event that has happened. The only supported event is `on_create` which happens when a form is received.|yes|
> |`event[].trigger`|What should happen after the named event. `assign_schedule` will assign the schedule named in `params` to this report. Similarly `clear_schedule` will permanently clear all messages for a patient that are part of schedules listed in the `params` field. The full set of trigger configuration directives are described [here](https://github.com/medic/medic-docs/blob/master/configuration/transitions.md#triggers).|yes|
> |`event[].params`|Any useful information for the event. In our case, it holds the name of the schedule to be triggered.|no|
> |`event[].bool_expr`|A JavaScript expression that will be cast to boolean to qualify execution of the event. Leaving blank will default to always true. CouchDB document fields can be accessed using `doc.key.subkey`. Regular expressions can be tested using `pattern.test(value)` e.g. /^[0-9]+$/.test(doc.fields.last_menstrual_period). In our example above, we're making sure the form has an LMP date.|no|
> |`validations`|A set of validations to perform on incoming reports. More information about validation rules can be found [here](app-settings-validations.md).|no|
> |`validations.join_responses`|A boolean specifying whether validation messages should be combined into one message.|no|
> |`validations.list[]`|An array of validation rules a report should pass to be considered valid.|no|
> |`validations.list[].property`|Report field for which this validation rule will be applied.|no|
> |`validations.list[].rule`|Validation condition to be applied to the property field. More information about rules can be found [here](app-settings-validations.md#rules).|no|
> |`validations.list[].translation_key`|Translation key for the message reply to be sent if a report fails this rule.|no|
> |`messages`|An array of automated responses to incoming reports.|no|
> |`messages[].translation_key`|Translation key for the message text associated with this event.|no|
> |`messages[].event_type`|An event that will trigger sending of this message. Typical values are: `report_accepted` when the report has been successfully validated and `registration_not_found` when the patient ID supplied in the report doesn't match any patient ID issued by Medic.|no|
> |`messages[].recipient`|Who the message should be sent to. Use `reporting_unit` for the sender of the report, `clinic` for clinic contact, and `parent` for the parent contact.|no|
> 
> #### Patient Reports
> 
> Under the `patient_reports` key in app_settings, we can setup actions to take for other form submissions. A patient report can be defined as shown below:
> 
> ```json
>   "patient_reports": [
>     {
>       "form": "V",
>       "name": "Visit (SMS)",
>       "format": "V <patientid>",
>       "silence_type": "ANC Reminders, ANC Reminders LMP, ANC Reminders LMP from App",
>       "silence_for": "8 days",
>       "fields": [
>         {
>           "field_name": "",
>           "title": ""
>         }
>       ],
>       "validations": {
>         "join_responses": true,
>         "list": [
>           {
>             "property": "patient_id",
>             "rule": "regex('^[0-9]{5,13}$')",
>             "translation_key": "messages.generic.validation.patient_id"
>           }
>         ]
>       },
>       "messages": [
>         {
>           "translation_key": "messages.v.report_accepted",
>           "event_type": "report_accepted",
>           "recipient": "reporting_unit"
>         },
>         {
>           "translation_key": "messages.generic.registration_not_found",
>           "event_type": "registration_not_found",
>           "recipient": "reporting_unit"
>         }
>       ]
>     }
>   ]
> ```
> 
> |property|description|required|
> |-------|---------|----------|
> |`form`|Form ID of the patient form.|yes|
> |`name`|Descriptive name of the form. This is not currently used in the app, but can be a helpful annotation.|no|
> |`format`|Guide of how the form can be used. This is not currently used in the app, but can be a helpful annotation.|no|
> |`silence_type`|A comma separated list of schedules to mute.|no|
> |`silence_for`|Duration from when the report was submitted for which messages should be muted. It is structured as a string with an integer value followed by a space and the time unit. For instance `8 weeks` or `2 days`. The units available are `seconds`, `minutes`, `hours`, `days`, `weeks`, `months`, `years`, and their singular forms as well. When a message is muted all messages belonging to the same group will be muted, even if it falls outside of this time period. See `messages[].group` in _Schedules_ for related info.|no|
> |`fields`|Descriptive list of form fields. This is not currently used in the app, but can be a helpful annotation.|no|
> |`validations`|A set of validations to perform on incoming reports. More information about validation rules can be found [here](app-settings-validations.md).|no|
> |`validations.join_responses`|A boolean specifying whether validation messages should be combined into one message.|no|
> |`validations.list[]`|An array of validation rules a report should pass to be considered valid.|no|
> |`validations.list[].property`|Report field for which this validation rule will be applied.|no|
> |`validations.list[].rule`|Validation condition to be applied to the property field. More information about rules can be found [here](app-settings-validations.md#rules).|no|
> |`validations.list[].translation_key`|Translation key for the message reply to be sent if a report fails this rule.|no|
> |`messages`|An array of automated responses to incoming reports.|no|
> |`messages[].translation_key`|Translation key for the message text associated with this event|no|
> |`messages[].event_type`|An event that will trigger sending of this message. Typical values are: `report_accepted` when the report has been successfully validated, `registration_not_found` when the patient ID supplied in the report doesn't match any patient ID issued by Medic. `on_mute` and `on_unmute` are used in the context of muting as described [here](transitions.md#muting)|no|
> |`messages[].recipient`|Who the message should be sent to. Use `reporting_unit` for the sender of the report, `clinic` for clinic contact, and `parent` for the parent contact.|no|
> 
> ### Building workflows via interoperability 
> 
> Workflows can incorporate other digital tools, such as a facility-based electronic medical record system for referral workflows. These can be configured using the [outbound push]() feature, and data can be received as reports using the [CHT API]()
> <!-- TODO: link to outbound push -->
> </details>

## Hierarchies

The Core Framework requires a hierarchy to organize the app. It is often based on the hierarchy of a health system. These levels might have different titles depending on a particular health system’s configuration. 

A user logging into their app will see a custom set of people, tasks, reports, and analytics based on the hierarchy level that they belong to. This allows appropriate data sharing based on the role of the user in the health system. 

Note that each place in a hierarchy must have a primary contact person assigned to it. Other program staff working at the same level can be registered but there is only ever one primary contact. Supporting more flexible hierarchies is on the Core Framework development roadmap.  

### Sample Hierarchy "A"

This is an example of a hierarchy that includes district, health center, and CHW areas as the three levels which serve as “places,” or units of organizing people. User roles can be assigned to log in at any of these levels. For example, it would be customary for a CHW to log in at the CHW Area level and view the people, i.e. patients, who belong there.

This is the typical setup for a project that prioritizes district-level overview and aggregation. In this hierarchy, patients are often created under the “CHW Area” level, and are not organized by household.

### Sample Hierarchy "B"

This is an example of a hierarchy that includes health center, CHW area, and families as the three levels which serve as “places” or units of organizing people. User roles can be assigned to log in at any of these levels. For example, it would be customary for a CHW to log in at the CHW Area level and view the families, and below that the people, i.e. patients or family members, who belong there.

This is the typical setup for a project that requires family-level views.

<!-- Visual page 23 -->

The app hierarchy can be modeled after the health system, health program or the community.  All people are associated with a place and these places can be associated to each other. For instance, a Family Member is part of a Household. A Household and CHWs are part of a CHW Area. A CHW Area and nurses are part of a Health Facility. Additional levels may be added as needed. The Admin level operates outside of the hierarchy and gives access to all levels and people.

<!-- Visual page 24 -->

<details>
  <summary><em>Details for app developers</em></summary>

> **Defining Hierarchy**
> 
> From 3.7.0 it is possible to configure what types of places and people are available by modifying the `contact_types` array in the app settings. Each type has the following properties.
>
> |Property|Description|Required|
> |-------|---------|----------|
> | `id` | String identifier for the type. At times this will be used to sort the contacts in the UI so it is recommended to using a number prefix with gaps between numbers, eg: `10-district`, `20-region`, etc. | Yes. |
> | `name_key` | The translation key used for the title for the contact profile. | No, defaults to 'contact.profile'. |
> | `group_key` | The translation key used for the title of a list of contacts of this type. | Yes. |
> | `create_key` | The translation key used on the button for creating new contacts of this type. | Yes. |
> | `edit_key` | The translation key used on the button for editing contacts of this type. | Yes. |
> | `primary_contact_key` | The translation key used to identify a person as the primary contact of contacts of this type. | No, defaults to 'Primary contact'. |
> | `parents` | An array of strings of IDs of parent types. If more than one then this type can appear in different places in the hierarchy. If more than one type lists the same type as a parent then a user will get a dropdown of places to create. If no parents then contacts of this type will be at the top of the hierarchy and cannot be added as a child of any contact. | No, defaults to no parents. |
> | `icon` | The string ID for the icon to show beside contacts of this type. | Yes. |
> | `create_form` | The string ID for the xform used to create contacts of this type. | Yes. |
> | `edit_form` | The string ID for the xform used to edit contacts of this type. | No, defaults to the create_form. |
> | `count_visits` | Whether or not to show a count of visits for contacts of this type. Requires UHC to be enabled. | No, defaults to `false`. |
> | `person` | Whether this is a person type or a place type. | No, defaults to `false`. |
>  
>
</details>

## User Roles

The app uses roles and permissions to determine who has access to what data. User roles are general categories we use to assign a collection of broad permissions to users. There are two classes of roles: online and offline. Generally speaking, CHWs are usually offline users, while managers and nurses are usually online users. SMS users do not use the app, and thus do not have a user role.

### Online Roles

Online roles are for users who need access to a lot of data and need to maintain the system or update system settings. An internet connection is required.

### Offline Roles

Offline roles are for users who need to be able to access data on the go in the field, don’t need to maintain the system, and don’t have a reliable internet connection. All the data they have access to will be synced to their device.

<details>
  <summary><em>Details for app developers</em></summary>

> **Defining User Roles**
> 
> Each user is assigned one of the defined roles. Roles can be defined using the App Management app, which is represented by the `roles` object of the `app-settings.json` file. Each role is defined by an identifier as the key, and an object with the following properties:
> 
> |Property|Description|Required|
> |-------|---------|----------|
> | `name` | The translation key for this role | Yes |
> | `offline` |  | No, default `true` |
> 
</details>

### Sample Hierarchy "B"

Some people in the app will also be app users. Differing levels of access and permissions are assigned to each persona. A user role is created to provide them with access to the information they need. Offline and online access, storage limitations, and data privacy are taken into account.

| Persona         | Hierarchy                                      | Device    | Permissions                                                                                                                                                                                                                                              |
| :-------------- | :--------------------------------------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Program Officer | Logs in as Admin                               | Computer  | Admin users, usually Program Officers, are online-only admin users not associated to a particular level. They have access to all people, places, and records in the app, but since they are online-only users, they cannot view any tasks or targets.    |
| CHW Supervisors | Logs in at Health Facility level               | Tablet    | User at this level have offline access to view CHWs, fill out reports about them, and view tasks and targets related to them. Due to storage limitations, they aren’t able to view households or submit reports and review tasks and targets about them. |
| CHWs            | Logs in at CHW Area level                      | Phone     | Users at this level have offline access to view households and family members, submit reports about them, and view tasks and targets about them.                                                                                                         |
| Family members  | Registered at Household level, does not log in | Messaging | Family members might include fathers, mothers, children, and other adults. The program model determines which family members should be registered in the app. However, they are not users of the app, and do not log in themselves.                      |

## User Permissions

Roles are broad general collections of permissions. Permissions are fine grained settings that individually toggle on or off to allow a role to do a certain action or see a certain thing.

Viewing permissions determine which page tabs a user sees in the app and which types of data they do and don’t have access to. User action permissions include who can create (e.g., create new users), who can delete (e.g., delete reports), who can edit (e.g., edit profiles), and who can export (e.g., export server logs).

<details>
  <summary><em>Details for app developers</em></summary>

> **Defining User Permissions**
> 
> Each user role must be given explicit access for each of the following permissions in the app settings.
> 
> |Property|Description|Default|
> |-------|---------|----------|
> | `placeholder` |  |  |

</details>

## Hardware & Software Requirements

Hardware procurement, ownership, and management is the responsibility of each implementing organization. We strongly urge all organizations to procure hardware locally to ensure ease of replacement, repair, sustainability, and hardware support when needed.
