---
layout: post
title:  "Github Actions - Create Automatic Pull Request"
author: Gaurav
categories: [ Git]
image: assets/images/github-actions-create-automatic-pull-request/header-1.svg
---
While working with automatic workflows using Github actions, there can be scenarios where some changes are to be done in the repository during the execution of the build steps. 
A very common example is updating information of the user to a file 'Author' in the code base every time a commit is pushed. 
In this demo we will also try to create a PR automatically by creating a branch which will have one single commit to a file called 'Author' with the information of the user who pushed the recent commit to main branch.

### Demo

[![Demo](/assets/images/github-actions-create-automatic-pull-request/youtube-screenshot.png)](https://youtu.be/NfsrMix5Qvc)

We will proceed incrementally with the configuration file changes. Let's create a new workflow file under .github/workflows

![Screenshot 1](/assets/images/github-actions-create-automatic-pull-request/Screenshot-1.png)

Add name and an event which we care for during our workflow:

![Screenshot 2](/assets/images/github-actions-create-automatic-pull-request/Screenshot-2.png)

***actions/checkout@v2***

Add first step as part of the jobs where we will checkout the code from main branch:

![Screenshot 3](/assets/images/github-actions-create-automatic-pull-request/Screenshot-3.png)

***rlespinasse/git-commit-data-action@v1.x***

Then we will add another action step which will export details of the recent commit as environment variables like GIT_COMMIT_AUTHOR_NAME.

![Screenshot 4](/assets/images/github-actions-create-automatic-pull-request/Screenshot-4.png)

Then we will add details extracted from previous step to Author file in the repository:

![Screenshot 6](/assets/images/github-actions-create-automatic-pull-request/Screenshot-6.png)

***peter-evans/create-pull-request@v3***

And as part of final step we will use action which will create a new branch, commit the change added in the previous step to the Author file and create the pull request. The 'delete-branch' config will ensure that the branch will be deleted after pull request is closed.

![Screenshot 5](/assets/images/github-actions-create-automatic-pull-request/Screenshot-5.png)

When we finish with this workflow file, we will commit it to the repo and push the commit to Github main branch. The push will trigger the workflow in Github as follows:

![Screenshot 7](/assets/images/github-actions-create-automatic-pull-request/Screenshot-7.png)

And once the workflow is finished we can navigate to the pull request tab to see the created pull request:

![Screenshot 8](/assets/images/github-actions-create-automatic-pull-request/Screenshot-8.png)

The complete workflow file is as follows:

<details>
    <summary>Expand to see code!</summary>
    
    <pre>
    
        name: Demo to create automatic PR
        
        on:
        push:
        branches:
        - main
        
        jobs:
        updateAuthor:
        runs-on: ubuntu-latest
        steps:
        - name: checking out code
        uses: actions/checkout@v2
        
        - name: extract git commit data
        uses: rlespinasse/git-commit-data-action@v1.x
        
        - name: Update author information
        run: echo ${{ env.GIT_COMMIT_AUTHOR_NAME }} > AUTHOR
        
        - name: Raise PR
        uses: peter-evans/create-pull-request@v3
        with:
        branch: "auto-pr-branch"
        base: main
        title: "demo for auto pr"
        committer: ${{ env.GIT_COMMIT_AUTHOR_NAME }} <${{ env.GIT_COMMIT_AUTHOR_EMAIL }}">
        author: ${{ env.GIT_COMMIT_AUTHOR_NAME }} <${{ env.GIT_COMMIT_AUTHOR_EMAIL }}">
        body:
        This is to show automatic PR creation
        token: ${{ secrets.GITHUB_TOKEN }}
        delete-branch: true
    </pre>
</details>

So we saw how easy it is to create an automatic pull request using appropriate Github actions.


