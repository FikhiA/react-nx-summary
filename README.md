# Effective React Development with Nx

# Introduction
When working at a company with more than one team, most of the times development teams are grouped by their domain or technology. 
For example, one team builds UI in React, and another one builds API in Express. 

They usually have separate code repositories, so a change in the software as a whole requires working on multiple repositories.

This multi-repo style, often called "Polyrepo" causes a few glaring problems, such as:

-  Lack of collaboration, because sharing code is harder.
-  Lack of consistency in lint, test, and release (you have to keep track of a ton of config files in separate repos!)
-  Lack of mobility between projects because lack of access or different development styles
-  Difficult to coordinate changes across repos
-  In the end, it will cause lots of bugs during integration (integration hell!)

**Well, is there even another way of approaching this? :(**

Surprisingly, there is! It's called monorepo!

**What the f is monorepo, anyway?**

In short, it is a single repo containing multiple projects (and even using different techs) at once!

Not only does this provide code colocation (all code, all in one place), it can also define relationships between them!

**What's in it for me, then?**

A proper monorepo approach can prevent head-bashing problems and wasted time :)

Even a lot of successful organizations such as Google and Microsoft, and large open source projects like React and Babel use monorepo! Sooo let's just dive right into it!

# Chapter 1: Getting Started with Nx

Before we go anywhere, let's define some things first.

- **Workspace** is a folder which contains apps and libraries. Simply, the whole repo.
- **Project** is an app or library within the Workspace.
- **Application** is something that you want to develop
- **Library** is a set of files to deal with something specific, like parsing json or centering a div :D 

## Creating a Nx workspace
Make sure you have NodeJS already installed, and run

```$ npx create-nx-workspace@13.3.0```

If it asks you to install something, just press Y and enter. 

It will look something like this.

For **workspace** name, i will use `zeroone`. and for the preset, pick `react`. 

![](images/1%20-%201.jpg)

You will be prompted for the **application** name and styling format. Let's use `bookstore` as the app name and `styled-components` for styling.

![](images/1%20-%202.jpg)

Nx asks you to set up Nx Cloud. It supposedly provides cloud-enhanced features, but we aren't going to worry about that for now.

![](images/1%20-%203.jpg)

After a couple of moments, Nx should finish creating the workspace, and we will end up with something like this.
<details>
<summary>Generated Folder Tree (click me!)</summary>

```
.
├── apps
│ ├── bookstore
│ │ ├── src
│ │ ├── jest.config.js
│ │ ├── project.json
│ │ ├── tsconfig.app.json
│ │ ├── tsconfig.json
│ │ └── tsconfig.spec.json
│ └── bookstore-e2e
│ ├── src
│ ├── cypress.json
│ ├── project.json
│ └── tsconfig.json
├── libs
├── babel.config.json
├── jest.config.js
├── jest.preset.js
├── README.md
├── nx.json
├── package-lock.json
├── package.json
├── tools
│ ├── generators
│ └── tsconfig.tools.json
├── tsconfig.base.json
└── workspace.json
```

</details>

---

The `apps` folder contains the code of all apps, inside of which 2 has been created by default right now: 
- The `bookstore` app, and 
- Its end-to-end (e2e) tests using Cypress.

The `libs` folder contains our libraries, currently empty.

The `tools` folder can be used for scripts that are workspace-specific.

The `nx.json` file configures Nx.

Aaand to start the app, type in `npm start` to the terminal.

This will start the `bookstore` app and you can open it at [http://localhost:4200](http://localhost:4200) 

We are greeted with a welcome page, yaay!

![](images/1%20-%204.jpg)

Great job on following along this far! if you want to read more about Nx workspace config, click below! (might require quite a bit of knowledge, be warned ._. )

<details>
<summary>Nx Workspace configs</summary>

TODO

</details>

---

# Chapter 2: Libraries

TODO

# Chapter 3: Working Effectively in Monorepo

TODO

# Chapter 4: Bringing it all together

TODO