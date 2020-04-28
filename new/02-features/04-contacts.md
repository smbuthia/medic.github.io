# Contacts: Person and Family Profiles
<!-- TODO Refine screenshots, and add desktop view. -->

## Overview

“People” is the generic name we use for individuals in the app. They can be patients, family members, nurses or health workers. Anyone with a profile in the app is a person.

“Places” is the generic name that represents a level in the hierarchy. “People” belong to “places” and “places” belong to other higher level “places” in the hierarchy.

Depending on the context, a “place” might be a health facility and the “people” who get created at that level might be nurses. Most often for CHWs, these “places” are families. 

Users can access their “people” and “places” from the People tab. 

## Main List

<img src="images/contacts-main-list.png" width="23%" align="right" />

The view on the right is what a logged-in CHW would see in when they access the “People” tab on a small screen. 

The item at the top of the list is the “place” the user belongs to. Below that, we see a list of the “places” they serve, represented by families. Individual “people” are not shown here, but they will appear in search results. 

Because this list defaults to show the “places” below the user in the hierarchy, a CHW supervisor would see a different view. Instead of families, they might see a list of CHW Areas they manage. 

New “places” can be added to this level of the hierarchy by clicking on the “Add new +” button at the bottom of the screen. This allows a CHW to add a new family to their list, or a CHW supervisor to add a new Area they manage. 


## Searching

Search for a “person” or “place” by clicking in the search area at the top of the screen. The freetext search works on all fields included in the “person” or “place” document such as patient name or patient ID. The exact fields depends on which information you’ve configured your app to collect.

After entering a search term, the list filters to show matching items. Searching will only return items that are lower than you in the hierarchy and that you have permission to view. 

To clear the search and return the default view, click on the refresh icon located to the right of the search box.

## Profiles

Clicking an item on the main list will open a profile where you can see detailed information about that person or place. At the top is general information like name and phone number.

<p float="left">
  <img src="images/contacts-profile-1.png" width="23%" />
  <img src="images/contacts-profile-2.png" width="23%" />
  <img src="images/contacts-profile-3.png" width="23%" />
  <img src="images/contacts-profile-4.png" width="23%" />
</p>

If you’re viewing a place profile, you’ll see a list of people or places that belong to this place in the app hierarchy, such as family members. The star signifies the primary contact.

Beneath that, you will find tasks for this person or place. At the very bottom is a history of submitted reports for this person or place.

From profiles, users can edit contact information, take actions, and, if viewing a place profile, add new people and assign a primary contact person. If a place is not at the bottom of the hierarchy, a user can add new places to the level below this.

<details>
  <summary><em>Details for app developers</em></summary>

> **Configuring Contact Profiles**
> 
> #### Configuring Contact Profiles
> 
> A contact's profile page is defined by the [Fields](), [Cards](), and [Context]().
> 
> To update the Contact profiles for an app, changes must be compiled into `app-settings`, then uploaded.
> 
> Eg `medic-conf --local compile-app-settings backup-app-settings upload-app-settings`
</details>


### Fields
The top card on all profiles contains general information for the contact. All the fields shown in this summary card are configurable.

<details>
  <summary><em>Details for app developers</em></summary>

