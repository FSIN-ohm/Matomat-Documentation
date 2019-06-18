Preparing the development environment
==

The following steps will guide you through the process of preparing the development environment for the admin and user-frontend on your computer.

## Step 1: Installing a suitable source code editor

We recommend using Visual Studio Code, it's available for Windows, macOS and Linux. Furthermore does it support JavaScript, TypeScript, Node.js and npm package manager. All four are required to edit the project files.</br>

Download Visual Studio Code [here](https://code.visualstudio.com/Download)

### Step 2: Installing Node.js

Node.js is an free open source server environment which can generate dynamic page content.</br>
Angular requires Node.js version 8.x or 10.x.</br>

Check the version you're running: Open your command line interface (CLI), and type `node -v`

If Node.js is not installed download it from [here](https://nodejs.org/en/)

### Step 3: Installing the npm (Node Package Manager)

NPM is a package manager for Node.js packages. A package in Node.js contains all the files you need for a module. Modules are JavaScript libraries Angular, the Angular CLI, and Angular apps depend on. Learn more [here](https://docs.npmjs.com/about-npm/index.html).</br>
In order to download and install npm packages, you need to have such a package manager installed.

This Quick Start uses the [npm client](https://docs.npmjs.com/cli/install) command line interface, which is installed with Node.js by default.

To check whether you have the npm client installed, run `npm -v` in a CLI.

### Step 4: Installing the Angular CLI

The Angular CLI was used to create the Matomat. Find out more about Angular, [here](https://angular.io/) </br>
To install the CLI using npm, run the following command in your command line interface:

##### Windows: `npm install -g @angular/cli`

##### MacOS/Linux: `sudo npm install -g @angular/cli`

### Step 5: Cloning the Github Repository

In order to edit the project files, in your chosen source code editor, clone the project folder from our official Github Repository on your computer.

#### Admin Frontend: [https://github.com/FSIN-ohm/Matomat-Admin-Frontend](https://github.com/FSIN-ohm/Matomat-Admin-Frontend)
#### Frontend: [https://github.com/FSIN-ohm/Matomat-Frontend](https://github.com/FSIN-ohm/Matomat-Frontend)

### Step 6: Setup the source code editor

Now that you've cloned the project folder to your workstation. Follow these steps to prepare Visual Studio Code for deploying the project, the process could be different with other editors:
#### Setting up the file explorer:
######  `File > Open Folder > "Path"\Matomat-Admin-Frontend > open folder`
######  `File > Open Folder > "Path"\Matomat-Frontend > open folder`

#### Installing required extensions for the Admin-Frontend:

######  `Terminal > New Terminal`
######  run: `ng add ngx-bootstrap`

#### Deploy the Admin-Frontend:

######  Terminal: `ng serve -o`

#### Common error:

###### `ERROR in (webpack)/hot/emitter.js`
###### Run this fix: `npm install events`
</br>
Congratiolations, now you should be able to edit the Matomat-Frontend and Matomat-Admin-Frontend.
