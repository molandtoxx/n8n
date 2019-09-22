# Create Node

It is quite easy to create own nodes in n8n. Mainly three things have to be defined:

 1. Generic information like name, description, image/icon
 1. The parameters to display via which the user can interact with it
 1. The code to run once the node gets executed

To simplify the development process we created a very basic CLI which creates boilerplate code to get started and then also builds the node (as they are written in TypeScript) and copies it to the correct location.


## Create the first basic node

 1. Install the n8n-node-dev CLI: `npm install -g n8n-node-dev`
 1. Create and go into the newly created folder in which you want to keep the code of the node
 1. Use CLI to create boilerplate node code: `n8n-node-dev new`
 1. Answer the questions (btw. Type “Execute” is the regular node you probably want to create).
    It will then at the end create the node in the current folder
 1. Program… Add the functionality to the node
 1. Build the node and copy to correct location: `n8n-node-dev build`
    That command will build the JavaScript version of the node from the TypeScript code and copies it then
    the user folder where custom nodes get read from `~/.n8n/custom/`
 1. Restart n8n and refresh the window that the new node gets displayed


## Node Development Guidelines


That everything works correctly, similarly and no unnecessary code gets added it is important to follow the following guidelines:


### Write nodes in TypeScript

All code of n8n is written in TypeScript so should also be the nodes. That makes development easier and faster and avoids at least some bugs.


### Do use the built in request library

Some third-party services have their own libraries on npm which make it a little bit easier to create an integration. It can be quite tempting to use them. The problem with those is that you add another dependency and not just one you add also all the dependencies of the dependencies. That means that more and more code gets added, has to get loaded, can introduce security vulnerabilities and bugs and so on. So please use the built-in module which can be used like this:

```typescript
const response = await this.helpers.request(options);
```

That is simply using the npm package `request-promise-native` which is the basic npm `request` module but with promises.


### Reuse parameter names

When a node can perform multiple operations for example edit and delete some kind of entity. It would need for both operations an entity-id. Do not call them "editId" and "deleteId" simply call them "id". n8n can handle multiple parameters with the same name without a problem as long as only one is visible. To make sure that is the case the "displayOptions" can be used. By keeping the same name, the value can be kept if a user switches the operation from for example "edit" to "delete".


### Create an "Options" parameter

Some nodes may need a lot of options. Add only the very important ones to the top level and create for all the other ones an "Options" parameter where they can be added if needed. So the interface stays clean and does not unnecessarily confuse people. A good example of that would be the "XML Node".


### Follow exiting parameter naming guideline

Ok, there is not much of a guideline yet but if your node can do multiple things call the parameter which sets the behavior either "mode" (like "Merge" and "XML" node) or "operation" like the most other ones. If this operations can be done on different resources (like "User" or "Order) create a "resource" parameter (like "Pipedrive" and "Trello" node)


### Node Icons

Check existing node icons as a reference when you create own ones. The resolution of an icon should be 60x60px and saved as png.