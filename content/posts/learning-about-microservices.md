+++
title = 'Learning About Microservices'
date = 2025-11-19T10:03:53+02:00
draft = false
+++

This is a ongoing summary of the book *"Building Microservices"* by Sam Neuman.

# Chapter 1 - What are microservices

From the book:

> Microservices are small, autonomous services that work toggether. 

Breaking it down we get:

* Small - in terms of code base and team.
* Autonomous - separate entities, deployed as a independent service.

Communication between services are through network calls.

Thoughts are made about what is exposed vs hidden.

## Key Benefits

### Technology Heterogeneity

Since each service is independent, we can choose the right tools for the job,
for example:

-  database
-  language

### Resilience 

One service going down won't make the whole system crash.
NOTE: the system should be ready for this cases and handle them correctly.

### Scaling

With smaller services, in contrast to monolith, we can just scale the part of the system that 
requires more resources.

### Ease of deployment

We can deploy just the service that changes were made in, not the whole system.

### Optimize for replaceability

The cost of replacing one service is not that expensive since it is small in size 
and complexity.
And since requirements are being changed all the time, and new technology is being
created all the time, the system must be ready to embrace changes!
Microservices are optimize for that uncertainty


