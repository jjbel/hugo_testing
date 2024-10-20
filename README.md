# Hugo Testing

## Prerequisites

### vscode

Code editor

https://code.visualstudio.com/download#

### Powershell

command-line shell

https://github.com/PowerShell/PowerShell/releases/download/v7.4.5/PowerShell-7.4.5-win-x64.msi

### Git

Version control

https://github.com/git-for-windows/git/releases/download/v2.47.0.windows.1/Git-2.47.0-64-bit.exe

### NodeJS

Javascript runtime for building sites

https://nodejs.org/dist/v20.18.0/node-v20.18.0-x64.msi

<!-- ### Chocolatey

package manager for windows

```ps
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
``` -->

### Hugo

static site generator

```ps
winget install Hugo.Hugo.Extended
```

## Create the site with Hugo

```ps
hugo new site . --force
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo "theme = 'ananke'" >> hugo.toml
```

---

### NOTE

At any point, to view the website locally, run `hugo server`

```

"Commit the changes" means run with a descriptive message:

```

git add .
git commit -m "my creative descriptive message"

```

and "push" means run `git push`

---

Create a file `.gitignore` at the root of the repo, and add:

```

.DS_Store
Thumbs.db
public/
.bin/node_modules/
/node_modules/
src/node_modules/
/dist/
.hugo_build.lock
resources/\_gen/

````

Commit the changes.

## Customize the Homepage

add this to `hugo.toml`
```toml
[languages.en]
title = "Breathe Easy"
weight = 1
contentDir = "content/en"

```

Add the following to `content/en/_index.md`

```md
---
title: "BreatheEasy"

description: "Tomorrow should be breathable."
cascade:
  featured_image: "/images/bg.jpg"
---

Welcome to the website of BreatheEasy. Read more below.
````

Where `/images/bg.jpg` is the background image, add one.

Commit the changes.

## Deploy to Github Pages

In the browser, to your repo > Settings > Pages.
In source choose "Github Actions"

Create a file `.github/workflows/hugo.yaml`
Copy-paste this in it:

```yml
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
      HUGO_VERSION: 0.134.2
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
          TZ: America/Los_Angeles
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
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
        uses: actions/deploy-pages@v4
```

**Now commit and push.**

Wait for a few seconds for the site to build.
Then you can view the site at https://username.github.io/reponame

To check the status of the build, go to the Actions tab of your repo's github.
