---
author:
  name: "Josh Duffney"
date: 2020-08-04
linktitle: asciidoc
toc: true
type: post
type: posts
title: asciidoc
tags: ["asciidoc"]
categories: ["how-to"]
---

= Heading 1

[subs="normal,specialcharacters"]
----
*Objective*
Run Ansible within a Docker Container

*Steps*
1. Install Docker Desktop
2. Create a Dockerfile
3. Build a Docker Image
4. Run a Docker Container
5. Push a Docker Image to DockerHub
----


.Dockerfile
----
FROM centos:centos7.7.1908 <1>

RUN  yum check-update; \
   yum install -y gcc libffi-devel python-devel openssl-devel epel-release; \
   yum install -y python-pip python-wheel; \
   yum install -y openssh-clients; \
   pip install --upgrade pip; \
   pip install ansible
----

<1> from command

[quote, Albert Einstein]
A person who never made a mistake never tried anything new.

[TIP]
.$(pwd) or ${PWD} for Windows
====
The pwd command outputs the current working directory. It is used to populate the directory you wish to mount to the container.

**Windows** - Use `${PWD}` to mount the working directory. You also might need to enable https://rominirani.com/docker-on-windows-mounting-host-directories-d96f3f056a2c[Shared Drives] in DockerDesktop.

    docker run --volume ${PWD}:/ansible -w /ansible
====