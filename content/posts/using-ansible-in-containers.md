---
author:
  name: "Josh Duffney"
date: 2020-08-11
lastmod: 2020-08-11
linktitle: Containers for Ansible Development 
toc: true
type: post
type: posts
title: Containers for Ansible Development 
tags: ["ansible","containers","devops","github-actions"]
aliases:
 - "/using-ansible-in-containers"
categories: ["how-to"]
---

## Introduction

Containers aren't just for developers. But without a web application what do you do? Don't you need five-plus years of web development experience to make sense of containers? No, you don't. In fact, once you dive in and start learning you'll realize containers have more to do with operations than development. It's just not immediately obvious what sysadmins can use containers for.

You can't look at more than two job postings without seeing containers or Kubernetes listed at least five times. When something becomes as popular as containers you have two choices. You can either ignore it or learn it. In this post, you'll dive into learning it.

Ansible is an open-source configuration management and application-deployment tool. Ansible enables you to define your infrastructure as code, and runs equally well on Linux and Windows. Ansible is ideal for containers, which can also run on Linux and Windows systems. 

What makes Ansible ideal is it's agentless. Instead of requiring a software agent to be installed and configured before it can be useful (as some Ansible alternatives do), Ansible uses native, remote protocols (like SSH and [WinRM](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html)) to connect to managed nodes. This means it's easily portable, and portability is what containers are all about.

In this post, you will learn how to:

1. Build a Dockerfile defining your Ansible environment
2. Run Ansible in a container interactively and non-interactively
3. Version your Ansible environment with tags
4. To deploy Ansible using GitHub actions.

By the end of this post, you will have learned how to build, run, and deploy a production-ready container.