> **Configuring Contact Summary**
> 
> Each field that can be shown on a contact's profile is defined as an object in the `fields` array of `contact-summary.templated.js`. The properties for each object determine how and when the field is shown.
> 
> <!-- If you change this table, update the duplicate descriptions in ### Cards -->
> | property | type | description | required |
> |---|---|---|---|
> | `label` | `string` | A translation key which is shown with the field. | yes |
> | `icon` | `string` | The name of the icon to display beside this field, as defined through the Configuration > Icons page. | no |
> | `value` | `string` | The value shown for the field. | yes |
> | `filter` | `string` | The display filter to apply to the value, eg: `{ value: '2005-10-09', filter: 'age' }` will render as "11 years". Common filters are: `age`, `phone`, `weeksPregnant`, `relativeDate`, `relativeDay`, `fullDate`, `simpleDate`, `simpleDateTime`, `lineage`, `resourceIcon`, `translate`. For the complete list of filters, and more details on what each does, check out the code in [`medic/webapp/src/js/filters` dir](https://github.com/medic/medic/tree/master/webapp/src/js/filters). | no |
> | `width` | `integer` | The horizontal space for the field. Common values are 12 for full width, 6 for half width, or 3 for quarter width. Default 12. | no |
> | `translate` | `boolean` | Whether or not to translate the `value`. Defaults to false. | no |
> | `context` | `object` | When `translate: true` and `value` uses [translation variables](https://angular-translate.github.io/docs/#/guide/06_variable-replacement), this value should provide the translation variables. | no |
> | `appliesIf` | `function()` or `boolean` | Return true if the field should be shown. | no |
> | `appliesToType` | `string[]` | Filters the contacts for which `appliesIf` will be evaluated. For example, `['person']` or `['clinic', 'health_center']`. | no |
> 
> See [How to configure profile pages]() for an example. 
</details>

### Condition Cards

A “condition” card displays data on a profile that’s been submitted in a report about that person or place. Data can be pulled from one report or summarize many reports.

<p float="left">
  <img src="images/contacts-condition-card-1.png" width="23%" />
  <img src="images/contacts-condition-card-2.png" width="23%" />
</p>

Condition cards can be permanent or conditional; set to appear only when a specific type of report is submitted. They can also be set to disappear when a condition is resolved or a certain amount of time has passed. You can have as many condition cards as you like, though we recommend keeping the user’s experience in mind.

Configurable elements include: 
- Title 
- Label for each data point displayed
- Data point for the field 
- Icon for the field, if desired
- Conditions under which to display

<details>
  <summary><em>Details for app developers</em></summary>

> **Configuring Condition Cards**
> 
> Each condition card is defined as a card object in the `cards` array of `contact-summary.templated.js`. The properties for each object determine how and when the card and its fields are shown.
> 
> <!-- If you change the field data in this table, update the duplicate descriptions in ### Fields -->
> | property | type | description | required |
> |---|---|---|--|
> | `label` | `translation key` | Label on top of card. | yes |
> | `appliesToType` | `string[]` | A filter, so `appliesIf` is called only if the contact's type matches one or more of the elements. For example, `['person']`. Please, note that `['report']` is also allowed to create a report card. But, you cannot use it in conjunction with a contact's type. | no |
> | `appliesIf` | `function()` or `boolean` | Return true if the field should be shown. | no |
> | `modifyContext` | `function(context)` | Used to modify or add data which is passed as input to forms filled from the contact page. | no |
> | `fields` | `Array[]` of [fields](#fields) | The content of the card. | yes |
> | `fields[n].appliesIf` | `boolean` or `function(report)` | Same as Fields.appliesIf above. | |
> | `fields[n].label` | `string` or `function(report)` | Label shown with the field. | yes |
> | `fields[n].icon` | `string` or `function(report)` | The name of the icon to display beside this field, as defined through the Configuration > Icons page. | no |
> | `fields[n].value` | `string` or `function(report)` | The value shown for the field. | yes |
> | `fields[n].filter` | `string` or `function(report)` | The display filter to apply to the value, eg: `{ value: '2005-10-09', filter: 'age' }` will render as "11 years". Common filters are: `age`, `phone`, `weeksPregnant`, `relativeDate`, `relativeDay`, `fullDate`, `simpleDate`, `simpleDateTime`, `lineage`, `resourceIcon`, `translate`. For the complete list of filters, and more details on what each does, check out the code in [`medic/webapp/src/js/filters` dir](https://github.com/medic/medic/tree/master/webapp/src/js/filters). | no |
> | `fields[n].width` | `integer` or `function(report)` | The horizontal space for the field. Common values are 12 for full width, 6 for half width, or 3 for quarter width. Default 12. | no |
> | `fields[n].translate` | `boolean` or `function(report)` | Whether or not to translate the `value`. Defaults to false. | no |
> | `fields[n].context` | `object` | When `translate: true` and `value` uses [translation variables](https://angular-translate.github.io/docs/#/guide/06_variable-replacement), this value should provide the translation variables. Only supports properties `count` and `total` on cards. | no |
> 
> See [How to configure profile pages]() for an example. 
</details>

### Care Guides <!-- todo: Resolve Care Guides vs Actions -->

<img src="images/contacts-care-guides.png" width="23%" align="right" />

“Care Guides” are dynamic forms that you can fill out for a person or place. You can access Care Guides by clicking on the + button at the bottom of a profile. For more info, see the "[Decision Support for Care Guides]()" section of this overview. 

You’ll see different forms here depending on which person or place you’re viewing. For example, forms for families might include a “Family Survey.” Forms for adult women might include “New Pregnancy.” Forms for adult women who have had a pregnancy report, and no delivery yet reported, would also see “ANC visit.” Forms for children might include “Under-5 Assessment” or “Growth Monitoring.”

Health workers can use these Care Guides at any time. If the app has scheduled a care visit or follow up, it will be listed under “Tasks.” 

<details>
  <summary><em>Details for app developers</em></summary>

> **Configuring Care Guides**
> 
> Each care guide accessible from a contact profile is defined as an [App Form](). Context information can be provided to forms via the `context` object of `contact-summary.templated.js`.
> 
> To show an App Form on a contact's profile, the form's `expression` field in its properties file must evaluate to true for that contact. The context infomation from the profile is accessible as the variable `summary`.
> 
> The context data is also available directly within the app forms' XForm calculations, as `instance('contact-summary')/context/${variable}`. For instance if `context.is_pregnant` is used in the contact summary, it can be accessed in an XForm field's calculation as `instance('contact-summary')/context/is_pregnant`. Note that these fields are not available when editing a previously completed form, or when accessing the form from outside of the profile page.
> 
> See [How to configure profile pages]() and [How to build app forms]() for examples and more information. 
</details>