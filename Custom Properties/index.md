---
layout: page
title: Custom Properties
nav_order: 6
has_children: false
permalink: /properties/home/
---

# Custom Properties

Custom Properties allow you to attach your own properties and values to any component version of your Fusion designs. This functionality differentiates 3 personas: 
- Custom Property Admin: the owner of the APS app who creates the property definitions and organizes them into collections
- Fusion Hub Admin: the person who, using the Custom Property Admin's app, can link a specific Property Definition Collection to the hub they are the admin of
- End Users: the users of the hub that has a property Definition Collection linked to it. These users can access these properties on all the component versions and modify their values

Now let's go through each persona and see what they can do using the API.

## Custom Property Admin

In order to create Property Definitions you need to create an APS app that will provide the credentials we need to use: [Create App](https://aps.autodesk.com/en/docs/oauth/v2/tutorials/create-app/) 

For the following opersations you can either use **2-legged** tokens, or the APS app's owner needs to be logged in so that the **3-legged** token is generated for them. Let's just use a **2-legged** token, which is simpler - see image:

![Viewer Connection Process](/mfgdm-api-tutorial/assets/images/2lo.png)





Now you've covered many possible scenarios enabled by AEC Data Model API.
From now you have more than enough to start experimenting with your custom workflows on your designs.
We also have a few samples with live demos and source code available for you to leverage.

[MFG Data Model Explorer source code](https://github.com/autodesk-platform-services/aps-data-explorer)

[MFG Data Model Code Samples source code](https://github.com/autodesk-platform-services/aps-fusion-data-samples/tree/beta)

[MFG Data Model Demo source code](https://github.com/autodesk-platform-services/aps-mfgdatamodel-demo)
