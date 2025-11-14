## Introduction to CI/CD with GitHub Actions
```
Continuous Integration (CI) and Continuous Deployment (CD) are key DevOps practices that automate how code is built, tested, and released.
```
- `CI (Continuous Integration)`: Every time developers push code to GitHub, it’s automatically tested and built to catch errors early.

- `CD (Continuous Deployment)`: Once the code passes all tests, it can automatically be deployed to production or staging — ensuring faster and more reliable releases.

`GitHub Actions` is a built-in automation tool on GitHub that allows you to set up CI/CD workflows `using simple YAML files`. These workflows define what should happen when specific events occur, for example, running tests or deploying code when new changes are pushed.

`In essence`:
```
CI/CD with GitHub Actions streamlines the development process by automating testing and deployment, helping developers and students build, deliver, and maintain software efficiently and confidently.
```
## Key Concepts and Terminology:
```
Before proceed, let have a brief understanding of the key concepts and termilonogy used in with GitHub Action. 
```
- Workflow: A workflow is a configuration automation process made up one or more jobs. Workflows are defined by a Yaml file in your repository. Example is the workflow, build and deploy a Node.js application upon a Git push.

- Event: An event is simply a specific activity that trigger a workfalow. Event include push, pull request, issue creation, or even a scheduled time. 

- Job: A set of steps in a workflow that are executed on the same runner. JObs can run sequentially or in paralell. 

- step: A step is simply an individual task that can run commands within a job. steps can run scripts or actions. Exanple, a step in a job to install depemdencies i.e, npm install.

- Action: this is a standalone ocmmand combined with into steps to create a job. Actions can be written by an individual or by the GitHub community. 

- Runner: This a seerver that runs our workflow when they are triggered. It can be hosted by GitHub or self-hosted. Example: when GitHub host runner that uses Ubuntu. 

# Practical Implementation
```
Below is the steps and guide to set up a CI/CD workflow using GitHub Actions and i will be using snapshots whenever possible.
```
- Create a new repository on GitHub. 

1.repo

- clone to your local machine: 
  
  Here i used the "git clone https://github.com/ICO-Tech/intro-CI-CD.git" command to clone it to my local machine 

- create a simple Node.Js Application:

    - initialize a Node.js project
  2.npm-init

- create a simple server using Express.js to serve a static web page.
  
2.express

- Save 
```
// Example: index.js
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => {"\n     res.send('Hello World!');\n   "});

app.listen(port, () => {
  console.log(`App listening at http://localhost:${port}`);
});
``` 
into our project folder.

## Writing Our First GitHub Action Workflow

- create a '.github/workflows' directry in our repository. 

- Create a yml file .i.e, nodejs.yml
- Copy and paste this code into your yml file. 
```
# Example: .github/workflows/node.js.yml

# Name of the workflow
name: Node.js CI

# Specifies when the workflow should be triggered
on:
# Triggers the workflow on 'push' events to the 'main' branch
push:
    branches: [ main ]
# Also triggers the workflow on 'pull_request' events targeting the 'main' branch
pull_request:
    branches: [ main ]

# Defines the jobs that the workflow will execute
jobs:
# Job identifier, can be any name (here it's 'build')
build:
    # Specifies the type of virtual host environment (runner) to use
    runs-on: ubuntu-latest

    # Strategy for running the jobs - this section is useful for testing across multiple environments
    strategy:
    # A matrix build strategy to test against multiple versions of Node.js
    matrix:
        node-version: [14.x, 16.x]

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
    - # Checks-out your repository under $GITHUB_WORKSPACE, so the job can access it
    uses: actions/checkout@v2

    - # Sets up the specified version of Node.js
    name: Use Node.js ${{" matrix.node-version "}}
    uses: actions/setup-node@v1
    with:
        node-version: ${{" matrix.node-version "}}

    - # Installs node modules as specified in the project's package-lock.json
    run: npm ci

    - # This command will only run if a build script is defined in the package.json
    run: npm run build --if-present

    - # Runs tests as defined in the project's package.json
    run: npm test
```
 ## Explaining each commands

- name: Node.js CI
```
This is just the name of the workflow.
```
- on: push / pull_request
```
This tells GitHub when to run the workflow:
When you push code to the main branch
When someone opens a pull request into the main branch
```
- jobs: build
```
A workflow contains jobs.
Here we have one job named build.
```
- runs-on: ubuntu-latest
```
The job will run on a Linux machine provided by GitHub.
```
- strategy/matrix
```
This tells GitHub to test your project using two versions of Node.js: Node 14 and Node 16
```
- steps
```
Steps are the tasks GitHub will run in order:
```
  - `actions/checkout@v2` Downloads your code so GitHub can work with it.
  - `actions/setup-node` Installs the version of Node.js listed in the matrix.

  - `npm ci` Installs your project dependencies.

  - `npm run build --if-present` Builds your project (only if you have a build script).

  - `npm test` Runs your tests to make sure everything works.

## Testing and Deployment

- install Jest 
 Inside your project folder
 ```
 npm install --save-dev jest
 ```
 
 3.jest

- Add a test script in package.json
```
"test": "jest"
```
- Create a folder for your tests
In the project root:
```
mkdir __tests__
```
- Add your first test
  Create this file: 
  ```
  __tests__/app.test.js
  ```
  then Add
  ```
  test("Sample test", () => {
    expect(1 +1).toBe(2);
  });
  ```
- Run tests locally
```
npm test
```
If it works, you are ready for CI testing.

4.test

## Deployment to AWS (Easiest: Eleastic Beanstalk)

- Install and configure AWS
  
  5.aws-config

This allows Github to deloy securely. 