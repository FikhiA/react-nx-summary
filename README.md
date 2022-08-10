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
- `bookstore-e2e`, which is its end-to-end (e2e) tests using Cypress.

The `libs` folder contains our libraries, currently empty.

The `tools` folder can be used for scripts that are workspace-specific.

The `nx.json` file configures Nx.

Aaand to start the app, type in `npm start` to the terminal.

This will start the `bookstore` app and you can open it at [http://localhost:4200](http://localhost:4200) 

We are greeted with a welcome page, yaay!

![](images/1%20-%204.jpg)

Great job on following along this far! if you want to read more about advanced Nx stuff, click below! 
(might require quite a bit of knowledge though, be warned ._. )

Or else you can just skip them :)

<details>
<summary><b>Nx Workspace Configs</b></summary>

Nx is built in a modular fashion, and gives you a lot of foundation such as dependency graph calculation, running generators and migrations, computation caching, and also a set of plugins to provide technology specific features, which provides you to gradually dive deeper into Nx features.

Nx provides specific config files:
- `nx.json` which is at the root of the workspace and to configure Nx CLI (set defaults for projects and code scaffolding, workspace layout, task runner, and computation cache configs)
- `workspace.json` is also at the root of the workspace is optional and can be used to list the projects in your workspace explicitly, instead of having Nx scan it for you.
- `project.json` is at the root of every project, which contains specific metadata and "targets" or tasks that can be ran on the project. Such as `build`, `serve`, `lint`, `test` and can be configured however you see fit.

</details>

<details>
<summary><b>Nx Commands</b></summary>

"Targets" mentioned in the previous section can be invoked or ran with `nx [target] [project]`

For example, in our `bookstore` app, we can run these targets.

```
# Serve the app
npx nx serve bookstore

# Build the app
npx nx build bookstore

# Run a linter for the application
npx nx lint bookstore

# Run unit tests for the application
npx nx test bookstore

# Run e2e tests for the application
npx nx e2e bookstore-e2e

# You can also see the dependecy graph of our workspace
npx nx dep-graph
```

Give it a try!

</details>

<details>
<summary><b>Installing Nx globally</b></summary>

If you don't want to type in `npx` for every command, you can install Nx globally with 

`npm install -g @nrwl/cli`

And check that it installed properly with `nx --version`

So, next time you run a command, you only need to type in `nx ...` without the `npx`.

</details>

---

## Preparing for Development

Let's end this chapter by removing the generated content from the `bookstore` app and adding some configs to the workspace.

Open up your favorite code editor and modify these three files.

**apps/bookstore/src/app/app.tsx**
```
import styled from 'styled-components';

const StyledApp = styled.div``;

export const App = () => {
  return (
    <StyledApp>
      <header>
        <h1>Bookstore</h1>
      </header>
    </StyledApp>
  );
}

export default App;
```

**apps/bookstore/src/app/app.spec.tsx**
```
import { getGreeting } from '../support/app.po';

describe('bookstore', () => {
  beforeEach(() => cy.visit('/'));

  it('should display welcome message', () => {
    getGreeting().contains('Bookstore');
  });
});
```

**apps/bookstore-e2e/src/integration/app.spec.ts**
```
import { getGreeting } from '../support/app.po';

describe('bookstore', () => {
  beforeEach(() => cy.visit('/'));

  it('should display welcome message', () => {
    getGreeting().contains('Bookstore');
  });
});
```

Make sure the tests still pass:
- `nx lint bookstore`
- `nx test bookstore`
- `nx e2e bookstore-e2e`

If you serve the app again ([at http://localhost:4200](http://localhost:4200)) you will end up with something like this: 

![](images/1%20-%205.jpg)

It's a bookstore alright. Might not look great, but hey, you made some progress!

Also, it's a good idea to commit your code before making any more changes. So you can keep track of your progress and can rollback if anything goes south :D

```
git add .
git commit -m 'end of chapter one'
```

**Summary:**

A typical Nx *workspace* consists of two types of projects: *applications* and *libraries*.

A newly created workspace comes with a set of targets we can run on the generated application: `lint`, `test`, and `e2e`.

Nx also has a tool for displaying the *dependency graph* of all the projects within the workspace.

# Chapter 2: Libraries

Now that we have a skeleton of the app, we can start adding to our app by creating and using *libraries*!
Before that though, let's understand a little more about the concept of libraries.

## Apps and Libs

As alluded to before, a typical Nx workspace contains "apps" and "libs". This separation helps

# Chapter 3: Working Effectively in Monorepo

TODO

# Chapter 4: Bringing it all together

TODO