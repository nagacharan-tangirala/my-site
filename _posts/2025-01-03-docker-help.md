---
layout: post
title: Helpful Docker Commands
date: 2025-02-28 09:00:00
description: Describes some regularly used docker commands
tags: howto
categories: howto
---

## Docker

It is a virtualization software that lets you containerized applications.
Here are some of the useful commands in Docker.

This is the command for starting a container of a given image in interactive mode.

```
docker run -it --entrypoint=/bin/bash myimagename
```
