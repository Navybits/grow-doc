# Documentation
Table of Content
- [Introduction](#intro)
- [Getting started](#getting_started)
- [Add Custom Modules](#add_custom_modules)
## <a name="intro">Introduction </a>
Grow Framework V2.0 is a javascript backend framework built using [meteorjs](https://www.meteor.com/), [reactjs](https://reactjs.org/) and [adminLTE](https://adminlte.io/) technologies


It has been designed in a way that allows backend developers get benefit of built in features, and to be able to add custom modules special for their projects

When using Grow, developers will be able to develop backend faster, provide API very easily, and build a well presentable dashboard for the control panel

Grow Framework includes the following macro built in features:
1. Users management module
2. CMS module
3. Configuration module
4. Dashboard module
5. DMS module
6. Notifications module
7. Api layer module
8. Acl module

Each of the above modules represents a meteor package (special module based structure for meteorjs).
To create your own modules you can follow the tutorial [Writing Atmosphere Packages](https://guide.meteor.com/writing-atmosphere-packages.html)

Other than the built in modules, Grow framework includes also other micro services available to be used in custom modules like:

- Scheduled jobs
- File uploader
- Listing
- Calendar
- Maps
- Pagination
- Filtering
- Grouping
- Reporting
- Notifications: SMS, Push, and email
- Reusable form ui for data management

and much more

Grow is a MERN stack framework, that's why you will have to handle working with MongoDB, ReactJs for UI and NodeJs as a server application engine.

---

## <a name="getting_started">Getting Started </a>
In order to start using Grow, you have to clone or download the project builder from [here](https://bitbucket.org/navybits/start-new-project.git) as indicated in the following section step 1

### Create new Grow project

1. clone this repository using git clone https://bitbucket.org/navybits/start-new-project.git grow 
2. cd grow
3. sh create-project.sh <project-name>
4. cd meteor-grow/<project-name>
5. sh start.sh <port>
6. Navigate to localhost:<port>
7. Use the credentials found at the end of the meteor-grow/<project-name>/customizations/settings.json file


### Clone existing Grow project
1. clone this repository using git clone https://bitbucket.org/navybits/start-new-project.git grow 
2. cd grow
3.  bash clone-existing-project -p <project-name> -c <customization-git-url>
4. cd meteor-grow/<project-name>
5. sh start.sh <port>
6. Navigate to localhost:<port>
7. Use the credentials found at the end of the meteor-grow/<project-name>/customizations/settings.json file

In order to add modules and make customizations, your code is under meteor-grow/<project-name>/customizations the repository meteor-grow/<project-name>/core is the core framework (Grow Framework)

## <a name="add_custom_modules">Adding Custom Modules</a>

Extending the core features in Grow means adding new modules in the customization zone of your project (under `grow/meteor-grow/<project-name>/customizations`)

Let's say you want to add new feature to your project, for example: Blog posts management. First question you could ask, how to start?!

Well, don't be afraid! We already prepared a generic sample module for you. Take a copy of the folder `grow/generic_package` and paste it in your customizations directory `grow/meteor-grow/<project-name>/customizations`. This generic package is a bare module for Grow, once you add it, few steps to do, and you will be in!

Inside this generic package, you will be able to:
1. Create new MongoDB collection (let's say `blog`) including its schema
2. Add special menu items in the side bar menu 
3. Add routing rules for your collection listing (list of blogs)
4. Add routing rules for the editing/update forms (create new blog, or edit an existing blog)
5. Create listing view for your collection (UI for list of blogs)
6. Create form view for you collection (UI for creation/editing)
7. Add filters, grouping, dashboards, reporting related to your collection
8. Add model methods (all logic related to blogs on the server)
9. Create all apis related to your collection (CRUD for blogs)

and much more 


... to be continued