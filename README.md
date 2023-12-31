# Space2Study-BackEnd-mvp
<a href="https://softserve.academy/"><img src="https://github.com/ita-social-projects/Space2Study-BackEnd-mvp/blob/main/photo.jpg" title="SoftServe IT Academy" alt="SoftServe IT Academy"></a>

# SpaceToStudy project

SpaceToStudy project is a platform where experts in various fields share their knowledge and students can learn from the best. Here you can find the proper training course, find a tutor, or find students and receive feedback from them.


[![GitHub issues](https://img.shields.io/github/issues/ita-social-projects/Space2Study-BackEnd-mvp)](https://github.com/ita-social-projects/Space2Study-BackEnd-mvp/issues)
[![Pending Pull-Requests](https://img.shields.io/github/issues-pr/ita-social-projects/Space2Study-BackEnd-mvp?style=flat-square)](https://github.com/ita-social-projects/Space2Study-BackEnd-mvp/pulls)
[![GitHub license](https://img.shields.io/github/license/ita-social-projects/Space2Study-BackEnd-mvp)](https://github.com/ita-social-projects/Space2Study-BackEnd-mvp/blob/main/LICENSE)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/horondi/horondi_client_fe)](https://s2s-front-stage.azurewebsites.net/)

---

- [Installation](#installation)
  - [Required to install](#Required-to-install)
  - [Clone](#Clone)
  - [Setup](#Setup)
  - [How to run local](#How-to-run-local)
- [Usage](#Usage)
  - [How to run tests](#How-to-run-tests)
- [Documentation](#Documentation)
  - [Rules and guidelines](#Rules-and-guidelines)
  - [Testing](#Testing)
  - [Generator](#Generator)
- [Project deploy](#project-deploy)
- [Contributing](#contributing)
  - [git flow](#git-flow)
  - [issue flow](#git-flow)
- [Teams](#teams) 
  - [Development team](#development-team) 
  - [DevOps team](#devops-team)  
- [FAQ](#faq)
- [Support](#support)
- [License](#license)

---

## Installation

- All the `code` required to get started

### Required to install

- NodeJS (18.14.0 LTS)

### Clone

- Clone this repo to your local machine using `https://github.com/ita-social-projects/Space2Study-BackEnd-mvp.git`

### Setup

> install npm packages

```shell
$ npm install
```

### How to run local

1. Open terminal.
2. Run `npm run start` to start application.<sup>[*](#footnote)</sup>
3. Open http://localhost:3000 to view it in the browser.

###### <a name="footnote">*</a> - to run the project you need an `.env` file in root folder


## Usage

### How to run tests

To run unit test open terminal and run `npm run test` in it.
To run E2E tests you need open terminal and run `npm run start` in it to start server.
Then open one more terminal and run `npm run cypress`.

---

## Documentation

### Rules and guidelines

- Redux
  - For each entity we should have separate folder
  - In each folder we should have different files for actions, reducer
    `{modelName}.actions.js` or `{modelName}.reducer.js`
- Configuration
  - Configuration is done via `.env` file where environment
    variables are located
  - Also we have `.env.example` that contains examples of environment
    variables
- Styles
  - For styling function `makeStyles` from `@material-ui`
    should be used and all styles should be located inside separate
    component.
- Components
  - Components that are connected to Redux should be located inside
    `containers` folder. Components without connection to Redux should
    be located inside `components` folder.
  - Each individual page that is accessed via `react-router`
    should be located inside `pages` folder. All components
    that are used inside particular page should be located inside
    folder for the specific page.
  - Each component should have at least three files:
    - `index.js` where we export anything from the whole folder
    - `{component-name}.jsx` - file where component is located
    - `{component-name}.styles.js` where all styles are located
- File naming
  - Files should be name in format `some-component.type.jsx`
- Architecture
  - Logic is separated in layers
    - resolver layer (handles graphql actions)
    - service layer (handles business logic, interactions with database)
    - model layer (mapping collections to mongoose models)
  - All business logic (any database operations or validation related to the business rules) and interaction with models is located inside service
  - Each model should live in its own folder
  - In each folder files should be named in format `{model-name}.{type}.js`
    (like `{model-name}.service.js` or `{model-name}.resolver.js`)
  - For each model we define class like `{ModelName}Service`
    in `{model-name}.service.js` and have separate methods for handling different types of operations
- Configuration
  - All configuration is implemented via environment variable that is located inside
    `.env` file

### Testing

 - Tests are implemented in the format of contract tests. We test actual GraphQL operations like queries, mutations, or subscriptions on the running application.
    For testing, we should use a database that is running as a container locally.
    We should have a folder per entity with tests.
  - Test files:

    - {entityName}.queries.spec.js - Testing the queries (if it exists)
    - {entityName}.mutations.spec.js - Testing the mutations (if it exists)
    - {entityName}.subscriptions.spec.js - Testing the subscriptions (if it exists)

  - Testing guides:
    1.  All fields in data from the response from the backend should be checked for the appropriate value.
    2.  If you cannot test some field for some particular value you should at least check its existence and its type.
    3.  We should test the validation of the provided data to ensure that the backend performs validation by sending different combinations of valid and invalid data.
    4.  Group tests for each operation (query, mutation, or subscription) to describe the statement.
        Content (base scenario, for some operations we can have additional scenarios):
        - describe(‘Validation’) with tests that validate a particular operation with combinations of valid and invalid data.
        - describe(‘Success business logic’) with tests that perform operations with valid data and ensures that valid flows work
    5.  Tests should be executed before any commit and don’t allow to push code if tests are failing.
    6.  We need to develop utility functions that we can reuse in many tests files for creating user and base authentication (obtaining JWT token) for future performing operations that require authorization
  - Libraries
    - vitest - testing framework
    - apollo-boost - client for performing GraphQL operations

- Runtime work
  - Locally application is running in docker container. We have two docker
    containers: `api` container and `database` container.
    
#### Components

Order of testing components:

1. simple stateless components that are used in multiple places
2. components that depends on other components but not connected to Redux and don’t have any state
3. components that have internal state but are not connected to Redux
4. components that connected to Redux

##### Don’t test:

- third-party libraries
- constants
- static css styles
- related components (test only one specific component at the specific moment of time)
##### How to test:
- testing using snapshots (actual ui)
- testing logic of component (dynamic)

Snapshots allow us to compare actual UI with saved one and throw an error if it has accidentally changed. We can use flag “updateSnapshot” to update save snapshots of a component.
It is appropriate for presentational components but doesn’t cover any logic

##### What to test in components:

- Properties
- default properties
- custom properties
- Data types (use library “jest-extended”)
- Conditions (what if)
- State
- default state
- state after some event has happened
- Events
- with parameters or custom props
- without arguments

#### Sagas

Flow:

- Set up the conditions of our test
- Mock the actual HTTP requests
- Instruct the saga to run through everything and finish its business
- Check that the expected side effects have happened (actions are dispatched, selectors are called, etc)

Link to the full article about proper saga testing: https://dev.to/phil/the-best-way-to-test-redux-sagas-4hib#:~:text=To%20test%20that%20the%20saga,selector%20into%20the%20following%20gen.

#### Actions creators

We test action creators as simple pure functions that just take an arguments and output proper arguments

#### Reducers

We test reducers as simple pure functions that just take an arguments and output proper arguments
Checks:

- valid default state
- changes of state when action is dispatched for different values of state

#### Cypress

1. Use `data-cy` as selector

---

### Generator

Command `npm run generate` is used to run [graphql code generator](https://graphql-code-generator.com)

1. before using codegen you must run backend server [SpaceToStudy backend](https://github.com/ita-social-projects/Space2Study-BackEnd-mvp)

2. open terminal

3. run `npm run generate`

4. you should run `npm run generate` every time new unions or interfaces are created

---

## Project Deploy

#### Deploy Сlient part: https://s2s-front-stage.azurewebsites.net/

---

## Contributing

You're encouraged to contribute to our project if you've found any issues or missing functionality that you would want to see. Here you can see [the list of issues](https://github.com/ita-social-projects/Space2Study-BackEnd-mvp/issues) and here you can create [a new issue](https://github.com/ita-social-projects/Space2Study-BackEnd-mvp/issues/new/choose).

Before sending any pull request, please discuss requirements/changes to be implemented using an existing issue or by creating a new one. All pull requests should be done into `develop` branch.

There are three GitHub projects: [SpaceToStudy-Client](https://github.com/ita-social-projects/Space2Study-Client-mvp) for frontend part, [SpaceToStudy-BackEnd](https://github.com/ita-social-projects/Space2Study-BackEnd-mvp/pulls) for backend part and admin part is currently under development. Every project has it's own issues.

Every pull request should be linked to an issue. So if you make changes on frontend, backend or admin parts you should create an issue with a link to corresponding requirement (story, task or epic).

All Pull Requests should start from prefix _#xxx-yyy_ where _xxx_ - task number and and _yyy_ - short description
e.g. #020-createAdminPanel

---

### Git flow

We have **main** , **develop** and **feature** branches.  
All **feature** branches must be merged into [develop](https://github.com/ita-social-projects/Space2Study-BackEnd-mvp/tree/develop) branch!!!
Only the release should merge into the main branch!!!

![Github flow](<https://wac-cdn.atlassian.com/dam/jcr:b5259cce-6245-49f2-b89b-9871f9ee3fa4/03%20(2).svg?cdnVersion=1312>)

#### Step 1

- **Option 1**

  - 👯 Clone this repo to your local machine using `https://github.com/ita-social-projects/Space2Study-BackEnd-mvp.git`

- **Option 2**

  - create new branch from development branch

#### Step 2

- add some commits to your new branch

#### Step 3

- 🔃 Create a new pull request using <a href="https://github.com/ita-social-projects/Space2Study-BackEnd-mvp/compare/" target="_blank">github.com/ita-social-projects/Space2Study-BackEnd-mvp</a>.

---

### Issue flow

#### Step 1

[![@KhrystynaPavlikovska](https://avatars.githubusercontent.com/u/34419998?s=400&u=15346304d164fb346cc2671a7d33052d2a6324e2&v=4)](https://github.com/KhrystynaPavlikovska)
[![@Roman-Peretiatko](https://avatars.githubusercontent.com/u/79856961?v=4)](https://github.com/Roman-Peretiatko)
[![@mxrcury](https://user-images.githubusercontent.com/34419998/222748150-75fae7f1-e219-48f6-a225-8f91f9cbbbd3.png)](https://github.com/mxrcury)
[![@tsivkadi](https://user-images.githubusercontent.com/34419998/222748492-37a29d91-8acc-4319-8402-52ec5fbaa57a.png)](https://github.com/tsivkadi)
[![@FryDay969](https://avatars.githubusercontent.com/u/39992977?v=4)](https://github.com/FryDay969)


### DevOps team

[![@abohatyrov](https://avatars.githubusercontent.com/u/52012169?v=4)](https://github.com/abohatyrov)

---

## FAQ

- **How do I do _specifically_ so and so?**
  - No problem! Just do this.

---

## Support

---

#### License

- **[MIT license](http://opensource.org/licenses/mit-license.php)**
- Copyright 2023 © <a href="https://softserve.academy/" target="_blank"> SoftServe IT Academy</a>.

[MIT](https://choosealicense.com/licenses/mit/)

[![License](http://img.shields.io/:license-mit-blue.svg?style=flat-square)](http://badges.mit-license.org)
