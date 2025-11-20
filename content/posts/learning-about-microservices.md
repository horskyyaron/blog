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

## Chapter 2 - The Evolutionary Architect

The term "Architect" has problems in the world of software.
It was borrowed from an existing profession when the architect has accountability
for example: if the building collapsed, the architect is the responsible persona.

But with software, things change over time, all the time, so you can't have a "correct"
timeless solution, since the future is unknown.

When we create documentation, and diagrams on diagrams describing the perfect system, that is a lie.

We should think of ourselves as 'City planner' as we care about zones in the system.

Requirements and resources are rapidly changing (money, technology)

Once our product reaches the client, we will reveal how the user actually uses the product,
and what we thought will be the main part of the system, might not be, thus we need
to differently allocate our resources.

We should plan to allow for changes.

Avoid over specifying things.

> the system should be habitable for developers to

We should worry less about what happen inside the zones and more between the zones.
How do they talk to each other.

### Principled approach

In Microservices we have many decisions we can make: database, language, this framework or the other.

What should we do then? Framing

We should set principles and practices. 

Principles - set of rules we made in order to achieve some larger goal.
Practices - how we ensure our principles are being carried out. a set of detailed practice
guidance for preforming a task.

Defining the required standard - what is a good citizen?

- monitoring (system health, push vs pull approaches)
- interfaces. (HTTP, verbs or nouns? pagination resources? Versioning of endpoint?)
- safety. 



