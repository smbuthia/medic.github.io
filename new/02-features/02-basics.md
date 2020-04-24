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

A form can also trigger an SMS to be sent to another user, such as CHW supervisor or nurse, where immediate notification is desired. 

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

## User Roles

The app uses roles and permissions to determine who has access to what data. User roles are general categories we use to assign a collection of broad permissions to users. There are two classes of roles: online and offline. Generally speaking, CHWs are usually offline users, while managers and nurses are usually online users. SMS users do not use the app, and thus do not have a user role.

### Online Roles

Online roles are for users who need access to a lot of data and need to maintain the system or update system settings. An internet connection is required.

### Offline Roles

Offline roles are for users who need to be able to access data on the go in the field, don’t need to maintain the system, and don’t have a reliable internet connection. All the data they have access to will be synced to their device.

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

## Hardware & Software Requirements

Hardware procurement, ownership, and management is the responsibility of each implementing organization. We strongly urge all organizations to procure hardware locally to ensure ease of replacement, repair, sustainability, and hardware support when needed.
