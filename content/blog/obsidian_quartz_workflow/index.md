---
title: Combining obsidian github and quartz for the ultimate note taking experience
description: "my personal journay to the definitive obsidian experience"
date: 2024-09-17
draft: false
tags:
    - obsidian
    - automation
    - "github actions"
    - quartz
    - "github pages"
---

My experience in university has teach me an important life lesson:

> ### I need to write down notes!!

it doesn't matter what am i doing or the argument, the writing process is essential to me in order to engraving information and elaborate links and improving my knowledge. So after some time i started to feel the need of streamline the note taking process.

## WHAT HAVE I TRIED SO FAR?

My first note taking system was based on a Samsung tablet with pen, good for writing, especially maths related stuff but limited to the tablet only, yes i could export pdfs but they are a read only system and not so easy to manage when sharing to multiple devices.

After some time i realized that if one of my big concerns was to share notes across devices, writing them with a text based formatting language was a must, and since `markdown` is my go to only option for project related documentation i decided to take the step and migrate away from the samsung notes tablet experience.

So after some testing with logseq i decided to fully commit to the markdown note taking powerhouse, obsidian

## THE OBSIDIAN EXPERIENCE

I don't want to make a deep dive in obsidian capabilities, it's the most complete and customizable markdown note taking experience that i have seen so far so after some notes here is my personal, after some tinkering i reached a pretty productive workflow based on some plugins:

- [advanced tables](https://github.com/tgrosinger/advanced-tables-obsidian) to easily manage obsidian tables
- [clear unused images](https://github.com/ozntel/oz-clear-unused-images-obsidian) to avoid pushing unwanted images
- [execute code](https://github.com/twibiral/obsidian-execute-code) for code snippents
- [git](https://github.com/Vinzent03/obsidian-git) for multiple device sync and as a backup measure
- [mermaid tools](https://github.com/dartungar/obsidian-mermaid) for graphs
- [minimal theme settings](https://github.com/kepano/obsidian-minimal) for clean environment
- [templater](https://github.com/SilentVoid13/Templater) for setting default page properties

> thanks to all this awesome people that contributes to this plugins 🙏

Overtime i also built my personal [obsidian utility](https://github.com/carnivuth/scripts/blob/main/bin/obsidian_manage.sh) to automate some operations specific to my usecases

It's all perfect and dandy until i need to visualize my work from other devices, yes! github can sync content with other git capable devices but on android ones is a major pain to configure it, and when my laptop runs out of charge if i don't find a plug to charge it i can't write anything without need to manually backport modifications

## THE SOLUTION: AS ALWAYS WEB

So my second option was to generate a static website for mobile device access and after some research i found [quartz](https://quartz.jzhao.xyz/), an amazing tool to generate a feature rich website from an obsidian vault, some of the features includes:

- mindmaps
- obsidian link conversions
- style customization
- dark theme
- mermaid and mathjax rendering

Then i designed a workflow to generate the website deploying on github pages:

{{< mermaid >}}
sequenceDiagram
participant me
participant github_repo
participant github_pages
me ->> github_repo: push changes to obsidian vault
github_repo ->> github_pages: deploys new site version
{{< /mermaid >}}

The next step was obvious in my mind: **AUTOMATION** cause if something pisses me off is manually building software so i came up with a github action to automate the work for me

```yaml
---
name: Deploy Quartz site to GitHub Pages

on:
  push:
    branches:
      - main
    paths-ignore:
      - ".gitignore"
      - "readme.md"

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - name: setup node
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: clone repo
        uses: actions/checkout@v4
        with:
          fetch-depth: 0 # Fetch all history for git info

      - name: Clone quartz
        uses: GuillaumeFalourd/clone-github-repo-action@v2.3
        with:
          branch: 'v4'
          owner: 'jackyzha0'
          repository: 'quartz'

      - name: build
        run: >
          cd $HOME
          && git clone https://github.com/jackyzha0/quartz
          && cp $GITHUB_WORKSPACE/quartz.config.ts quartz
          && cp $GITHUB_WORKSPACE/quartz.layout.ts quartz
          && cd quartz
          && npm install
          && npx quartz create -s $GITHUB_WORKSPACE -X copy -l shortest
          && rm -rf content/quartz
          && npx quartz build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: /home/runner/quartz/public

  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

The action will:

- download vault content and quartz
- setup quartz configuration for the build
- run quartz to generate site
- push the `html` content to github pages

The github action relies in the fact that in the root of the repository is stored a copy of the quartz [configuration](sample_files/quartz.config.ts) and [layout](sample_files/quartz.layout.ts) file in order to build the website
