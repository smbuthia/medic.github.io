---
layout: default
title: Introduction
parent: Building Core Framework Applications from Scratch
nav_order: 0
---

# Part 1: getting our feet wet

Welcome! With the release of the [Community Health Toolkit (CHT)](https://communityhealthtoolkit.org/) reference app, we thought it was appropriate to write a collection of tutorials explaining what goes into a CHT configuration, and guide you step by step on how to build your own. Later, we’ll talk about the more complex situations. These tutorials are aimed at software developers who are looking to build applications inside the CHT. As such, there will be programming! It’s all very simple though, and so not to worry.

If you want to look at one we’ve prepared earlier, you can also take a look at our fully formed reference application (brief instructions are at the bottom of the page). It may be helpful to periodically refer to this as the tutorial continues if you’d like to self-learn and dig into more complicated scenarios.

## Are these tutorials for me?

Firstly, you should be interested in building or maintaining an application built using the CHT Core Framework. For more information about the CHT, [take a look at our CHT overview](/). If you’re unsure if you’re interested in building an application with the CHT, this is where you should start.

Secondly, you’ll also need _some_ software development skills. Applications are built using a combination of JavaScript, JSON and XML, and configurations are deployed using a terminal. If you’re comfortable writing simple scripts in JavaScript you will do fine here.

Ready?

## Brief architecture, and some terms

We’re not going to delve into too much detail here---that’s more for [Core Framework development](https://github.com/medic/cht-core). However, it’s probably worth going through and clarifying a few terms and technologies.

The Core Framework is an offline-first web application that works on mobile phones as well as larger devices like tablets and laptops. On Android phones it can also be loaded directly as an Android app. It uses [CouchDB](https://couchdb.apache.org/) and [PouchDB](https://pouchdb.com/) to store data on the user’s device, and synchronize that data to the server when internet is available.

Its primary use case is as a health-care tool for Community Health Workers (CHWs): CHWs create families and patients and create reports about them, directed by a tasks system and incentivised by a targets system, with the ultimate goal being improving a CHW’s ability to provide healthcare in under-served, disconnected communities.

The Core Framework is highly configurable: by default (since 3.7.0) we ship with the _reference application configuration_, a fully featured implementation of an ANC workflow.

Configurations typically contain:
 - XForms, using an extension of the [ODK XForms spec](https://opendatakit.github.io/xforms-spec/), written using [XLSForms](http://xlsform.org/) (spreadsheet-based form definitions). There are a few different types of forms:
     - Forms for creating Contacts, such as Places (physical places like Facilities, or conceptual places like Families or Areas) or People (patients, but also CHWs, Nurses and other humans)
     - Forms for creating Reports, which are the primary form of data entry available to users
 - Rules that create Tasks for users to complete (e.g., if a CHW create a report stating a patient is ill, a task may be created to check up on that patient the next day)
 - Rules that track Targets for users to aspire to (e.g. to follow up with sick patients in under 24 hours 90% of the time)
 - Customisable information panels that will appear on a Contact’s (e.g. patient) profile page. These can show any information contained in reports for that Contact, e.g., how many times they have been pregnant, what immunisations they’ve had and so on
 - Translations, white labelling, and much much more

There are also some other uses (stock monitoring, or as a hub for primarily SMS-based workflows, which in turn have their own complex configurations), and there is nothing _inherently_ health-care related in its technology and feature-set, but the description above is what we’re going to focus on.

## Getting our feet wet

In this tutorial we’re going to set up a blank CHT configuration. By the end you will have an environment capable of building and deploying CHT configurations, with the Core Framework running on your local machine. We’ll be using this as a basis for later tutorials.

First, we’re going to install [medic-conf](https://github.com/medic/medic-conf), the tool used to compile and deploy CHT applications. Then, we’ll create a default project and walk through what each part is for. Finally, we’ll install the CHT core and deploy our (still very blank) configuration to it.

**Note**: this tutorial was written by someone using Linux. There may be differences for MacOS and Windows users. We’ll try to keep this guide up to date for all operating systems!

## Installing medic-conf

To install `medic-conf`, you first need to install [NodeJS](https://nodejs.org/) and [Npm](https://www.npmjs.com/). How to do this will vary depending on your operating system. Npm has a [great guide for different operating systems](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm).

Once you’ve done this, in your terminal install medic-conf:

```sh
npm install -g medic-conf
```

You can confirm the installation went successfully by typing `medic-conf` at your terminal:

![terminal showing medic-conf output](./res/001-medic-conf-output.png)

## Creating a blank project

Medic-conf can create a blank template project, so let’s do that!

```sh
mkdir cht-app-tutorials
cd cht-app-tutorials
medic-conf initialise-project-layout
```

This will create a new, blank project. Your directory structure should look this:

```
 ➜  cht-app-tutorials tree
.
├── app_settings.json
├── contact-summary.js
├── forms
│   ├── app
│   ├── collect
│   └── contact
├── resources
├── resources.json
├── targets.js
├── tasks.js
└── translations
```

Let’s talk briefly about what each of these is for:
- `app_settings.json` is a JSON file containing lots of different small pieces of configuration: everything from supported locales to roles and permissions.
- `contact-summary.js` is a JavaScript file which allows you to extend what is displayed on the pages of contacts (patients, CHWs etc) to show custom panels of information, such as how many pregnancies they’ve had, immunisation status etc
- `forms/apps` is where forms that generate “reports” (new pregnancy, visits etc) are stored
- `forms/collect` is where forms that are specific to an application called Medic Collect are stored, don’t worry about this one!
- `forms/contact` contains forms that are used to create contacts: CHWs, facilities, patients etc
- `resources` and `resources.json` allows you to specify custom icons: the directory is where they are stored, and the JSON file is where you define a mapping between the name of the resource (such as `icon-person`) and it’s filename in the `resources` directory
- `targets.json` contains configuration for targets
- `tasks.json` contains our declarative tasks config
- `translations` is the directory where you’d put custom translations, to either make changes to a language already supported in the CHT core, or to upload your own

We’ll go over these in far more detail in later tutorials. For now, let’s go through deploying your project, first by getting the Core Framework up and running.

## Installing the Core Framework

There are a few different ways of getting the Core Framework running.

If you are just interested in Core Framework configurations, follow the [easy installation guide](https://github.com/medic/medic/blob/master/INSTALL.md).

If you’re _also_ interested in [contributing to the Core Framework](https://github.com/medic/medic/blob/master/CONTRIBUTING.md) and are comfortable with a more complicated setup feel free to follow the [developer setup guide](https://github.com/medic/medic/blob/master/DEVELOPMENT.md).

If you followed the easy installation guide the framework should be available at https://localhost.

If you followed the developer setup guide the framework should be available at http://localhost:5988.

![login page](./res/001-login.png)

To login to the Core Framework, you'll need a username and password. You would have come up with one by following either guide (traditionally: easy installation would give you `medic` and `password`, while the developer guide would give you `admin` and `pass`).

![admin page](./res/001-admin.png)

As you’re logging in as an administrative user you’re logged into the application settings page. In later tutorials we’ll go into more detail, but just understand that this is where some settings can be viewed / changed, and to access the main application click ‘Application’ in the top right.

## Deploying the blank project

Now that the framework is working we can use `medic-conf` to deploy your blank project. In your configuration folder enter the following:

```sh
medic-conf --url=https://medic:password@localhost --accept-self-signed-certs
# or, if you setup the Core Framework development environment
medic-conf --local
```

Here we’re passing two parameters
- `--url` is the fully authenticated (note the username and password) location of the framework server. If you are running the framework via the developer instructions you can instead pass `--local` which uses credentials from your environment variables
- `accept-self-signed-certs` tells medic-conf that it’s OK that the server’s certificate isn’t signed properly, which will be the case when using docker locally

We’re then not passing anything else. This is where you _could_ pass in a collection of actions for `medic-conf` to perform. As we’re not passing in any, this tells `medic-conf` to perform the default collection of actions, which will compile and upload everything in your project each time.

Once you’ve run the above command it should complete quite quickly, with the message: `INFO All actions completed.`.

How can you tell from inside your http://localhost application? Well, since we don’t have any configuration yet, there is nothing to see!

## Where to from here?

If you want to get a bit of a head start and poke around with what’s possible, you can use the same `medic-conf` command you used to deploy your own config to deploy the reference config:
- First, check out https://github.com/medic/cht-core using git.
- Navigate to `./config/default`
- Run your `medic-conf` command as you did above

This will do a **lot** more, and your framework will now contain configuration, forms, tasks and more.

To get back to your own empty configuration, just run `medic-conf` in your new project directory again.

That’s all we’re going to cover this time. I know that feels like a lot of effort to make nothing much, but it’s laid a good foundation to start on. Stay tuned, as next time we’re going to get stuck into building some actual configuration.

We haven’t done too much yet, but I’ve also uploaded our current progress to [a git repository](https://github.com/medic/cht-app-from-scratch/tree/001), and will continue this for each subsequent tutorial. If you’re having trouble at any point you can refer to this repository and see where you should be ending up.

To ask questions, get clarifications or share your experiences, the [community health forums](https://communityhealthtoolkit.org/) are open for discussions.

Further reading:
- [Part One's application Github Repository](https://github.com/medic/cht-app-from-scratch/tree/001)
- [Community Health Toolkit overview](https://communityhealthtoolkit.org/)
- [Core Framework's Github page](https://github.com/medic/cht-core)
- [medic-conf's Github page](https://github.com/medic/medic-conf)
