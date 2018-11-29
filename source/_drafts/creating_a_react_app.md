---
title: create_react 
category: diary
permalink: creating_a_react_app
author: David Lin
tags:
  - diary
description: Creating a React App
---

Creating a React App | 创建一个 React 应用

INTRODUCTION

React is a user interface framework developed by Facebook. It has a quickly growing developer adoption rate and was ranked as the most loved language or technology in the 2017 Stackoverflow developer survey. This article will walk you through setting up your first React app and assumes you are familiar with text editors and command line navigation. We will be using the Node package manager (npm), so you will need to have Node installed.

1. SET UP THE BOILERPLATE APPLICATION
It is possible to manually create a React app, but Facebook has created a node module create-react-app to generate a boilerplate version of a React application.

Besides providing something that works out-of-the-box, this has the added benefit of providing a consistent structure for React apps that you will recognize as you move between React projects. It also provides an out-of-the-box build script and development server.

We will use npm to install the create-react-app command line interface (CLI) globally (-g).

Open up your terminal and run npm install -g create-react-app: createReactApp

Now that you have the CLI available for use, navigate to the parent directory that you would like to place the application within. Then, run create-react-app with the name you would like to use for your project (just no capital letters :-) ).

create-react-app <name-of-app>

Upon completion, you will get some quick tips on how to use the application:

createReactAppCommands

Before we run the app, lets take a look around the app structure and see what it contains.

2. REACT APP STRUCTURE
Change directories into the app you just created. If you list the contents of the directory including hidden files (ls -la), you should see the following structure:

createReactAppStructure

create-react-app has taken care of setting up the main structure of the application as well as a couple of developer settings. Most of what you see will not be visible to the visitor of your web app. React uses a tool called webpack which transforms the directories and files here into static assets. Visitors to your site are served those static assets.

.gitignore

This is the standard file used by the source control tool git to determine which files and directories to ignore when committing code. While this file exists, create-react-app did not create a git repo within this folder. If you take a look at the file, it has taken care of ignoring a number of items (even .DS_Store for Mac users):

createReactAppGitIgnore

package.json

createReactAppPackageJson

This file outlines all the settings for the React app.

name is the name of your app
version is the current version
"private": true is a failsafe setting to avoid accidentally publishing your app as a public package within the npm ecosystem.
dependencies contains all the required node modules and versions required for the application. Here, it contains two dependencies, which allow us to use react and react-dom in our JavaScript. In the screenshot above, the react version specified is ^15.5.4. This means that npm will install the most recent major version matching 15.x.x. In contrast, you may also see something like ~1.2.3 in package.json, which will only install the most recent minor version matching 1.2.x.
devDependencies contains useful node modules and versions for using the React app in a development environment. Here, it contains one dependency, react-scripts, which provides a set of useful development scripts for working with React.
scripts specifies aliases that you can use to access some of the react-scripts commands in a more efficient manner. For example running npm test in your command line will run react-scripts test --env=jsdom behind the scenes.
node_modules

This directory contains dependencies and sub-dependencies of packages used by the current React app, as specified by package.json. If you take a look, you may be surprised by how many there are.

Running ls -1 | wc -l within the node_modules/ directory will yield more than 800 subfolders. This folder is automatically added to the .gitignore for good reason! Don’t worry, even with all these dependencies, the basic app will only be around 50 KB after being minified and compressed for production.

package-lock.json

This file contains the exact dependency tree installed in node_modules/. This provides a way for teams working on private apps to ensure that they have the same version of dependencies and sub-dependencies. It also contains a history of changes to package.json, so you can quickly look back at dependency changes.

public

This directory contains assets that will be served directly without additional processing by webpack. index.html provides the entry point for the web app. You will also see a favicon (header icon) and a manifest.json.

The manifest file configures how your web app will behave if it is added to an Android user’s home screen (Android users can “shortcut” web apps and load them directly from the Android UI). You can read more about it here.

src

This contains the JavaScript that will be processed by webpack and is the heart of the React app. Browsing this folder, you see the main App JavaScript component (App.js), its associated styles (App.css), and test suite (App.test.js). index.js and its styles (index.css) provide an entry into the App and also kicks off the registerServiceWorker.js. This service worker takes care of caching and updating files for the end-user. It allows for offline capability and faster page loads after the initial visit. More on this methodology is available here (https://goo.gl/KwvDNy).

As your React app grows, it is common to add a components/ directory to organize components and component-related files and a views directory to organize React views and view-related files.

3. STARTING THE REACT APP DEVELOPMENT SERVER
As was stated in the success message when you ran create-react-app, you just need to run npm start in your app directory to begin serving the development server. It should auto-open a tab in your browser that points to http://localhost:3000/ (if not, manually visit that address). You will be greeted with the React welcome banner:

createReactAppDemo

As stated, any changes to the source code will live-update here. Let’s see that in action.

Leave the current terminal tab running (it’s busy serving the React app) and open src/App.js in your favorite text editor. You’ll see what looks like a mashup of JavaScript and HTML. This is JSX, which is how React adds XML syntax to JavaScript. It provides an intuitive way to build React components and is compiled to JavaScript at runtime. We’ll delve deeper into this in other content, but for now, let’s make a simple edit and see the update in the browser.

Change the main paragraph text to read: Hello Codecademy! in App.js and save the file.

createReactAppAppJs

If you left the terminal running, you should be able to switch over to your browser and see the update:

createReactAppDemoUpdate

Congratulations! You’re now up and running with React. You can clean up any unnecessary files and begin adding functionality for your application.