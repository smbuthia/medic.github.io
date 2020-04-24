# Introduction

*We’re building open source technology for a new model of healthcare that reaches everyone. We envision a world where primary health care is equitable, accessible, and delivered by people who are trusted in their communities.*

The Community Health Toolkit (CHT) is a project by a group of leading organizations who have come together to support the development of digital health initiatives in the hardest-to-reach areas. 

The CHT provides you with resources to design, build, deploy, and monitor digital tools for community health. It includes open source software frameworks and applications, guides to help design and use them, and a community forum for collaboration and support.  Together, we envision a world where healthcare is of the highest attainable quality, equitable, accessible, and delivered by the people who are trusted most in their communities.  

With more than 24,000 health workers using these tools to support a million home visits every month, the CHT is the most full-featured, mature, and widely-used open source software toolkit designed specifically for advanced community health systems. 

## CHT Core Framework

The Core Framework makes it faster to build full-featured, scalable digital health apps by providing a foundation developers can build on. These apps can support most languages, are offline-first, and work on basic phones (via SMS), smartphones, tablets, and computers. 

App developers are able to define health system roles, permissions and reporting hierarchies, and make use of five highly configurable areas of functionality: messaging, task and schedule management, decision support workflows, longitudinal person profiles, and analytics. 

The Core Framework can be used to support the unique needs of a given health system and the work of community health workers, frontline supervisors, facility-based nurses, health system managers, and even patients and caregivers.

## App Development

From a technical perspective, developing a custom app begins with writing XForms, JSON, and JavaScript code that configures the Core Framework’s features to meet your organization’s needs. 

The Core Framework allows you to define each element in your app in a modular way, and then specify when and how it should appear for different types of users, without having to modify the underlying Framework. Collectively, this customization is referred to as Configuration Code.

Developing an app using the Core Framework requires an understanding of:
- Javascript code and expressions
- JSON format used to specify configuration
- XLSForms to setup actions and contacts

For more info about app development, visit [our website](). 

### Reference Apps

The Community Health Toolkit’s Reference Apps provide organizations with a template for structuring and organizing a community health workflow, its configuration code, and testing framework. They include a foundation for forms, data fields, and even analytics, and can be deployed as-is or easily customized by a developer for your unique context.

This slide deck won’t describe any one Reference App in detail. Instead, we’ll use screenshots from the Medic Android Demo App and other Reference Apps to give you a general idea of how the Core Framework’s features can be deployed for different types of users in a wide range of health programs.

## Responsive Web App Model

A responsive web app is a hybrid of a website and a mobile application. On desktop and laptop computers, it runs in the web browser. On Android devices (such as cell phones or tablets), it is downloaded as an app. The same source code powers the experience, meaning that the app you see on your desktop is the same app you see on your mobile device. 

Web apps built with the Core Framework are fully responsive, which means content will scale to fill the available space. Users accessing the app on a mobile device will see a single-panel mobile layout. Users accessing the app on a desktop or laptop device will see a two-panel layout.

## Offline-First Technology

Our technologies need to support health systems in a wide range of low infrastructure environments. Apps built with the Core Framework are designed to be offline-first and work with only an occasional internet connection.

The app stores a user’s data locally on their device so that workflows can be completed without syncing to the server. When a connection becomes available, data will automatically sync to and from the server. Offline-first technology enables health workers to carry out important duties even when opportunities to sync may be weeks apart.

As with any app, there is a limit to how much data can be stored locally, particularly on a mobile device. For users needing access to large amounts of data, online user roles are available. 

## Made for Localization

Apps can be customized for different deployments and types of workflows. We’re already using the framework in many countries around the world with localization settings.

Users can currently interact with the app in English, French, Hindi, Nepali, Spanish, Swahili, or Indonesian and new languages can be added in the admin console. The app also supports Bikram Sambat or Gregorian calendars and localized date formatting.



## User Personas

We’ve used the Core Framework to build apps for a variety of users, including community health workers (CHWs), CHW supervisors, nurses, system administrators, and other people who deliver care and support.

User personas give us a common understanding of who we are serving, particularly when working across diverse contexts. Our global personas are based on “typical” users, knowing that some variation is present in different settings.

Being explicit about who are we designing with and for, and understanding what’s important to them helps us prioritize features, make better design decisions, and optimize impact.

### Community Health Worker (CHW)

CHWs are the central users of apps built with the Core Framework. They conduct household visits and are responsible for the health of their community. CHWs typically live in and are chosen by their community. Their degree of health training, responsibilities, and support depends upon their country and program. Many are unpaid volunteers, though a growing number receive wages or incentives. Most operate as a CHW on a part-time basis while engaging in farming or other income-generating activities. Most CHWs are responsible for:
- Registering new people and families
- Conducting guided health assessments
- Screening for and tracking specific conditions
- Providing basic medicines and health supplies
- Reporting danger signs & referring to the clinic
- Following up about clinic visits and care

### CHW supervisor

The CHW supervisor is the person who trains and supports CHWs and helps them meet their monthly goals. Supervisors usually split their time between administrative duties at the local health facility and accompanying CHWs on their community visits. Most CHW supervisors are responsible for:
- Training CHWs and reinforcing health knowledge and protocols
- Following up with CHWs on high-priority cases 
- Liaising with the facility-based staff on the needs at the community level
- Mobilizing CHWs to educate community on health promotion campaigns
- Tracking progress towards key impact metrics and helping CHWs reach their targets
- Aggregating CHW data and reporting on activities to health system officials

### Nurse

Nurses are stationed at the health facility and spend their days seeing patients. They are very busy and may see 50 or more patients a day. At the clinic, they sometimes deal with staff shortages, stock-outs, and poor internet connectivity. They help train and manage CHWs, particularly during monthly meetings at the facility. They are interested in seeing improvements in health metrics for the areas their facility serves.  Most nurses are responsible for:
- Assessing patients and providing primary care 
- Reporting service delivery statistics to health system officials
- Coordinating care for high-priority patients through CHWs and supervisors
- Initiating events to promote healthy practices in the community

### Data Manager

Data Managers are often based at a regional health facility and serve many local facilities. They are responsible for collating and reporting on community and health system data. Their work often involves following up with supervisors and nurses to verify data and retrieve missing information. Most data managers are responsible for:
- Collecting health system data from the field
- Verifying data for accuracy and completeness
- Aggregating data and providing reports to nurses, supervisors, and health system officials with raw numbers and trends on key metrics
