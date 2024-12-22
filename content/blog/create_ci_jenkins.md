---
title: Configure jenkins ci with github repos
date: 2024-09-16
id: CREATE_CI_JENKINS
aliases: []
tags: []
index: 13
draft: true
---

Jenkins is a CI service that can build and test software from different VCS, in order to create a new pipeline to build and compile software from github repos, in this setup github will trigger with a webhook the jenkins instance to run a build defined in a Jenkinsfile inside the repo, events that triggers the CI pipeline can be specified in the github repo config section

- Create a new pipeline on Jenkins and add a GitHub repository url

![](/images/Pasted%20image%2020240416132959.png)

- Set the CI script to pull from SCM

![](/images/Pasted%20image%2020240416133043.png)

- Create a `Jenkinsfile` with the Jenkins CI pipeline (here example for building docker images)

```groovy
pipeline {
	environment {
		registry = "carnivuth/<project_name>"
		registryCredential = 'dockerhub_id'
		dockerImage = ''
	}

	agent any
	stages {
		stage('Cloning Repository') {
			steps {
				git branch:'main',
				    url:'https://github.com/carnivuth/<project_name>'
			}
		}

		stage('Building <project_name> docker image') {
			steps {
				script {
					dockerImage = docker.build registry + ":$BUILD_NUMBER"
				}
			}
		}

		stage('Upload docker image to docker hub') {
			steps {
				script {
					docker.withRegistry('', registryCredential) {
						dockerImage.push()
					}
				}
			}
		}

		stage('Cleaning up environment') {
			steps {
				sh "docker rmi $registry:$BUILD_NUMBER"
			}
		}
	}
}
```

- Configure Jenkins to add GitHub hooks automatically to the repo


