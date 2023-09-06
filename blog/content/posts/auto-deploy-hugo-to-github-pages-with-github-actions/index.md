---
title: "Auto deploy Hugo to GitHub Pages with GitHub Actions"
date: 2023-09-06T17:00:00Z
draft: false
image: cover.jpg
---

## Introduction

This post is sharing how to deploy HUGO to GitHub Pages as a personal site (`https://username.github.io`), and automate the process by GitHub Actions.

## Shall we start?
Assume we have a Hugo project on a local machine. We create a public GitHub repository using the free plan.

We push the files to the repository before start configuring GitHub Pages.

We go to "Settings" tab of the repository, find “Pages” in the menu. Under “Build and deployment”, change “Source” to “Github Actions”.

You may choose the latest GitHub Actions template for deploying Hugo by going to  “Actions” tab of the repository, then press:  
 “New workflow” → “Choose a workflow”  
 Look for “Pages” section → “Hugo”  
 This is subjected to change and you may find the texts are different.

If you prefer do it manually, create `.github/workflows/hugo.yaml` and paste the content from GitHub or Hugo documentation. The content should be like this:

```yaml{linenos=table}
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.115.4
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"          
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

In our case, the source file is in `new-site/` instead of the root directory. We have to modify the command in "Build with Hugo" step to this:

```yaml{linenos=table,linenostart=58,hl_lines=3}
run: |
    hugo \
    -s ./new-site \
    --gc \
    --minify \
    --baseURL "${{ steps.pages.outputs.base_url }}/"   
```

And also an update in "Upload artifact" step:
```yaml{linenos=table,linenostart=63,hl_lines=4}
- name: Upload artifact
  uses: actions/upload-pages-artifact@v1
  with:
    path: ./new-site/public
```

We commit the yaml file to the repository with the above updates. Github will queue the workflow once there is a push in the `main` branch.

To monitor the workflow, go to “Actions” tab of the repository. If the workflow is successful, you should see a tick in a green circle.

The site is live now, navigate to your site: `https://username.github.io` to check.

## Wrapping up

We have deployed the `main` branch of our HUGO site to GitHub Pages and setup GitHub Actions to continuously deploy that branch to the site. Now, you can focus on writing posts.

## References
- [Host on GitHub Pages | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
- [hugo | Hugo](https://gohugo.io/commands/hugo/)
