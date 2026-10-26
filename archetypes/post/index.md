---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
slug: "{{ .File.ContentBaseName | urlize }}"
description: "One sentence shown on the project card."
date: {{ .Date }}
image: cover.png
weight: 10
categories:
    - Machine Learning
tags:
    - Python
draft: true
---

**[GitHub Repo](https://github.com/u77w41/REPO-NAME)**

## Overview

What the project does and why.
