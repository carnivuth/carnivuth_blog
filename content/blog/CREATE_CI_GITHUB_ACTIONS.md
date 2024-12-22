---
title: Create continous integration pipelines with `github` actions
date: 2024-09-16
aliases: []
tags: []
draft: true
---

`Github` offers a CI service called `github actions`, the service runs workflows defined for the repo in a dedicated server called workers, workflows are defined in yaml format under the folder `.github/workflows/`.

each workflow must specify:

- which event triggers the workflow (*eg. push/commit/merge*)
- which operating system is required
- list of steps for the workflow

In order to create a CI pipeline with GitHub actions

- create a GitHub action yaml file in a new branch in the repository

```bash
git switch main
git branch github_actions
git switch github_actions
```

example for docker image build:

```yaml
name: build docker container
run-name: ${{ github.actor }} is creating the new docker release of the container
on:
  push:
    branches:
      - 'main'
    paths-ignore:
      - "notes/**"
      - ".github/**"

jobs:
  build_and_push_container:
    runs-on: ubuntu-latest
    steps:
      -
        name: Set up QEMU
        uses: docker/setup-qemu-action@v3
      -
        name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      -
        name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: Build and push
        uses: docker/build-push-action@v5
        with:
          push: true
          tags: <DOCKERHUB_USERNAME>/<DOCKER_IMAGE_NAME>:latest

```

- add secrets in GitHub secret project session

example for docker image build

```bash
cd <repo>
gh secret set DOCKERHUB_USERNAME
gh secret set DOCKERHUB_TOKEN
```

- push file in the branch and merge it in the new repository

```bash
git add .github/workflows/
git commit -m 'added github actions'
git push
pr_to_main_branch
```

- test with some commits in the main branch


