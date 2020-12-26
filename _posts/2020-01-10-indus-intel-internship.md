---
layout: post
title: Software Engineering Internship at IndusIntel
tags: 
excerpt_separator: <!--more-->
---

During my internship at IndusIntel, I worked on prototyping the data collection scheme for CNC machines of 
machine health and operational parameters. Due to the high volume of this type of data, alongside the scaling requirements
I implemented this prototype with Cassandra, a NoSQL database model.

For the machine-to-database pipeline, I implemented a RESTful API backed by Kafka in the interest of design simplicity and 
fault-tolerance.