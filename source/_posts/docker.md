---
title: Getting started with Docker
date: 2019-12-05 11:28:38
categories: [CI, Basic, Tools]
tags:
---

Since its release in 2013, [Docker] has been receiving a lot of attention and is considered to be likely to change the software industry.

However, many people don't know exactly what Docker is, what problems to solve, and what are the benefits? This article explains it in detail to help everyone understand it. It also comes with easy-to-understand examples to teach you how to use it for daily development.

![Docker Logo][Docker Logo]

<!--more-->

# The Problem of Environmental Configuration

One of the biggest troubles of software development is environment configuration. The user's computer environment is different, how do you know that your own software can run on those machines?

Developers often say, "It works on my machine", which means that other machines will probably not run.

A virtual machine is a solution for installation with an environment. It can run one operating system in another, such as a Linux system in a Windows system. 

However, this scheme has several disadvantages.

* Resource occupation
* Many redundant steps
* Slow start

# Linux Container

Due to these shortcomings of virtual machines, Linux has developed another virtualization technology: Linux Containers (abbreviated as LXC).

Instead of emulating a complete operating system, **`a Linux container isolates processes`**. Or, put a [protective layer] over the normal process. 
Because containers are process-level, they have many advantages over virtual machines.

- Quick start
- Less resource occupation
- Small size

In short, containers are a bit like lightweight virtual machines which can provide a virtualized environment, but at a much lower cost.

# What is Docker?

**`Docker is a kind of encapsulation of Linux container, which provides a simple and easy-to-use container interface`**. It is currently the most popular Linux container solution.


# Example: Making Your Own Docker Container


## Writing a Dockerfile


## Creating an image file

## Generate Container

## Publishing Image Files





[Docker]: http://www.docker.com
[protective layer]: https://opensource.com/article/18/1/history-low-level-container-runtimes
[Docker Logo]: /img/docker-logo.png "Docker Logo"
