# Documentation

Table of Content

- [Introduction](#intro)
- [Getting started](#getting_started)
- [Add Custom Modules](#add_custom_modules)
- [Skin](#)

## <a name="intro">Introduction </a>

Grow Framework V2.0 is a javascript backend framework built using [meteorjs](https://www.meteor.com/), [reactjs](https://reactjs.org/) and [adminLTE](https://adminlte.io/) technologies

It has been designed in a way that allows backend developers to benefit from built in features, and to add custom modules special for their projects

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

Other than the built in modules, Grow framework includes also other micro services available for custom modules like:

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
7. Use the credentials found at the end of the meteor-grow/<project-name>/customizations/settings.json file to login

### Clone existing Grow project

1. clone this repository using git clone https://bitbucket.org/navybits/start-new-project.git grow
2. cd grow
3. bash clone-existing-project -p <project-name> -c <customization-git-url>
4. cd meteor-grow/<project-name>
5. sh start.sh <port>
6. Navigate to localhost:<port>
7. Use the credentials found at the end of the meteor-grow/<project-name>/customizations/settings.json file to login

Added modules and customizations code is under meteor-grow/<project-name>/customizations.
The repository meteor-grow/<project-name>/core is the core framework (Grow Framework)

## <a name="add_custom_modules">Adding Custom Modules</a>

Extending the core features in Grow means adding new modules in the customization zone of your project (under `grow/meteor-grow/<project-name>/customizations`)

Let's say you want to add a new feature to your project, for example: Blog posts management. How to start?

Well, don't be afraid! We already prepared a generic sample module for you. Take a copy of the folder `grow/generic_package` and paste it in your customizations directory `grow/meteor-grow/<project-name>/customizations`. This generic package is a bare module for Grow, once you add it, few steps to go, and you're there!

Inside this generic package, you will be able to:

1. Create new MongoDB collection (let's say `blog`) including its schema
2. Add special menu items to the side bar menu
3. Add routing rules for your collection listing (list of blogs)
4. Add routing rules for the editing/update forms (create new blog, or edit an existing blog)
5. Create listing view for your collection (UI for list of blogs)
6. Create form view for you collection (UI for creation/editing)
7. Add filters, grouping, dashboards, reporting related to your collection
8. Add model methods (all logic related to blogs on the server)
9. Create all apis related to your collection (CRUD for blogs)

and much more

So, your `custom-packageName` will be a copy of `generic-package`.

First, you need to ensure you have the right structure for your code to behave as expected.
It's preferred to organize the package folders accordingly to their code's destination: common code (`both`), `server` code and `client` code.

![package structure](https://portal.navybits.org/web/image/1446/download.png?access_token=9dcac7f8-77dc-4224-83b7-eccc88726dd2)

For more precision, we used multiple files with different purposes in each folder.

<!-- TODO elaborate on files -->

![package files](<https://portal.navybits.org/web/image/1447/download%20(1).png?access_token=fd2776ad-89a5-45dd-a437-a6f84ae77c5e>)

Second, you can handle how you manage these files through `package.js`.

```javascript
api.addFiles("lib/stylesheets/style.css");
// Here is the entry point for client & server:
// Use api.mainModule only if one of the following is
// Exporting some functionality or components
// To the external modules
api.addFiles("lib/both/main.js", ["server", "client"]);
api.addFiles("lib/server/index.js", ["server"]);
api.mainModule("lib/client/main.js", "client");
```

Finally, you need to add a dependency to your custom package from `custom-packages`.

```javascript
Package.onUse(function(api) {
  let packages = [
    // ...
    "<your-custom-package-name>"
    // ...
  ];
  // grant this package access to other packages symbols
  api.use(packages);
  // grant the app using this package access to other packages symbols
  api.imply(packages);
});
```

<!-- Why adding another schema for my collection?
The adjusted schema is used in Query Box and Form generator .

Depending on a custom schema, gives me the ability to undirectly control dynamic component , which depends on the collection schema.

Using the main simpl-schema forces considering unwanted or secured fields to be accessed.

In addition, optimized schema lets me add extra informations i need. Especially, when having relational links to be put in a form generator.

Important:
It's of the same structure of a schema but it'll not be attached to the collection with attchSchema as the simpl-schema .


How to add the side schema  ?
In your custom package, go to  main.js file ('lib/both/main.js').

Use the method addSchema defined in "ui-config-collector" methods.

Specify the name of your collection and give it your needed schema object.

Just like simpl-schema, the keys of nested objects are the fields of the collection. Each can hold : type, allowedValues , & label fields, and they will be used when working with schemas.

As for foreign keys and fields coming from other collections , extra info are needed (isLink, linkName, linkedField,& linkedCollection).

import Collector from 'meteor/ui-config-collector'
const {methods:{addSchema}}=Collector

const myAdjustedSchema={
 fieldNameA1:{
  type: String,
  label:'My first field'
 },
 fieldNameA2:{
  type: Number,
  allowedValues:[1,2,3]
 },
 fieldNameAB:{
  type: String,
  label: 'Field related to collection B',
  isLink: true,
  linkedCollection: "collectionB",
  linkedField: "fieldNameB1", /* the field needed from the collection B*/
  linkName: "link-A-B"
 }
}

addSchema({name: "collectionA" , schema: myAdjustedSchema })

Make sure you add a dependancy on "ui-config-collector" (core package) to use this method

 -->

## Built-in listing component

### How to use built-in list view?

Use the component `ListDefaultView` provided by `ui-config-collector` core package.
Specify your collection name in the props `collectionName`. In some cases, when you need to distinguish 2 different faces of the same collection, you can use `list` property.
This component handles, internally, the subscriptions and publications needed to get the collection data.
But, it won't know what to display, unless you specify the columns you want to show about your records.
To pass to that step, we need to fulfill the [pagination step](#pagination_step).
`ListDefaultView` provides additional props as well.
**List view additional options**:

- `listTitle`: optional string to be displayed on top of the resultant list.
- `defaultFilters`: optional object.
- `sortByCriteria`: optional field name (defaults to `createdAt` field).
- `sortByDirection`: optional "asc" or "desc" (defaults to "desc").

### <a name="pagination_step">Lists pagination</a>

One of the main configurations to move on with offered core features is what we call `pagination info`.
This configuration mainly controls, as its name means, the info related to list pagination. It's an object that holds a **`limit`** (of type `number`: defaults to 25 records per page). It supports **`skip`** option as well (of type `number`: defaults to 0 skipped records from the queried data cursor).
But the most affective option is **`fields`**.
**`fields`** is an object used to filter the record's info. The object has the fields desired, as keys with value 1 . ([accordingly with grapher's query options](https://cult-of-coders.github.io/grapher/#Query-Options)).
If **fields** is left empty, all schema fields will be returned.

#### How to determine the columns content

The list component needs to have an array of objects. Each object represents a column.
You can manage the header of a column `label`, the info displayed in it through `fieldName`.
Even you can manage a special resultant view, by replacing `fieldName` with a `render` function. The render function should always expect the record argument as a first param. It optionally receives a second argument which you can build, by adding `getColumnInfo` method in `ListDefaultView`. `getColumnInfo` takes **{row, column, dataToBeRouted}** as params. **row** is the record. **column** is the object you've configured. **dataToBeRouted** are relevant to pagination purposes.What you decide to return from this function will be the second entry param of `render`.
The array you built will be passed to the ready method `addListColumns` (provided by `ui-config-collector` core package), along with `collectionName` in **name** key.

```javascript
import { Meteor } from 'meteor/meteor'
import Collector from 'meteor/ui-config-collector'
const { methods: { addListColumns } } = Collector
if(Meteor.isClient){
 import React from 'react'

 const myColumns=[
  {
    label:'Column #1', fieldName:'fieldName1'
  },
  {
    label:'Column #2', render: (record, info)=>(<span>....</span>)
  }
.....
 ]
}

addListColumns({name: 'collectionName', columns: myColumns })
```

#### Make columns sortable

Add to your column's definition and additional field `sortingPath` in which you will set the path of "to be sortable" info.

```
import React from 'react'
import Collector from 'meteor/ui-config-collector'
const {components: { ListDefaultView } } = Collector

class MyClass extends React.Component {
 render(){
   return (
     <div className="content">
           <ListDefaultView collectionName="myCollectionName"
                sortByCriteria="myFieldName"
                listTitle="My List Title"
                defaultFilters={{_id: {$eq: Meteor.userId()} }} /*direct form of a query*/
            />
     </div>)
 }
}
1. It will hold a label that appears on top of the column.

2. The fieldName of the value i want to show , or , a render function.

              (The render function accepts two parameters :

               First param: the whole record .

               Second param: the return value of the function getColumnInfo (specified as a ListDefautView prop).

In main.js , test Meteor.isClient before using addListColumns and returning any react element for any column.
As for info, they're coming from your custom getColumnInfo function code . It's passed as a ListDefaultView prop in your custom component.

It receives 2 params: row & column.

The row represents one record . The column is, at eachtime, one of the columns defined in main.js.

                <ListDefaultView collectionName="myCollectionName"
                  sortByCriteria="myFieldName"
                  listTitle="My List Title"

                  getColumnInfo={ ({row,column})=> {....} }
                 />
 ```

... to be continued

## Linking collections

### How to build links between collections ?

To define links, use `addLinks` method with your collection (As explained in [grapher documentation](https://cult-of-coders.github.io/grapher/#Linking-Collections)).

### How to save linked data in your record ?

Use the method named `addGrapherLinks` provided by the core package `ui-config-collector` and give it the same link structure defined for grapher function, along with your collection name. This will created something similar to layer, upon building records, that will add to each record additional fields representing the links you already defined. The field has the name of its corresponding link and holds the data retreived from target collection.

<!-- __________________________________ -->

<!-- `imports/custom/startup/client/config.js` holds

```javascript
document.title = "Your title here";
```

In `imports/custom/startup/client/skin.less` define your styling rules then wrap with one class.

```less
@import "../../../../client/lib/base/lib/bootstrap/mixins.import.less";
@import "../../../../client/lib/base/lib/bootstrap/variables.import.less";
@import "../../../../client/lib/base/lib/admin-lte/variables.less";
@import "../../../../client/lib/base/lib/admin-lte/mixins.less";
@secondary: #053255;
@main : #bd4334;
@sidebar-dark-bg : #053255;
.skin-custom {
  //Navbar
  .main-header {
    .navbar {
      .navbar-variant(@main; #fff);
      .sidebar-toggle {
        color: #fff;
        &:hover {
          background-color: darken(@secondary, 5%);
        }
      }
      @media (max-width: @screen-header-collapse) {
        .dropdown-menu {
          li {
            &.divider {
              background-color: rgba(255, 255, 255, 0.1);
            }
            a {
              color: #fff;
              &:hover {
                background: darken(@main, 5%);
              }
            }
          }
        }
      }
    } //Logo
    .logo {
      .logo-variant(darken(@main, 5%));
    }
    li.user-header {
      background-color: @main;
    }
  } //Content Header
  .content-header {
    background: transparent;
  } //Create the sidebar skin
  .skin-dark-sidebar(@secondary);
}
```

In `config.js`, put the wrapper styling class name in the constant **CustomSkin**, then add it to the module exports.

```javascript
const CustomSkin = "my-custom-skin";
module.exports = { CustomSkin };
```
To customize the logo, use an additional constant **CustomBranding**. 
```javascript
const CustomSkin = "my-custom-skin";
const CustomBranding=<a href="#" className="logo">
    <span className="logo-mini"><img src="/img/custom-img/logo.png" height="30px" /></span>
    <span className="logo-lg"><img src="/img/custom-img/logo.png" style={{ height: "40px", paddingBottom: "3px" }} /></span>
</a> -->

module.exports = { CustomSkin, CustomBranding};
```
<!-- How to change DOM title?

How to customize layout skin? -->

<!-- TODO mainCss file -->
<!-- 
How to customize brand logo (in header)?
Declare a const named CustomBranding and give the element you want to display. Use the className logo to give the right styling . Hence, this frameWork supports displaying a mini and a large logo according to the extension of the sidebar menu.

Then use module.exports to export it .

import React from 'react'

const CustomBranding=<a href="#" className="logo">
<span className="logo-mini">...</span>
<span className="logo-lg">....</span>
</a>

module.exports={CustomBranding}

How to customize the footer?
Define the constant CustomFooter .

Use a tag named footer with main-footer className, and give it the content you want to see as children (You can use responsive react code).

Export your const.

import React from 'react'

const CustomFooter=<footer className="main-footer">...</footer>

module.exports={CustomFooter}

How to customize the navigation menu title?
In config.js, use menuHeaderText const to display your custom text .

Use module.exports to make it reachable .

const menuHeaderText="My custom menu title"

module.exports={menuHeaderText} -->
