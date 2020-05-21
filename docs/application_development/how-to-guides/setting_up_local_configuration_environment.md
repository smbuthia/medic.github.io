# How to set up a CHT local configuration environment

## Purpose of the guide

This guide will take you through setting up a local environment to configure and test CHT applications. This includes setting up the necessary tools to download and run the CHT public docker image as well as a command line interface tool to manage and configure CHT apps.

By the end of the guide you should be able to:

- View the login page to CHT webapp on localhost
- Upload default configurations to localhost

## Brief overview of key concepts

**CHT Core Framework** The Core Framework makes it faster to build full-featured, scalable digital health apps by providing a foundation developers can build on. These apps can support most languages, are offline-first, and work on basic phones (via SMS), smartphones, tablets, and computers. 

**Medic Project Configurer** aka ***medic-conf*** is command-line interface tool to manage and configure CHT apps.

**Docker** is a tool designed to make it easier to create, deploy, and run applications by using containers.  

**Containers** allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and deploy it as one package.

## Required resources

Before you begin, you need to have some useful software and tools that are required for things to work.

First off, install [nodejs](https://nodejs.org/en/) 8 or later and [npm](https://www.npmjs.com/get-npm).

You also require Docker for your operating system:

- [Docker for Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [Docker for MacOS](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
- [Docker for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

## Implementation steps

Now that you have the dependent tools and software install, you are ready to begin setting up your CHT local configuration environment.

### _1. Install medic-conf_

Using npm on your terminal or command line, install medic-conf globally using the following command;

```zsh
npm install -g medic-conf
```

You can confirm that the installation was successful by typing `medic-conf` in your terminal or command line.

![confirm medic-conf](images/local-config-environment/confirm-medic-conf.png "Confirm medic-conf")

### _2. Install the Core Framework_

Download the [docker-compose.yml](https://github.com/medic/cht-core/blob/master/docker-compose.yml) file to a folder of your choice. Be sure to place it in a folder that's easy to find on terminal or command line.

Open your terminal or command line and navigate to the folder where you have your [docker-compose.yml](https://github.com/medic/cht-core/blob/master/docker-compose.yml) file and run the command:

```zsh
docker-compose up
```

Once the command is done running, navigate to `https://localhost` on a Google Chrome browser and login with the default username `medic` and default password `password`.

![medic login](images/local-config-environment/medic-login.png "Medic login")

### _3. Create and upload a blank project_

With `medic-conf` you can create a blank project. This provides you a template from which you can begin working on your configurations. Just run:

```zsh
mkdir cht-app-tutorials
cd cht-app-tutorials
medic-conf initialise-project-layout
```

![initialise project layout](images/local-config-environment/init-project-layout.png "Initialise project layout")

Then deploy the blank project onto your local test environment with the command:

```zsh
medic-conf --url=https://medic:password@localhost --accept-self-signed-certs
```

`accept-self-signed-certs` tells medic-conf that it’s OK that the server’s certificate isn’t signed properly, which will be the case when using docker locally.

Once you’ve run the above command it should complete with the message: `INFO All actions completed.`.

![all actions completed](images/local-config-environment/all-actions-completed.png "All actions completed")

## Frequently Asked Questions

## Guide summary