---
layout: post
title: Creating a Minecraft Server with Terraform
author: scott_bouloutian
excerpt: 
modified: 2019-11-30T4:25:00-05:00
categories: projects
tags:
  - minecraft
  - terraform
  - aws
image:
  path: /images/minecraft-bees.jpg
comments: true
share: true
---

## The Problem
At the time of writing this, Minecraft is on the verge of another update, 1.15. Known as the Buzzy Bees update, I am sure you can guess one of the new mobs being introduced into the game 🐝. I wanted to create and automate the infrastructure needed to host a state of the art minecraft server. The server should take into account modern security best practices, be flexible enough to be altered or modded at any time, and ideally be hosted on AWS.

## Terraform
After researching a few possible solutions including Packer, CloudFormation, Chef, and Puppet, I settled upon Terraform.
As described on their homepage, Terraform is a powerful tool which allows you to use Infrastructure as Code to provision and manage any cloud, infrastructure, or service. This was exactly the tool I was looking for in order to spin up a Minecraft server, plus I have been meaning to play around with HashiCorp's infrastructure as code solution for a while.
Another neat feature of Terraform is its ability to generate a visual display of its resource hierarchy. Below is an image of the resources being managed for the minecraft server.

![Resource Graph](https://raw.githubusercontent.com/ScottBouloutian/thecraftmine/master/terraform/graph.svg?sanitize=true)

## Minecraft Server Manager
While Terraform is used to automate the required infrastructure, Minecraft Server Manager is used to automate the
orchestration of various different types of minecraft servers. MSM is installed and configured on an EC2 instance
using various configuration files. For example, the game difficulty and seed are two of the many parameters specified in
these files. MSM is also extremely flexible in the types of servers it is able to bootstrap and manage. In this particular instance I configured it to bootstrap the prerelease snapshot 1.15-pre3.

## Solution
Although there is still room to add support for a variety of different minecraft features, I was extremely pleased
with the result. Running a single terraform command is able to completely bootstrap a fully functional minecraft server.

<div markdown="0"><a href="https://github.com/ScottBouloutian/thecraftmine" class="btn">Source Code</a></div>
