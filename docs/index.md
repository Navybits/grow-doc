# Documentation

Table of Content

- [Introduction](#intro)
- [Getting started](#getting_started)
- [Layout customization](#layout_customization)
  - [Title](#title)
  - [Logo](#logo)
  - [Styles](#styles)
  - [Footer message](#footer_message)
  - [Profile menu customization](#profile_menu)
- [Add Custom Modules](#add_custom_modules)
- [Collection creation](#collection_creation)
- [Collection links](#linking_collections)
- [Adjusted schema](#adjusted_schema)
  - [Why adding another schema for my collection ?](#adjusted_schema_reason)
  - [How to add it ?](#adjusted_schema_method)
- [Built-in list view](#list_view_feature)
  - [How to use built-in list view?](#list_view_integration)
  - [How to determine the columns content ?](#columns_content)
  - [How to make columns sortable ?](#sortable_columns)
  - [How to manage lists pagination ?](#lists_pagination)
  - [Print as pdf](#print_pdf)
  - [Export to Excel](#export_excel)
- [Route creation](#route_creation)
- [Form generator](#form_generator)
  - [Translatable fields](#translatable_fields)

## <a name="intro">Introduction</a>

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

## <a name="getting_started">Getting Started</a>

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

---

## <a name="layout_customization">Layout customization</a>

### <a name="title">Title</a>

To change the document title, simply override it in `custom/startup/client/config.js` like so.

```javascript
document.title = "Your title goes here";
```

### <a name="logo">Logo</a>

Logo is the image appearing in the header, on top of the collapsible sidebar.
Inside `custom/startup/client/config.js` as well, you need to declare a constant named **CustomBranding**, and export it holding a react element (with `logo` className) destined to be your custom logo.
It's preferred to have 2 images for this purpose, given 2 classNames respectively:

1. `logo-mini`: displayed in responsive mode.
2. `logo-lg`: used in full sized screens.
   For best practices, the images used in your project can be put in `/custom-img` folder and called prefixed with `/img/custom-img`.

```javascript
import React from "react";
const CustomBranding = (
  <a href="#" className="logo">
    <span className="logo-mini">
      <img src="/img/custom-img/logo.png" height="30px" />
    </span>
    <span className="logo-lg">
      <img
        src="/img/custom-img/logo.png"
        style={{ height: "40px", paddingBottom: "3px" }}
      />
    </span>
  </a>
);

module.exports = { CustomBranding };
```

### <a name="styles">Styles</a>

In `custom/startup/client/skin.less` define your styling rules then wrap them with one main class, as in the following example.

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

In `custom/startup/client/config.js`, put the wrapper class name in the constant **CustomSkin**, then add it to the module exports.

```javascript
const CustomSkin = "my-custom-skin";
module.exports = { CustomBranding, CustomSkin };
```

However, declaring global variables to use in your custom module's `styles.css` files won't be possible through our `skin.less` file. So the best approach is to create your custom file (for example: `custom/startup/client/mainCss.css`) and import it in `custom/startup/client/index.js`.

```javascript
import "./mainCss.css";
```

### <a name="footer_message">Footer message</a>

In `custom/startup/client/config.js` file, the constant **CustomFooter** is a <footer> tag (with `main-footer` className) holding the message to be shown at the bottom of all pages (usually holds branding and copy rights). This constant would be accessible through the file exports.
As follows:

```javascript
import React from "react";
import moment from "moment";
const CustomFooter = (
  <footer className="main-footer">
    <div className="pull-right hidden-xs">
      <b>Version</b> 1.0.0
    </div>
    <strong>
      Copyright &copy; {moment().format("YYYY")} <a href="">My brand name</a>.
    </strong> All rights reserved.
  </footer>
);
module.exports = { CustomBranding, CustomSkin, CustomFooter };
```

### <a name="profile_menu">Profile menu customization</a>

To override header's profile menu, you'll need to use this file `custom/ui/pages/userBodySubList.jsx` and customize the returned component containing list item(s) having for class **"user-body"**
The wrapper element of the returned component would be :

```javascript
<li className="user-body">...</li>
```

## <a name="add_custom_modules">Adding Custom Modules</a>

Extending the core features in Grow means adding new modules in the customization zone of your project (under `grow/meteor-grow/<project-name>/customizations`)

Let's say you want to add a new feature to your project, for example: Blog posts management..
How to start?
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

So, your `custom-packageName` will be a copy of our `generic-package`.

**First**, you need to ensure you maintain the right files structure, for your code to behave as expected.
It's preferred to organize the package folders accordingly to their code's destination: common code (`both`), `server` code and `client` code.

![package structure](https://portal.navybits.org/web/image/1446/download.png?access_token=9dcac7f8-77dc-4224-83b7-eccc88726dd2)

For more precision, we used multiple files with different purposes in each folder.

![package files](https://portal.navybits.org/web/image/1462/Screenshot%20from%202019-10-16%2016-25-12.png?access_token=03f6f6e7-4b95-443f-81bf-b89822f8c3e5)

<!-- https://portal.navybits.org/web/image/1461/Screenshot%20from%202019-10-16%2016-21-36.png?access_token=8a1e919f-7765-4cf9-9e72-1d2e9b4b8e46 -->
<!-- TODO elaborate on files -->

**Second**, you can handle how you manage these files through `package.js`.

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

**Finally**, you need to add a dependency to your custom package from `custom-packages`.

```javascript
Package.onUse(function(api) {
  let packages = [
    // ...
    "<your-custom-package-name>" //ex: "posts-package"
    // ...
  ];
  // grant this package access to other packages symbols
  api.use(packages);
  // grant the app using this package access to other packages symbols
  api.imply(packages);
});
```

## <a name="collection_creation">Collection creation</a>

"both" holds all files related to the collection and its schema

Add the file `lib/both/schema.js` . Declare and initialize your schema using [`simpl-schema`](https://www.npmjs.com/package/simpl-schema) npm.

```javascript
import SimpleSchema from 'simpl-schema'
const SchemaName = new SimpleSchema({......});
export default SchemaName;
```

In `lib/both/collection.js` , create your mongo collection using your defined schema as follows:

```javascript
import { Mongo } from "mteor/mongo";
import SchemaName from "./schema.js";
var CollectionName = new Mongo.Collection("collectionName");
CollectionName.attachSchema(SchemaName);
export default CollectionName;
```

Then import it in `lib/both/main.js`.

```javascript
import "./collection";
```

<!-- TODO elaborte on attach schema -->

This is to organize and centralize collection declaration code, and to maintain an easy way to reference and change collection's schema.

## <a name="linking_collections">Linking collections</a>

To create links, define needed links in an object (As explained in [grapher documentation](https://cult-of-coders.github.io/grapher/#Linking-Collections)). This same object will be used to initiate grapher links (through `Collection.addLinks`) and to declare the existence of these links internally to the system (using `addGrapherLinks`).
In `lib/both/main.js` write the following code and update [subscribed fields](#lists_pagination)

```javascript
import Collector from "meteor/ui-config-collector";
const { methods: {  addGrapherLinks  } } = Collector;
const links = {
  postOwner: { //custom link name
    type: "one", //either "one" or "many"
    collection: Meteor.users, //collection instance
    field: "createdBy" //field in "posts" collection holding the _id of a record from "users" collection
  },
  .....
};
Meteor.Collection.get("posts").addLinks(links);
addGrapherLinks({ name: "posts", links });
```

## <a name="adjusted_schema">Adjusted schema</a>

### <a name="adjusted_schema_reason">Why adding another schema for my collection ?</a>

The adjusted schema is used by the **Query box** and **Form generator**. It helps identify the type of a field and find out the input that suits it the best.
If customized schema not provided, **Form generator** will not work while **Query box** will depend on the original simpl-schema.

<!-- Important:
It's of the same structure of a schema but it'll not be attached to the collection with attchSchema as the simpl-schema . -->
<!-- How to add the side schema ? -->
<!-- TODO elaborate on fields of type object + if not exist query box will take all original schema fields -->
<!-- TODO what is it like / how to write it -->
<!-- TODO translation -->
<!-- TODO attach schema links, hook, grapher ... -->
<!-- TODO side bar seperators (dividers) -->

### <a name="adjusted_schema_method">How to add it ?</a>

In `lib/both/main.js`, use the method `addSchema` of `ui-config-collector`.
Similarly to the schema declaration in [collection creation](#collection_creation), fields definition can hold: type (String, Number, Boolean, Date, Object...), an array of allowedValues, and a label.
As for foreign fields, additional info are needed: isLink, linkName, linkedField, and linkedCollection.
For example:

```javascript
import Collector from "meteor/ui-config-collector";
const {
  methods: { addSchema }
} = Collector;

const adjustedSchema = {
  title: { type: String, label: "My post title" },
  isPublished: { type: Boolean, label: "Status" },
  shareWith: {
    type: String,
    allowedValues: ["public", "friends", "friends of friends"]
  },
  views: { type: Number },
  likers: { type: Array },
  "likers.$": { type: String },
  createdBy: {
    type: String,
    label: "Field related to another collection",
    isLink: true,
    linkedCollection: "users",
    linkedField: "name", // the field needed from users collection
    linkName: "postOwner" // as it's defined in grapher links
  },
  extraInfo: {
    type: Object,
    location: { type: String },
    postedAt: { type: Date }
  }
};

addSchema({ name: "posts", schema: adjustedSchema });
```

## <a name="list_view_feature">Built-in listing component</a>

### <a name="list_view_integration">How to use built-in list view?</a>

Use the component `ListDefaultView` provided by `ui-config-collector` core package.
Specify your collection name in the props `collectionName`. In some cases, when you need to distinguish 2 different faces of the same collection, you can use `list` property.
This component handles, internally, the subscriptions and publications needed to get the collection data.
But, it won't know what to display, unless you specify the columns you want to show about your records.
To pass to that step, we need to fulfill the [pagination step](#pagination_step).
`ListDefaultView` provides additional props as well.
**List view additional options**:

<!-- HeaderContent
QueryBox
DataCalendar
DataGraph
DataListComponent
PaginationBox -->
<!-- list/collectionList -->

- `listTitle`: optional string to be displayed on top of the resultant list.
- `defaultFilters`: optional query object.
- `sortByCriteria`: optional field name (defaults to `createdAt` field).
- `sortByDirection`: optional "asc" or "desc" (defaults to "desc").

<!--TODO example to be reviewed
// list={`${type}`}
//  allowSelection={true}
// useQueryBox={true}
// hideGroups={true}
-->
<!-- TODO .. overriding a schema like users masalan + hideFromUi option -->

```javascript
import React from "react";
import Collector from "meteor/ui-config-collector";
const {
  components: { ListDefaultView }
} = Collector;

class CollectionList extends React.Component {
  render() {
    let { type } = this.props;
    return (
      <div className="content">
        <ListDefaultView
          collectionName="posts"
          sortByCriteria="type"
          listTitle="My List Title"
          defaultFilters={{
            type: { $eq: type }
          }} /*direct form of a query*/
          getColumnInfo={({ row, column, dataToBeRouted }) => {
            return row && row.type;
          }}
        />
      </div>
    );
  }
}
export default withTracker(props => {
  let type = getNested(props, "match", "params", "type");
  return {
    currentUser: Meteor.user(),
    type
  };
})(CollectionList);
```

This list view includes by default the pagination feature. To disable it, use `hidePagination` prop with **true** value.
In case you want your list paginated but you need to prevent the user from changing the `limit` of records per page, review [Lists pagination](#lists_pagination)

### <a name="columns_content">How to determine the columns content ?</a>

The list component needs to have an array of objects. Each object represents a column.
You can manage the header of a column `label`, and the info displayed in it through `fieldName`.
Even you can manage a special resultant view, by replacing `fieldName` with a **`render`** function. The render function should always expect the record argument as a first param. It optionally receives a second argument which you can build, by adding `getColumnInfo` method in `ListDefaultView`. `getColumnInfo` takes **{row, column, dataToBeRouted}** as params. **row** is the record. **column** is the object you've configured. **dataToBeRouted** are relevant to pagination purposes. What you decide to return from this function will be the second entry param of **`render`** ("info" in the example).
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

### <a name="sortable_columns">How to make columns sortable ?</a>

After declaring the columns array, adding to a column's definition an additional field `sortingPath` will make sortable. `sortingPath` is the path of the record's info you to sort by.
Your code will look something like this:

```javascript
import { Meteor } from 'meteor/meteor'
import Collector from 'meteor/ui-config-collector'
const { methods: { addListColumns } } = Collector
if(Meteor.isClient){
 import React from 'react'

 const myColumns=[
  {
    label:'Titles', fieldName:'title', sortingPath:'title'
  },
  {
    label:'Owner', render: (record, info)=>(<span>....</span>), sortingPath:'createdByUser.name'
  }
.....
 ]
}

addListColumns({name: 'posts', columns: myColumns })
```

### <a name="lists_pagination">How to manage lists pagination ?</a>

One of the main configurations to move on with offered core features is what we call **pagination info**.
This configuration mainly controls the info related to list pagination. It's an object that holds a **`limit`** (of type `number`: defaults to 25 records per page). It supports **`skip`** option as well (of type `number`: defaults to 0 skipped records from the queried data cursor).But the most affective option is **`fields`**.
**`fields`** is an object used to filter the record's info. The object has the fields desired, as keys with value 1 . ([accordingly with grapher's query options](https://cult-of-coders.github.io/grapher/#Query-Options)).
If **fields** is left empty, all schema fields will be returned.
`lib/both/main.js` will hold the following:

```javascript
import Collector from "meteor/ui-config-collector";
const {
  methods: { addPaginationInfo }
} = Collector;

addPaginationInfo({
  name: "posts",
  info: {
    limit: 5,
    skip: 1,
    fields: {
      title: 1
      // ....
    }
  }
});
```

To disable the capability of limit selection change add the previous object the property `preventLimitEdit` and set it to true. Your code will become:

```javascript
import Collector from "meteor/ui-config-collector";
const {
  methods: { addPaginationInfo }
} = Collector;

addPaginationInfo({
  name: "posts",
  info: {
    limit: 5,
    preventLimitEdit: true,
    skip: 1,
    fields: {
      title: 1
      // ....
    }
  }
});
```

As for data resultant from grapher links, it can be found in an object field holding the correspondante link name. This is, for sure, after apssing through [Linking collections](#linking_collections) step.
So in fields you can choose the fields you want to get from the target linked collection, as follows:

```javascript
import Collector from "meteor/ui-config-collector";
const {
  methods: { addPaginationInfo }
} = Collector;

addPaginationInfo({
  name: "posts",
  info: {
    limit: 5,
    preventLimitEdit: true,
    skip: 1,
    fields: {
      title: 1,
      postOwner: {
        name: 1,
        email: 1
        // ...
      }
    }
  }
});
```

### <a name="print_pdf">Print as pdf</a>

If you want to add ability to print data to pdf from list view, you have to configure a print schema
In `lib/both/main.js`, use the method `addPrintSchema` defined in `ui-config-collector`, specify the name of your collection and give it your needed schema object.
If a collection called `providers` has the data model { en: {name, description } }, we have to set the print schema like the following

```javascript
import Collector from "meteor/ui-config-collector";
const {
  methods: { addPrintSchema }
} = Collector;
addPrintSchema({
  name: "providers",
  schema: {
    content: [
      // if you don't need styles, you can use a simple string to define a paragraph
      "This sentence will be replaced by the provider name",

      // using a { text: '...' } object lets you set styling properties
      { text: "This paragraph will not be affected", fontSize: 15 }
    ]
  },
  substitutions: [
    {
      position: "0",
      substitution_field: "en.name"
    }
  ]
});
```

The object structure used in the `schema` field above is following the pdfmake documentation. For more info about how to customize the pdf report you want to build, visit [pdfmake playground](https://pdfmake.github.io/docs/document-definition-object/)

<!--TODO Make sure you add a dependancy on `ui-config-collector" (core package) to use this method in package.js file -->

### <a name="export_excel">Export to Excel</a>

If you want to control the exported excel sheet when using the built in list view, you have to configure a schema in order for the excel generator to be able to build the columns correctly

How to add the export schema ?

In `lib/both/main.js`, use the method `addExportSchema` found in `ui-config-collector` collector.
Specify the name of your collection and give it your needed schema object.
If a collection `posts` has the data model {extraInfo:{location,postedAt}}, we have to set the export schema as follows:

```javascript
import Collector from "meteor/ui-config-collector";
const {
  methods: { addExportSchema }
} = Collector;
addExportSchema({
  name: "posts",
  schema: {
    Location: "extraInfo.location",
    "Creation date": "extraInfo.postedAt"
  }
});
```

<!-- Make sure you add a dependancy on "ui-config-collector" (core package) to use this method -->

## <a name="form_generator">Form generator</a>

### <a name="translatable_fields">Translatable fields</a>

In order to make a field translatable:

1. Add the array of strings `translationIds` to your schema:

```javascript
// ...
translationIds: {
    type:Array,
    optional:true
    },
'translationIds.$':String
```

2. Disable attaching the schema in your `lib/both/collection.js` file

```javascript
// Collection.attachSchema(SchemaName);
```

3. Add translation links with the relevant pagination config, in `lib/both/main.js` file:

```javascript
import Collector from "meteor/ui-config-collector";
const {
  methods: { addGrapherLinks, addPaginationInfo, addGrapherCacheFields }
} = Collector;
import Collection from "./collection";
import CollectionName from "./collectionName";
const links = {
  // ...
  liveTranslations: {
    type: "many",
    collection:
      Meteor.Collection.get("translations") ||
      new Mongo.Collection("translations"),
    field: "translationIds"
  }
};
Collection.addLinks(links);
addGrapherLinks({ name: CollectionName, links });

const paginationInfo = {
  limit: 10,
  skip: 0,
  fields: {
    // ....
    translationIds: 1,
    liveTranslations: {
      fieldName: 1,
      content: 1,
      language: 1,
      lang: 1
    }
  }
};
addGrapherCacheFields({ name: CollectionName, info: paginationInfo });
```

4. Create a file `lib/both/collection-attach-schema` that will extend the schema with the relevant translation fields for auto caching of translations inside the record and then re-attach the resultant schema to your collection, as follows:

```javascript
import collectionNameSchema from "./collectionSchema";
import CollectionName from "./collectionName";
import Collector from "meteor/ui-config-collector";
const {
  methods: { getSchemaExtensionFromLinks }
} = Collector;

import Collection from "./collection";
import SimpleSchema from "simpl-schema";

collectionNameSchema.extend(
  new SimpleSchema(getSchemaExtensionFromLinks(CollectionName))
);

Collection.attachSchema(collectionNameSchema);
export default Collection;
```

5. Require `collection-attach-schema` by `lib/both/main.js` file:

```javascript
require("./collection-attach-schema");
```

6. To add a db hook for caching relational fields inside the record document, in your `lib/server/main.js` file add the following:

```javascript
import Collector from "meteor/ui-config-collector";
const {
  methods: { addGrapherLinkCacheHook }
} = Collector;
import CollectionName from "../both/collectionName";
addGrapherLinkCacheHook(CollectionName);
```

7. Finally, `FieldComponent` needs the prop `translatable` to hold the current edited record "_id"

```javascript
<FieldComponent
  // ...
  translatable={recordId}
/>
```

**Note**: make sure that the version of simpl-schema used is above 1.4.2 (`"simpl-schema" : "^1.4.2"`)

## <a name="route_creation">Route creation</a>

In your custom package `lib/client/ui-config.js` file, use the method `addCustomRoutes` to declare the routes and to specifiy their main parameters.

1. `path` : based on [React Router v4](https://reacttraining.com/react-router/web/example/route-config). When using [route params](https://reacttraining.com/react-router/web/api/Hooks/useparams), we can get them by tracking component's `props`(data routed can be found in: `props`->`match`->`params`).
2. `component` : It's represents the page content to route to, through this path
3. `name` : It's a unique route name
4. `group` : It can be a string or an array of string. It checks the existence of this/these group(s) in current user roles. It depends on the group name configured especially for the related collection. Review [Group declaration](#group_config).
   <!-- TODO HOW to add a Group ? + elaborate on roles-->
   Note: no `collectionName` is required by this method, it only takes an array of objects(routes).

```javascript
import Collector from "meteor/ui-config-collector";
const {
  methods: { addCustomRoutes }
} = Collector;
import { CollectionList, CollectionEditor } from "./components";
addCustomRoutes([
  { path: "/posts", component: CollectionList, name: "Posts list" },
  {
    path: "/posts/manage/new",
    group: "Posts",
    component: CollectionEditor,
    name: "New post"
  },
  {
    path: "/posts/:recordId", // Inside "CollectionEditor" component use props.match.params.recordId
    group: ["Posts", "Users"],
    component: CollectionEditor,
    name: "Post Editor"
  }
]);
```
