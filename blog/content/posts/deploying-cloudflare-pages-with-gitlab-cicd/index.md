---
title: "Deploying Cloudflare Pages with GitLab CI/CD"
date: 2025-01-07T10:13:00Z
draft: false
image: cover.jpg
---

## Introduction

Cloudflare Pages is a way to deploy static websites on a global scale. We would like to focus on preparing the content and do not want to direct upload those html files from the dashboard every time. We are going to discuss setting up an automation for deploying the site to save time.

## Implementation

There are two options to setup the automation. We are going to use GitLab as an example. You can choose either connecting Git and Cloudflare and running Wrangler in CI/CD.

### Connecting Git and Cloudflare

The most straightforward way to setup is connecting Cloudflare with our Git, which hosts our contents.

We log in to the Cloudflare dashboard and select "Worker & Pages". We select "Create application" > "Pages" > "Connect to Git" to connect our pages to GitLab. We select the repository to connect. We can define the project name of the deployment and the production branch. We may want to choose a specific build setting for the framework used. There are a lot of choices [here](https://developers.cloudflare.com/pages/framework-guides/deploy-anything/). If not, just continue. After the setup, we can select "Save and Deploy" to trigger the first build and deployment. We can monitor the whole process in the dashboard. An URL would be shown after a successful deployment for us.

In this way, our contents will be deployed after a successful push to GitLab. But that exposes all of our repositories to Cloudflare.

### Running Wrangler in CI/CD pipeline

If you prefer not exposing all of your repositories, you may want to try using Cloudflare API via wrangler to deploy automatically.

We log in to the Cloudflare dashboard and create API tokens for our account by using "Cloudflare Workers" template. We save that token privately. We go to GitLab CI/CD pipeline section and create 2 variables: `CLOUDFLARE_ACCOUNT_ID` and `CLOUDFLARE_API_TOKEN`, which are the account id and the token just generated. Those variables should be protected and masked to ensure not showing in any logs.

We create `.gitlab-ci.yaml` in the root folder of the repository with content like this:
```yaml
publish:
  image: node:latest
  stage: deploy
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
  script:
    - npm install wrangler --location=global
    - wrangler pages deploy ./dist --project-name "YOUR_PROJECT_NAME" --branch "main"
```
We may want to specify the version later to have a better management of the site. We use `latest` tag now for simplicity. We also use `main` branch as the production branch, which can be changed to fit your need.

After committing and pushing to the remote, we can see that the pipeline would try to deploy everything under `dist` folder to the Cloudflare Pages. The end of the job output should be like this:
```
$ wrangler pages deploy ./dist --project-name "YOUR_PROJECT_NAME" --branch "main"
Uploading... (224/345)
Uploading... (265/345)
Uploading... (305/345)
Uploading... (345/345)
✨ Success! Uploaded 121 files (224 already uploaded) (1.91 sec)
🌎 Deploying...
✨ Deployment complete! Take a peek over at https://<SOME_HASH>.<YOUR_PROJECT_NAME>.pages.dev
Cleaning up project directory and file based variables 00:01
Job succeeded
``` 

We can check the site from the link provided from the log. We can also check the deployment status in the dashboard. And that is it.

For GitHub, the concept is the same. We can setup the automation in GitHub Actions to deploy the same content.

## Wrapping up

We have walked through setting up an automation for deploying static websites. You have understood different options and their benefits as well. You may want to check the latest documentation while you are implementing. As the services are advancing, some settings and output may vary.


## References

- https://developers.cloudflare.com/pages/framework-guides/deploy-anything/
- https://developers.cloudflare.com/workers/ci-cd/external-cicd/gitlab-pipelines/
- https://developers.cloudflare.com/pages/get-started/git-integration/
- https://about.gitlab.com/blog/2022/11/21/deploy-remix-with-gitlab-and-cloudflare/
- https://medium.com/@mountainash8/gitlab-ci-deploy-to-cloudflare-pages-6c7ecb8f59fe

## Credits

Cover photo by [The Chaffins](https://unsplash.com/@thechaffins?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-view-of-a-mountain-covered-in-clouds-Vd_s71-WVcA?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 
