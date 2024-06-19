---
layout: page
title: Explorer and First Queries
nav_order: 3
has_children: false
permalink: /explorer/home/
---

# Explorer and First Queries

Now that you're in good shape to use the MFG Data Model API, we can start experimenting with the queries and the explorer to get more comfortable with this new service.

In this section, we'll introduce you to the interface that will help you explore your design data, focusing mainly on the API. As said before, we don't want you to worry about frameworks, coding, and cloud providers. We can keep it simple using the [explorer](https://mfgdatamodel-explorer.autodesk.io).

The explorer's UI was built to be simple and intuitive. We'll use it mostly to perform our queries by passing the payload and checking the response, just like in the image below:

![Explorer UI](/mfgdm-api-tutorial/assets/images/explorerui.png)

> _The explorer is built on top of the [graphiql](https://github.com/graphql/graphiql) project! If you want additional details on this project, feel free to check its documentation ;)_

It also comes with multiple functionalities to check the history of queries, format queries, configure themes and shortcuts, and a button to display the queries available with the API. This last option is the first one we'll go through, as it provides us access to our API schema. This will be our entry point. We'll start our journey by getting familiar with the MFG Data Model API schema.

## MFG Data Model Schema

As described at [graphql.org](https://graphql.org/learn/schema/):

> _"Every GraphQL service defines a set of types which completely describe the set of possible data you can query on that service. Then, when queries come in, they are validated and executed against that schema."_

Our API has a schema suitable to address the common data from the MFG industry. The main objects described below:

- **DesignItem**: Represents an item that contains a product design. You coudl think of it as the file in your project.
- **DesignItemVersion**: A specific version of a design.
- **Component**: Component is the way you can organize your model into parts. Each design has a root component, and each component can have multiple sub-components. 
- **ComponentVersion**: A specific version of a component.
- **Occurrence**: An instance of a component inside another component.
- **Properties**: Custom properties of a ComponentVersion. 
- **PhysicalProperties**: Physical properties of a ComponentVersion: area, volume, density, mass and bounnding box.
- **Derivatives**: Export file formats that can be requested for a ComponentVersion: STEP, STL or OBJ.
- **ManagePropertiesOnVersion**: Lifecycle information related to the ComponentVersion: itemNumber, lifeCycle, etc. 

### Explorer Docs

Now let's use the explorer to view our schema.

Log in with your Autodesk account, then click on the Docs button and scroll down to access the queries available in the MFG Data Model's schema.

![Schema through explorer](/mfgdm-api-tutorial/assets/images/docs.png)

The first query we used in the previous section returned to us a list of hubs. According to this documentation we could, for instance, use a filter to retrieve only the hubs matching certain conditions. Exploring the schema gives us a better idea about the capabilities of the API. If you scroll down you'll see a list with all the queries available including the parameters that can be passed to compose the responses.

### GraphQL Voyager

> There's also another great tool to explore GraphQL API schemas:
> The [GraphQL Voyager](https://mfgdatamodel-explorer.autodesk.io/voyager)

![Voyager](/mfgdm-api-tutorial/assets/images/voyager.png)

With that you will be able to inspect all the available queries and constructs from MFG Data Model API.

> _Keep one tab with the schema open for further exploration throughout this tutorial ;)_

Now that we know the schema's importance and know how to view it using the explorer, we can continue with the subsequent queries.

## First Queries

We suppose that you're familiar with how the data is organized in the context of Fusion hubs but if not, here is a quick overview:

At the top level, there are the hubs.
Inside each hub, there are the projects.
Inside a project, there are multiple folders.
Inside a folder, there can be other folders or items.
Lastly, an item can have multiple versions.

![Fusion Team hierarchy](/mfgdm-api-tutorial/assets/images/hierarchy.png)

Let's traverse this structure through our queries in 4 steps:

### Step 1 - Listing the hubs

The query to retrieve the hubs is quite simple and it is available in the first pane of the explorer. To list the hubs available you just need to click in the first panel of the explorer and then run the query, like the image below:

![GET hubs](/mfgdm-api-tutorial/assets/images/gethubs.png)

In the next query, you'll need to use your hub id as input.

> _This id is the same one used by other APS APIs (e.g. Data Management) to point to hubs._

### Step 2 - Listing the projects

Following the hierarchy, we're going to list all of the projects available inside one hub. For that, we'll need to provide the hub id as input for the get projects query.

Go ahead and copy the id of the hub you're using, move to the `GetProjects` pane, and paste the id in the proper field, just like in the image below:

![GET projects](/mfgdm-api-tutorial/assets/images/getprojects.png)

Now you'll need to find the project that hosts your Fusion designs for this tutorial.

The GetProjects query available in the explorer, as is, requires you to paste the `hub id` as a string argument, but you can also asign that using the Variables.

Using Variables instead, the query would be just like the one below:

```js
query GetProjects ($hubId:ID!) {
  projects(hubId: $hubId) {
    pagination {
      cursor
    }
    results {
      id
      name
      alternativeRepresentations{
        externalProjectId
      }
    }
  }
}
```

And in Variables space, the id of the hub:

```js
{
  "hubId": "YOUR HUB ID HERE!"
}
```

This way is better to address variables as they can be assigned multiple time easier at any place in the query, and we don't need to change any value in the query to point to a different hub.

In case your hub has many projects making the one you need to use missing from the first page (or even hard to find), there's a way to filter the response.

For that you can filter the projects by name, passing the name of your project like the gif below:

![GET projects](/mfgdm-api-tutorial/assets/images/getprojectsfilter.gif)

For simplicity, you can just copy and paste the query below if needed (replacing it with your project name and hub id) ;)

```js
query GetProjects ($hubId:ID!, $projectName:String!) {
  projects(hubId: $hubId, filter:{name:$projectName}) {
    pagination {
      cursor
    }
    results {
      id
      name
      alternativeRepresentations{
        externalProjectId
      }
    }
  }
}
```

```js
{
  "hubId": "YOUR HUB ID HERE!",
  "projectName": "YOUR PROJECT NAME HERE!"
}
```

The next query requires a project id, and MFG Data Model API works with its unique value for the project id. That's why it exposes the usual project id inside the `alternativeRepresentations` field.
We are not going to use the alternative representation for the projects in this tutorial but is always good to know how to retrieve it. You'll need it if you want to connect with Data Management APIs, for instance.

### Step 3 - Listing Designs

Usually inside a project, we have a complete structure of folders separating files according to project phase, disciplines, teams, etc...
You might be used to traverse this folder structure to reach your items level, but that isn't necessary when we use MFG Data Model API.





And with that, we covered the first queries with the MFG Data Model API.
In the next step, we'll understand how this connection with the viewer works and explore more complex queries.

[Next Step - Async Operations](../../async/home/){: .btn}