**_Watch the PSPowerHour Episode 7 - Ansible and containers [livestream](https://www.youtube.com/watch?v=nGHK_Nh8H_I&feature=youtu.be)_**

## Prerequisites

To complete this tutorial, you will need:

- Docker Desktop installed on your local machine. You can install Docker Desktop for Mac or Windows by following the steps on the [Docker Desktop](https://www.docker.com/products/docker-desktop) website.
- GitHub account (either existing or newly created). You can create an account by going to [github.com/join](https://github.com/join)

_Example repository, [ansible-in-containers](https://github.com/Duffney/ansible-in-containers)._

## Build an Ansible Container

Docker has become the most common style of container, because it's so easy to work with. It is built using a Dockerfile, which is just a special text document that contains a sequence of commands. Those commands represent codified instructions that make up a Docker (container) image. You can think of the Docker image as a virtual machine template. Just as virtual machines are created from a base template, Docker containers are created from Docker images. Before you can run a container you must first have an image. In order to create an image, you need a Dockerfile.

### Create a Dockerfile

Within your editor or terminal of choice, create a file named `Dockerfile`. 

```bash
touch Dockerfile
```

```powershell
New-Item Dockerfile
```

You normally don't write or install an operating system by hand on a virtual machine and you don't do that with containers either. A base image is equivalent to a virtual machine operating system. You specify the base image with the `From` command.

On the first line type `FROM`.

`FROM centos:centos7.7.1908`

Before installing Ansible update the container and install Ansible's dependencies. Python 3 and pip3 are needed to install Ansible using pip. There are other ways of installing Ansible, but this is the simplest.

Add the `RUN` command to the Dockerfile. Run all the required yum and pip3 commands to install Ansible.

```bash
RUN yum check-update; \
  yum install -y gcc libffi-devel python3 epel-release; \
  pip3 install --upgrade pip; \
  pip3 install ansible
```

Notice that all these commands are under a single `RUN` command. The reason is, each `RUN` command creates a layer for the container. It's much more efficient and best practice to use as few `RUN` commands as possible.

*Dockerfile*

```docker
FROM centos:centos7.7.1908

RUN yum check-update; \
  yum install -y gcc libffi-devel python3 epel-release; \
  yum install -y openssh-clients; \
  pip3 install --upgrade pip; \
  pip3 install ansible
```

### Build a Docker Image

Docker’s build command is used to build an image from a Dockerfile. You will use this image for running your Ansible containers.

```bash
docker build . -t ansible-in-containers:latest
```

The `-t` option is short for `--tag`. A tag provides a name for the image and optionally a tag in the `name:tag` format. They are also used as an alias for the ID of the image.

Your image is tagged `ansible-in-containers:latest` which is made of two parts; image name and tag name. The image name is `ansible-in-containers` with the tag name of `latest`. Tag names are most often used for versioning. The `latest` tag is a special tag that allows you to run the container without specifying the tag name portion of the tag. 

### View Docker Images

Having created the docker image, issue the `docker images` command to view the image.

```bash
docker images
```

Within the output locate your newly created image. The image name is in the **REPOSITORY** column and the tag name is in the **TAG** column. 

### Run Interactively

The command to run Docker containers is `docker run`.  By default containers run detached. You, however, need to run the container interactively. You accomplish that by adding the `-it` option to the docker run command.

Issue the docker run command to start the Ansible container.

```bash
docker run -it ansible-in-containers
```

Immediately your terminal enters the Ansible container. From here you are able to interact with the container as if you used ssh to connect to a Linux virtual machine. Run the `ansible --version` command and exit the container.

```bash
ansible --version
exit
```

Get a list of all the running and stopped docker containers with `docker ps`.

```bash
#List all running containers
docker ps

#List all containers
docker ps --all
```

Notice that the first command doesn't output your ansible container. The reason is when you exited the container it stopped it. Without a running process, Docker will stop the container.  The second command displays your Ansible container in the output. 

You could start it back up and reconnect to it with an interactive terminal. However, that negates one of the biggest benefits of containers, their immutability. If you issue the docker run command enough, eventually you will have a lot list of stop containers. Who's only purpose is to take up space.

### Remove on Exit

You don't have a need for stopped Ansible containers. It's better to have the container remove on exit instead. The docker run option that allows you to do that is `--rm`.

Issue the docker run command with the remove on exit option.

```bash
docker run -it --rm ansible-in-containers
```

Inside the container run the `ansible --version` command, then exit the container. After you exit, run the `docker ps` command to list all stopped and running containers.

```bash
docker ps -all
```

Notice that the Ansible container is not listed. Using `--rm` will cleanup your containers for you.

### Bind Mount Volumes

Using Ansible inside a container has made it lightweight and disposable, but it hasn't improved your development experience. Without a way to share files between your local machine and the container, you're stuck using vi or nano to create and modify files. That's a tough sell for most. 

Your solution is to use volumes. By using bind mounts volumes you can share a directory with the container. Changes made to the files in the volume will update in real-time within the container. This means you can modify Ansible playbooks with all the comfort of your IDE of choice while having the isolated disposable environment only a container can provide.

The docker run option to bind mount a volume is `-v` followed by `/source:/target`. The source is the file path existing on your local machine. Target is the path you wish to mount the directory inside the container.

Start an Ansible container and mount the current directory to `/ansible` inside the container.

```bash
#LINUX
docker run -it --rm --volume "$(pwd)":/ansible ansible-in-containers
```

```bash
#WINDOWS
docker run --volume ${PWD}:/ansible -w /ansible ansible-in-containers
```

Once inside the interactive terminal change to the `/ansible` directory. Did that make you twitch? Yeah, me too. Let's fix that.

### Specify a Working Directory

Having to change directories is a minor inconvenience. With an easy solution, the docker run option `--workdir` or the shorthand `-w`. This docker run option specifies the working directory inside the container. Preventing you from having to change directories.

Add the `-w` option to the docker run command.

```bash
#LINUX
docker run -it --rm --volume "$(pwd)":/ansible -w /ansible ansible-in-containers
```

```bash
#WINDOWS
docker run -it --rm --volume ${PWD}:/ansible -w /ansible ansible-in-containers
```

Notice that once the container starts, the working directory is set to `/ansible`.

### Add Environment Variables

Environment variables control everything from attempting retries to connect to a cloud provider. Some you'll want to statically assign, while others are dynamic and best passed in at run-time.

### Environment Variables in the Dockerfile

Ansible configuration that is static is best set via environment variables in the Dockerfile. Setting the environment variable there ensures it will be populated when the container starts. It also prevents you from having to define it at run-time or manually afterward. Examples of static configuration values are; host key checking, Ansible fact-gathering, and retry files.

Open your `Dockerfile` and create environment variables for; ANSIBLE_HOST_KEY_CHECKING, ANSIBLE_GATHERING, and ANSIBLE_RETRY_FILES_ENABLED.

```docker
FROM centos:centos7.7.1908

ENV ANSIBLE_HOST_KEY_CHECKING false
ENV ANSIBLE_GATHERING smart
ENV ANSIBLE_RETRY_FILES_ENABLED false

RUN yum check-update; \
  yum install -y gcc libffi-devel python3 epel-release; \
  yum install -y openssh-clients; \
  yum install -y sshpass; \
  pip3 install --upgrade pip; \
  pip3 install ansible
```

Rebuild the image, overwrite the latest tag.

```bash
docker build . -t ansible-in-containers:latest
```

Execute the docker run command, use `printenv` to display  the environment variables.

```bash
docker run --rm ansible-in-containers printenv
```

### Environment Variables with docker run

Ansible configurations that are dynamic are best passed in at run-time. Connecting to a cloud provider is a good example of a dynamic configuration. Locally you want to connect to a development subscription or account. But, within a release pipeline, you want to target a production subscription or account. The docker run command gives you that flexibility with the `--env` option, `-e` for shorthand.

Use the `--env` option with the docker run command, populate AWS or Azure connection variables.

```bash
#LINUX

##AWS
docker run -it --rm --volume "$(pwd)":/ansible --workdir /ansible \
--env "AWS_ACCESS_KEY_ID='<AWS_Access_Key_ID>'" \
--env "AWS_SECRET_ACCESS_KEY='<AWS_Secret_Access_Key>'" \
ansible-in-containers

##AZURE
docker run -it --rm --volume "$(pwd)":/ansible --workdir /ansible \
--env "AZURE_SUBSCRIPTION_ID=<Azure_Subscription_ID>" \
--env "AZURE_CLIENT_ID=<Service_Principal_Application_ID>" \
--env "AZURE_SECRET=<Service_Principal_Password>" \
--env "AZURE_TENANT=<Azure_Tenant>" \
ansible-in-containers
```

```powershell
#WINDOWS

##AWS
docker run -it --rm --volume "$(pwd)":/ansible --workdir /ansible `
--env "AWS_ACCESS_KEY_ID='<AWS_Access_Key_ID>'" `
--env "AWS_SECRET_ACCESS_KEY='<AWS_Secret_Access_Key>'" `
ansible-in-containers

##AZURE
docker run -it --rm --volume "$(pwd)":/ansible --workdir /ansible `
--env "AZURE_SUBSCRIPTION_ID=<Azure_Subscription_ID>" `
--env "AZURE_CLIENT_ID=<Service_Principal_Application_ID>" `
--env "AZURE_SECRET=<Service_Principal_Password>" `
--env "AZURE_TENANT=<Azure_Tenant>" `
ansible-in-containers
```

## Run Non-interactively with ENTRYPOINTS

There's an alternative to the interactive terminal, an entry point. Entry points provide a default executable for the container. When working with Ansible that executable is either the command `ansible` for ad-hoc commands or `ansible-playbook` to execute automation in playbooks. The benefit of an entry point is the ability to run the container non-interactively.

### ansible-playbook as an EntryPoint

Using the `ansible-playbook` command as an entry point execute the playbook provided when the container starts. If you find yourself starting an Ansible container to just run `ansible-playbook` over and over, this is a good option.

Open the `Dockerfile`, add an ENTRYPOINT command to execute `ansible-playbook`.

```docker
ENTRYPOINT ["ansible-playbook","playbook.yml"]
```

Rebuild the image, overwrite the latest tag.

```bash
docker build . -t ansible-in-containers:latest
```

Create the `playbook.yml` playbook.

```yaml
---
  - hosts: localhost
    gather_facts: false
    connection: local

    tasks:
      - name: ping localhost
        ping:
```

Run the Ansible container non-interactively.

```powershell
#LINUX
docker run --rm --volume "$(pwd)":/ansible -w /ansible ansible-in-containers

#WINDOWS
docker run --rm --volume ${PWD}:/ansible -w /ansible ansible-in-containers
```

Hard-coding the playbook in the ENTRYPOINT is only a default. It can be overwritten. Create another playbook called `createdirectory.yml`.

```yaml
---
  - hosts: localhost
    gather_facts: false
    connection: local

    tasks:
      - name: create ansible directory
        file:
          path: /ansible
          state: directory
```

Run the `createdirectory.yml` non-interactively by passing it as an argument to docker run.

```yaml
#LINUX
docker run --rm --volume "$(pwd)":/ansible -w /ansible ansible-in-containers createdirectory.yml

#WINDOWS
docker run --rm --volume ${PWD}:/ansible -w /ansible ansible-in-containers createdirectory.yml
```

### Shell script as an EntryPoint

Depending on your Ansible setup, you might need to pull down Ansible galaxy roles or create a vault file before you can run the `ansible-playbook`command. If that's the case, you can still use entry points. Switching out the `ansible-playbook` command with a bash script is the simplest option.

Create the entry point shell script, `entrypoint.sh`. 

```bash
#!/bin/bash

echo $ANSIBLE_VAULT_PASSWORD >> .vault

ansible-playbook $1 --vault-password-file .vault

rm .vault
```

The `entrypoint.sh` uses the echo command to output an environment variable to a file that stores the Ansible vault secret. Afterward, it runs the `ansible-playbook`command. Using `$1` as the playbook name allows you to pass the playbook in as an argument. Adding `--vault-password-file` provides the Ansible vault password via a file instead of an interactive prompt. Lastly, it removes `.vault`file. Without that the file is store on your local machine because of the bind mount volume.

Run the Ansible container with the shell entry point. Add the environment variable `ANSIBLE_VAULT_PASSWORD` to the docker run command.

```bash
#LINUX
docker run --rm --volume "$(pwd)":/ansible -w /ansible -e "ANSIBLE_VAULT_PASSWORD=P@ssw0rd" ansible-in-containers createdirectory.yml

#WINDOWS
docker run --rm --volume ${PWD}:/ansible -w /ansible -e "ANSIBLE_VAULT_PASSWORD=P@ssw0rd" ansible-in-containers playbook.yml
```

Increase the flexibility for the entry point. Allow the Ansible container to run with or without an Ansible vault password and add the inventory argument.

Update the `entrypoint.sh` script. 

```bash
#!/bin/bash

if [ ! -z "$ANSIBLE_VAULT_PASSWORD" ]
then
      echo $ANSIBLE_VAULT_PASSWORD >> .vault;
      ansible-playbook $1 -i $2 --vault-password-file .vault;
      rm .vault
else
      ansible-playbook $1 -i $2
fi
```

Rebuild the image, overwrite the latest tag.

```bash
docker build . -t ansible-in-containers:latest
```

Run the n

```yaml
#LINUX
docker run --rm --volume "$(pwd)":/ansible -w /ansible ansible-in-containers playbook.yml localhost

#WINDOWS
docker run --rm --volume ${PWD}:/ansible -w /ansible ansible-in-containers playbook.yml localhost
```

Your options are only limited by your bash skills and creativity when you use a shell script as an entry point. 

### Override an EntryPoint

You're probably not going to want two versions of the Dockerfile. One with an entry point and one without. And you don't have too. To use the Ansible container with an entry point interactively you simply override it.

Run the Ansible container with an entry point interactively.

```bash
docker run --entrypoint bash -it --rm --volume ${PWD}:/ansible -w /ansible  ansible-in-containers
```

Adding the `--entrypoint` option along with the `-it` option will start the container in an interactive bash session as it did before the entry point was added. Now you can alternate between which modality best suits you.

---
**NOTE**

Entry points are what allow you to run Ansible containers in a release pipeline. Adding them to your Dockerfile makes them easy to use with CI CD systems like GitHub actions and workflows.

---


## Version with Tags

Over time you will make changes to your Ansible environment. When that happens you don't need to "roll forward" or take snapshots. Instead, version your Ansible container using tags. As an example switch the Ansible container image to Alpine instead of CentOS. 

Tag the CentOS image with `v1.0`

```bash
docker build . -t ansible-in-containers:v1.0
```

Create a directory named `alpine-ansible`, change to that directory.

```bash
#BASH
mkdir alpine-ansible; cd alpine-ansible

#POWERSHELL
New-Item -Type Directory alpine-ansible | % { cd $_.name}
```

Copy `entrypoint.sh` to the alpine-ansible directory

```bash
#BASH
Copy ..\entrypoint.sh .

#POWERSHELL
Copy-Item ..\entrypoint.sh .
```

Create the `Dockerfile` for the alpine container.

Dockerfile

```docker
FROM alpine:3.11

ENV ANSIBLE_VERSION=2.9.11
ENV ANSIBLE_LINT_VERSION=4.2.0

RUN apk update \
  && apk add --no-cache --progress python3 openssl \
  ca-certificates git openssh sshpass \
  && apk --update add --virtual build-dependencies \
  python3-dev libffi-dev openssl-dev build-base bash \
  && rm -rf /var/cache/apk/* 

RUN pip3 install --upgrade pip \
  && pip3 install ansible==${ANSIBLE_VERSION} \
  && pip3 install ansible-lint==${ANSIBLE_LINT_VERSION}

COPY ./entrypoint.sh /entrypoint.sh

ENTRYPOINT ["bash","/entrypoint.sh"]
```

Build the image, tag it with `v2.0`.

```bash
docker build . -t ansible-in-containers:v2.0
```

Run the container using either tag.

```bash
#CentOS
docker run -it --rm ansible-in-containers:v1.0 playbook.yml localhost

#Alpine
docker run -it --rm ansible-in-containers:v2.0 playbook.yml localhost
```
---

**NOTE**

The `ansible-in-containers` image equals `ansible-in-containers:latest`. Latest was built using the CentOS image and would also launch the CentOS Ansible container not the Alpine.

---

## Push to Share with DockerHub

All the images you've created so far only exist locally on your machine. Which means you can't use it anywhere else. You can't share it with your team and you can't use it in a release pipeline. Container registries store your container images, allowing them to be pulled down elsewhere. DockerHub is a free public container registry. In order to push container images to DockerHub, you have to first have an account. 

**Create a DockerHub Account**

1. Go to the DockerHub signup page.
2. Enter a username. ( used as Docker ID )
3. Enter a unique and valid email.
4. Enter a password
5. Click **Sign up**
6. Click the link within the verification email

Sign Into DockerHub, replace `<DockerHub-UserName>` with your username, and follow the prompts to log into DockerHub.

```bash
docker login --username <DockerHub-UserName>
```

Update the Ansible image tags to include your DockerHub Username.

```bash
docker build . -t <DockerHub-UserName>/ansible-in-containers:latest
```

Push the Ansible container image to DockerHub.

```bash
docker push <DockerHub-UserName>/ansible-in-containers:latest
```

Once the upload is complete, you can pull the image from anywhere.

```bash
docker pull <DockerHub-UserName>/ansible-in-containers:latest 
```

## Deploy Ansible with GitHub Actions

"*Automate, customize, and execute your software development workflows right in your repository with GitHub Actions.*" Says the introduction on the GitHub Actions documentation page. How does this relate to Ansible? Ansible is code and because of that, you can build a workflow that deploys it based on Git events. Such as a push or pull request to the repository.

### Create an Ansible Action

GitHub Actions are individual tasks that you combine to create jobs and customize workflows. Using a [Docker container Action](https://docs.github.com/en/actions/creating-actions/about-actions#docker-container-actions), you will create your own to run Ansible whenever you push or merge a pull request.

Create the Ansible Action directory.

```bash
mkdir .github/actions/ansible
```

Create a new `Dockerfile`.

```yaml
FROM  <DockerHub-UserName>/ansible-in-containers

COPY ./entrypoint.sh /entrypoint.sh

ENTRYPOINT ["bash","/entrypoint.sh"]
```

Copy the `entrypoint.sh` script to the Ansible Action directory.

```yaml
#BASH
Copy entrypoint.sh .github/actions/ansible

#POWERSHELL
Copy-Item entrypoint.sh .github/actions/ansible
```

Create the `action.yml` in the new directory.

```yaml
name: 'Ansible'
description: 'Runs an Ansible playbook'
inputs:
  playbook:
    description: 'Ansible playbook to run'
    required: true
    default: playbook.yml
  inventory:
    description: 'Ansible inventory to use'
    required: true
    default: localhost
runs:
  using: 'docker'
  image: 'Dockerfile'
  args:
    - ${{ inputs.playbook }}
    - ${{ inputs.inventory }}
```

Name and description give context to what the action is and what it does. Inputs define the parameters used by the Docker container Action. The playbook and inventory are required inputs for this action because without them the entry point arguments would be empty and the `ansible-command` would fail to run. Each of the inputs also has a default value.

Within the runs section where the magic happens. Setting using to docker is what specifies the Action type as a Docker container Action. Setting the image to Dockerfile instructs the Action to use the Dockerfile local to the Action's directory to build the container used by the Action. Args then are passed in after the image is built and the Action runs the container.

### Create the Workflow

Workflows are custom automated processes that allow you to orchestrate your build, test, and release. Using a workflow you will lint all your Ansible files, then run the Ansible Docker container Action.

Create the `workflow` directory

```yaml
mkdir .github/workflows
```

Create the `deploy_ansible.yml` workflow file.

```yaml
name: deploy ansible

on:
  push:
    branches:
    - master
  pull_request:
    branches:
    - master

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: checkout repo
      uses: actions/checkout@v2
    - name: Lint Ansible Playbook
      uses: ansible/ansible-lint-action@master
      with:
        targets: ""
  deployAnsible:
    needs: build
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - uses: ./.github/actions/ansible
      with: 
        playbook: playbook.yml
        inventory: localhost
```

The above will create a GitHub workflow named **deploy Ansible.** It will only be triggered when there is a push or pull request made to the master branch of the repository. Once triggered the workflow will kick-off two jobs. First, the `build` job runs. Build runs a public GitHub Action called ansible-lint which runs the ansible-lint command-line utility against all .yml or.yaml files in your repository. If ansible-lint is successful the second job is triggered. Running the Ansible Docker container action `deployAnsible` runs using the with values provided as arguments to the container. The container then runs the `ansible-playbook` command with the arguments specified in the `with` list.

Only one thing remains, push your changes. GitHub will detect the files within the .github directory and create the Actions and Workflow based on the .yml documents in the corresponding directories. Give it a minute or two, then your repository on [GitHub.com](http://github.com), and review the action's results under the Actions tab.

![github-action-results](/img/posts/using-ansible-in-containers/github-action-results.png "github-action-results")

# Conclusion

You've now learned how to build, run, and deploy Ansible inside a Docker container. Containers are no longer something only developers use to run applications. It's also how sysadmins, DevOps engineers, and SREs deploy infrastructure as code. Using containers yourself you'll see the benefits first hand. Containers provide a consistent development experience for you, your team, and release pipelines, versioned images of your Ansible environment, increased portability, and flexibility. Containers are the future of infrastructure, not just web apps. Don't wait until you need to learn about them, start today, start now.

---
<p align="center">

<img src="/img/posts/using-ansible-in-containers/become-ansible.png">
<script src="https://gumroad.com/js/gumroad.js"></script>
<br></br>
<a class="gumroad-button" href="https://gum.co/become-ansible" target="_blank">Buy the book</a>
<br></br>
</p>
