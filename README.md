# Jenkins CI/CD Pipeline

![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-red?style=for-the-badge&logo=jenkins)
![Node.js](https://img.shields.io/badge/Node.js-Build-green?style=for-the-badge&logo=node.js)
![React](https://img.shields.io/badge/React-16-blue?style=for-the-badge&logo=react)
![Docker](https://img.shields.io/badge/Docker-Environment-blue?style=for-the-badge&logo=docker)
![Shell Script](https://img.shields.io/badge/Shell-Scripts-black?style=for-the-badge&logo=gnubash)

CI/CD study project using Jenkins Pipeline to build, test and deliver a React application.

This repository demonstrates how to define a delivery flow as code using a `Jenkinsfile`, shell scripts and npm commands.

---

## Project goal

The goal of this project is to practice CI/CD fundamentals with Jenkins:

- Pipeline as code
- Automated dependency installation
- Automated test execution
- Production build generation
- Temporary delivery environment
- Manual approval step before finishing the delivery stage
- Process cleanup after validation

---

## Tech stack

- Jenkins
- Jenkins Pipeline
- Node.js
- npm
- React 16
- Shell Script
- Docker / container-based environment

---

## Pipeline flow

The `Jenkinsfile` defines the following stages:

```txt
Build -> Test -> Deploy for production
```

### Build

Installs application dependencies:

```bash
npm install
```

### Test

Runs the test script through `jenkins/scripts/test.sh`:

```bash
npm test
```

### Deploy for production

Runs the delivery script:

```bash
./jenkins/scripts/deliver.sh
```

This stage:

- builds the React application;
- starts the application locally;
- waits for manual validation through Jenkins input;
- stops the running process using `jenkins/scripts/kill.sh`.

---

## Jenkinsfile overview

```groovy
pipeline {
    agent any

    tools {
        nodejs 'nodeRecent'
    }

    environment {
        CI = 'true'
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'chmod +x ./jenkins/scripts/test.sh'
                sh './jenkins/scripts/test.sh'
            }
        }

        stage('Deploy for production') {
            steps {
                sh 'chmod -R +x ./jenkins/scripts'
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
```

---

## Project structure

```txt
.
├── Jenkinsfile
├── package.json
├── jenkins
│   └── scripts
│       ├── test.sh
│       ├── deliver.sh
│       └── kill.sh
└── src
```

---

## Application scripts

Available npm scripts:

```bash
npm start
npm run build
npm test
npm run eject
```

---

## Running locally

### Requirements

- Node.js
- npm

### Install dependencies

```bash
npm install
```

### Start the app

```bash
npm start
```

The application will be available at:

```txt
http://localhost:3000
```

### Run tests

```bash
npm test
```

### Build for production

```bash
npm run build
```

---

## Running with Jenkins

### Requirements

- Jenkins configured with a Node.js installation named `nodeRecent`
- Git plugin
- Pipeline plugin
- Permission to execute shell scripts

### Job configuration

Create a Jenkins Pipeline job using:

```txt
Pipeline script from SCM
```

Repository:

```txt
https://github.com/vinicius-ssantos/pipelines
```

Script path:

```txt
Jenkinsfile
```

---

## What this project demonstrates

- CI/CD fundamentals
- Jenkins Pipeline as code
- Build, test and delivery stages
- Shell automation inside pipelines
- Manual approval step in delivery flow
- React application build process
- Process management using PID files
- Practical integration between GitHub and Jenkins

---

## Next improvements

- Add a Dockerfile for the React application
- Add Docker Compose for local Jenkins execution
- Add branch-based deployment rules
- Add pipeline status badges
- Add artifact archiving for production builds
- Add static analysis and linting stage
- Add deployment to a real environment
