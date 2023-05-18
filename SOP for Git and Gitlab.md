# Standard Operating Procedure for GitLab Best Practices

## Purpose

The purpose of this document is to provide guidelines and recommendations for using GitLab as a version control and collaboration platform for software development projects.

## Scope

This document applies to all developers, testers, and managers involved in software development projects that use GitLab as a repository hosting service.

## Definitions

- **Git**: A distributed version control system that tracks changes in source code and enables collaboration among developers.
- **GitLab**: A web-based platform that provides hosting and management services for Git repositories, as well as features such as issue tracking, code review, continuous integration, continuous delivery, and more.
- **GitLab Flow**: A workflow model that defines how to use Git branches and merge requests to manage software development projects on GitLab.
- **Feature branch**: A branch that is created from the main branch to work on a specific feature or task.
- **Merge request**: A request to merge changes from one branch into another branch on GitLab.
- **Continuous integration**: A practice of merging code changes frequently and running automated tests to ensure quality and reliability.
- **Continuous delivery**: A practice of delivering code changes to production or staging environments automatically or with minimal human intervention.

## Procedures

### 1. Use feature branches rather than direct commits on the main branch

Using feature branches is a simple way to develop and keep the source code clean. Feature branches allow developers to work on different features or tasks without affecting the main branch. Feature branches also facilitate code review and testing before merging into the main branch.

To create a feature branch, follow these steps:

- On GitLab, go to your project page and click on the **New branch** button.
- Enter a descriptive name for your feature branch, such as `feature/add-login-form` or `fix/bug-123`.
- Select the main branch as the source branch and click on **Create branch**.

To work on your feature branch locally, follow these steps:

- On your terminal, navigate to your local repository and run `git fetch` to update your remote references.
- Run `git checkout -b feature/add-login-form origin/feature/add-login-form` to create and switch to your feature branch locally. Replace `feature/add-login-form` with your actual feature branch name.
- Make your code changes and commit them with descriptive messages. For example, `git commit -m "Add login form UI"`.
- Push your changes to the remote feature branch. For example, `git push origin feature/add-login-form`.

### 2. Test all commits, not only ones on the main branch

Testing all commits ensures that the code quality and reliability are maintained throughout the development process. Testing all commits also helps to identify and fix bugs early, before they cause problems in production.

To test all commits, follow these steps:

- On GitLab, go to your project page and click on **CI/CD** > **Pipelines**.
- Click on the **New pipeline** button and select your feature branch as the source branch.
- Click on **Create pipeline** and wait for the pipeline to run.
- Check the pipeline status and review the test results. If there are any errors or failures, fix them and push your changes again.

Alternatively, you can configure GitLab to run pipelines automatically for every commit or merge request. To do this, follow these steps:

- On GitLab, go to your project page and click on **Settings** > **CI/CD** > **General pipelines**.
- Under **Pipeline triggers**, select **Run pipeline for merge requests** and/or **Run pipeline for pushes**.
- Click on **Save changes**.

### 3. Perform code reviews before merging into the main branch

Code reviews are an essential part of software development that improve code quality, readability, maintainability, and security. Code reviews also foster knowledge sharing, feedback, and collaboration among developers.

To perform code reviews on GitLab, follow these steps:

- On GitLab, go to your project page and click on **Merge requests** > **New merge request**.
- Select your feature branch as the source branch and the main branch as the target branch.
- Enter a descriptive title and description for your merge request. You can also assign reviewers, labels, milestones, etc.
- Click on **Compare branches and continue**.
- Review the changes in the diff view and add comments or suggestions as needed.
- Wait for the reviewers to approve or request changes on your merge request. You can also reply to their comments or resolve them if they are addressed.
- Once your merge request is approved and passes all tests, click on **Merge**.

### 4. Deployments are automatic based on branches or tags

Deployments are the process of delivering code changes to production or staging environments where they can be accessed by users or customers. Deployments should be automatic based on branches or tags to ensure consistency and efficiency.

To configure automatic deployments on GitLab, follow these steps:

- On GitLab, go to your project page and click on **Settings** > **CI/CD** > **Environments**.
- Click on **New environment** and enter a name and URL for your environment. For example, `staging` and `https://staging.example.com`.
- Click on **Create environment**.
- Repeat steps 2 and 3 for any other environments you want to create. For example, `production` and `https://example.com`.
- On your local repository, create a file named `.gitlab-ci.yml` in the root directory. This file defines the configuration for your continuous integration and delivery pipelines.
- In the `.gitlab-ci.yml` file, define stages for your pipeline. For example:

```yaml
stages:
  - build
  - test
  - deploy
```

- In the `.gitlab-ci.yml` file, define jobs for each stage. For example:

```yaml
build:
  stage: build
  script:
    - echo "Building..."
    - npm install
    - npm run build
  artifacts:
    paths:
      - dist/

test:
  stage: test
  script:
    - echo "Testing..."
    - npm run test

deploy_staging:
  stage: deploy
  script:
    - echo "Deploying to staging..."
    - scp -r dist/* user@staging:/var/www/html/
  environment:
    name: staging
  only:
    - main

deploy_production:
  stage: deploy
  script:
    - echo "Deploying to production..."
    - scp -r dist/* user@production:/var/www/html/
  environment:
    name: production
  only:
    - tags
```

The above example assumes that you have a Node.js application that uses npm as a package manager. You can modify it according to your application's requirements.

The `build` job runs the `npm install` and `npm run build` commands to install dependencies and create a production-ready version of your application in the `dist` folder.

The `test` job runs the `npm run test` command to run tests on your application.

The `deploy_staging` job copies the contents of the `dist` folder to the staging server using `scp`. It also sets the environment name to `staging`. It only runs when changes are pushed to the main branch.

The `deploy_production` job copies the contents of the `dist` folder to the production server using `scp`. It also sets the environment name to `production`. It only runs when a tag is created.

You can use other tools or methods for deploying your application depending on your preferences.

For more information on how to configure `.gitlab-ci.yml`, refer to [GitLab CI/CD configuration reference](https://docs.gitlab.com/ee/ci/yaml/).

### 5. Tags are set by the user, not by CI

Tags are labels that mark specific points in the history of a repository. Tags are useful for creating releases or snapshots of your application at different stages of development.

Tags should be set by the user manually rather than by CI automatically. This gives more control over when and how tags are created.

To create a tag manually on GitLab, follow these steps:

- On GitLab, go to your project page and click on **Repository** > **Tags**.
- Click on **New tag**.
- Enter a name for your tag. You can use semantic versioning (e.g., v1.0.0) or any other naming convention you prefer.
- Select a commit or branch as the source for your tag.
- Optionally, enter a message or release notes for your tag.
- Click on **Create tag**.

Alternatively, you can create a tag locally using git commands. For example:

```bash
# Create a tag named v1.0.0 pointing to the latest commit
git tag v1.0.0

# Create a tag named v1.0.0 pointing to a specific commit
git tag v1.0.0 <commit-sha>

# Push tags to remote repository
git push --tags
```

For more information on how to use tags with GitLab, refer to [Git Tagging Basics](https://docs.gitlab.com/ee/university/training/topics/git_tagging_basics.html).
