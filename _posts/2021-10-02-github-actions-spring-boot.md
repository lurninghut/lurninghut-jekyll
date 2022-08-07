---
layout: post
title:  "Github Actions: Building Spring boot application"
author: Gaurav
categories: [ GIT ]
image: assets/images/github-actions-spring-boot/header-1.png
---
Github now a days is not just used as a central Git repository by the organisations but also as a tool for orchestration of CI/CD pipelines. Github actions automate all the software development workflows and enable build, test and deployment right from Github. In the demo we will see how we can use Github actions for basic development tasks by automating them using some configuration files.

**Demo**

[<img src="/assets/images/github-actions-spring-boot/demo.png" height="400px" width="600px"/>](https://youtu.be/mc7k2zOcQsg)

**Resources**

Github account : This will host the repository for our sample application which is to be built.

**Sample spring boot application**

Create a project from start.spring.io website as follows:

Note that we chose a Gradle project with spring web as a dependency.

<img src="/assets/images/github-actions-spring-boot/Screenshot-1.png" height="400px" width="600px"/>

Open the downloaded project in Intellij IDE

<img src="/assets/images/github-actions-spring-boot/Screenshot-2.png" height="400px" width="600px"/>

Create html page under static directory in resources folder.

Run the application and go to the browser to load the html page as follows:

<img src="/assets/images/github-actions-spring-boot/Screenshot-3.png" height="400px" width="600px"/>

**Github Workflow Changes**

Create a repository in Github

<img src="/assets/images/github-actions-spring-boot/Screenshot-4.png" height="400px" width="600px"/>

Push the local code to the repository using the commands mentioned as part of repository creation.

<img src="/assets/images/github-actions-spring-boot/Screenshot-5.png" height="400px" width="600px"/>

Once we push the local changes we will see our repository has all the code:

<img src="/assets/images/github-actions-spring-boot/Screenshot-6.png" height="400px" width="600px"/>

In our local repository lets create a new directory as .github/workflows:

<img src="/assets/images/github-actions-spring-boot/Screenshot-7.png" height="400px" width="600px"/>

Create a new file called demo.yml inside .github/workflows folder with the content as follows:

<img src="/assets/images/github-actions-spring-boot/Screenshot-8.png" height="400px" width="600px"/>

The demo.yml file created above will be executed by Github actions to run our build code. The code as text is as follows:

<pre>
    name: Demo to setup a project with github actions
    
    on:
    push:
    branches:
    - main
    
    jobs:
    build:
    runs-on: ubuntu-latest
    
    steps:
    - name: checking out code
    uses: actions/checkout@v2
    
    - name: Setting up java env
    uses: actions/setup-java@v1
    with:
    java-version: 11
    
    - name: grant permissions
    run: chmod +x gradlew
    
    - name: build action
    run: ./gradlew build
</pre>

Once the file is created, add it to local git by committing the changes:

<img src="/assets/images/github-actions-spring-boot/Screenshot-9.png" height="400px" width="600px"/>

Once committed, just push the code to the Github repo:

<img src="/assets/images/github-actions-spring-boot/Screenshot-10.png" height="400px" width="600px"/>

Once pushed we can to the Github UI console and check the Actions tab:

<img src="/assets/images/github-actions-spring-boot/Screenshot-11.png" height="400px" width="600px"/>

All the above demonstrated how we can build any Spring boot app using Github Actions.



