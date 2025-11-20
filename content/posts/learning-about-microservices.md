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

# Chapter 2 - The Evolutionary Architect

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

## Principled approach

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

# Chapter 3 - How to model services

How to think about the boundaries of our microservices.

## What makes a good service?

1. loosely coupled
2. high in cohesion

### loose coupling

A Change in one service does not require a change in another.

A loosely coupled service knows as little as it needs about services it collaborates with.

Thus, we'll probably want to limit the number of different calls from one service to another.
This will reduce the change for tight coupling.

### High cohesion

Related behavior will sit together while different behavior sit elsewhere.

So we need to find boundaries in our problem domain that will help ensure that 
services of related behavior sits together.

## The bounded context

Any domain consists of multiple bounded contexts.
Each has its own explicit interface, deciding what to share with other contexts.

In `Domain-Driven-Design` book, Evans uses the metaphor of a cell.

> cell can exist because their membraned define what is in and out and determine what can pass

Example of bounded context, Warehouse department and Finance department.

### Shared and hidden models

A warehouse department has knowledge that only interest himself, e.g. number of
forklifts it has.
But it also has knowledge that interest other services such as the finance department
say list of the inventory.

Thus, the *stock item* becomes a shared model.

But the stock item in the warehouse context can have a representation different that one in the finance team.
So there is an *internal representation* and *external representation* .

A simple example:

Say a shared model is user:

```javascript
{
  "id": "u123",
  "name": "Yaron"
}
```

Authentication service internal representation:


```javascript
{
  "userId": "u123",
  "fullName": "Yaron Horsky",
  "passwordHash": "6fsd09dfj0932fjsd9f...",
  "lastLoginAt": "2025-11-20T10:00:00Z",
  "failedLoginAttempts": 3
}
```

Notification service representation:

```javascript
{
  "id": "u123",
  "displayName": "Yaron",
  "email": "yaron@example.com",
  "emailOptIn": true,
  "preferredLanguage": "en"
}
```

Sometimes models can mean very different things depending on the context being used.
For example *Order*:

Payment Service context:

```javascript
{
  "orderId": "o123",
  "totalAmount": 124.90,
  "currency": "USD",
  "paymentStatus": "PENDING"
}
```

Order Service context:

```javascript
{
  "orderId": "o123",
  "customerId": "c1",
  "items": [
    { "sku": "x", "quantity": 2 }
  ],
  "status": "PLACED"
}
```

Shipping Service context

```javascript
{
  "orderId": "o123",
  "address": {
    "street": "...",
    "city": "...",
    "country": "IL"
  },
  "weight": 5.4,
  "shippingStatus": "READY_TO_SHIP"
}

```

### Premature decomposition

Premature decomposition can be costly.
It is much easier to start with one code base and then split it than thinking ahead 
and split first.

## Business capabilities

We should think about context capabilities and not data that is shared.

E.g an *Ordering Service*, these are some capabilities that interests us:

* creating an order
* validate item availability
* apply business rules (discount, limits)
* change order state
* etc..

It exposes *behaviors* like:

```HTTP
POST /orders/validate
POST /orders/confirm
POST /orders/cancel
```

This is a bounded context. The order data is not the main issue, but the capabilities of the
service are.

VS the anemic version, a CRUD version of the same *Ordering Service*:

```HTTP
GET /orders/:id
POST /orders
PUT /orders/:id
DELETE /orders/:id
```

This is basically db with a wrapper.

Why this is bad?

- other services needs to implement the business rules
- high coupling: the one that writes the data controls behavior
- the core is the data, not the capabilities

## Communication in terms of business concepts

Changes are often about changes the business wants to make to how the system
behaves. We are changing capabilities exposed to our customers.

If the system is decomposed by bounded context of our domain, changes are local 
and isolated.

Thus its important to think of the communication in terms of business concepts.

It can be useful to think of forms being sent between these microservices, much
as forms are sent around an organization.

An example of form passing - Loan Application

Loan application Service -> credit check service -> risk service -> approval service -> payout service 
