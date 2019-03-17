---
title: Label and Selector
tags: [docker]
keywords: Kubernetes Label Selector
last_updated: March 17, 2019
summary: "Kubernetes label is an method to keep its components organized. Labels can also be used by k8s to identify resources to act on."
sidebar: mydoc_sidebar
permalink: k8s_lable_and_selector.html
folder: docker
---
## Label
* Labels are key-value pairs, which you can attach to all kinds of kubernetes components such as services, pods and etc.
* Labels do not affect kubernetes semantics. 

## Selector
Selectors are a way of expressing what objects to select based on their labels. 

By selectors you can specify if a label equals a criteria or inside a set of criteria.
* equality-based
* set-based
