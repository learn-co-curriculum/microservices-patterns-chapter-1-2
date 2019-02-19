# Microservices Patterns - Week 1
## Chapters 1 & 2, (Steven)
### Chapter 1 					
*This chapter covers*

* The symptoms of monolithic hell and how to escape it by adopting the microservice architecture
* The essential characteristics of the microservice architecture and its benefits and drawbacks
* How microservices enable the DevOps style of development of large, complex applications
* The microservice architecture pattern language and why you should use it

### Chapter Summary					 					
* The Monolithic architecture pattern structures the application as a single deployable unit.
* The Microservice architecture pattern decomposes a system into a set of independently deployable services, each with its own database.
* The monolithic architecture is a good choice for simple applications, but microservice architecture is usually a better choice for large, complex applications.
* The microservice architecture accelerates the velocity of software development by enabling small, autonomous teams to work in parallel.
* The microservice architecture isn’t a silver bullet—there are significant drawbacks, including complexity.
* The Microservice architecture pattern language is a collection of patterns that help you architect an application using the microservice architecture. It helps you decide whether to use the microservice architecture, and if you pick the microservice architecture, the pattern language helps you apply it effectively.
* You need more than just the microservice architecture to accelerate software delivery. Successful software development also requires DevOps and small, autonomous teams.
* Don’t forget about the human side of adopting microservices. You need to consider employees’ emotions in order to successfully transition to a microservice architecture.

## What’s the app do?			
At its core, the FTGO application is quite simple. Consumers use the FTGO web- site or mobile application to place food orders at local restaurants. FTGO coordinates a network of couriers who deliver the orders. It’s also responsible for paying couriers and restaurants. Restaurants use the FTGO website to edit their menus and manage orders. The application uses various web services, including Stripe for payments, Twilio for messaging, and Amazon Simple Email Service (SES) for email.

The app was becoming a big ball of mud and was written using some increasingly obsolete frameworks. The FTGO application is exhibiting all the symptoms of monolithic hell. The FTGO application has a hexagonal architecture.

![Benefits of a monolith](https://i.imgur.com/mo0AFmh.png)

The benefits of the monolithic architecture
* Simple to develop tooling wise
* Easy to make radical changes to the application
* Straightforward to test
* Straightforward to deploy
* Easy to scale

Why Monoliths suck
* Complexity scares people. **What scares you in our codebase?(Flatchat/Ironboard/Phoeyonce)**
* Development is slow. I’m not sure we suffer from this.
* Path from commit to deploy is a pain. I don’t think we suffer from this.
* Scaling is hard. **What parts of Learn do we wish we could scale independently?**
* Delivering a reliable Monolith is challenging because one change may break something unrelated
* Locked into a specific tech stack.

> “Interestingly, software architecture has very little to do with functional requirements. You can implement a set of use cases—an application’s functional requirements—with any architecture.”

-ilities

Maintainability, extensibility, and testability are directly affected by architecture decisions.

Scalability cube

![Scalability cube](https://i.imgur.com/WQLy3oZ.png)

* X Scaling: Throw more servers at it. AKA servers are cheaper than developers. Load Balancer to find a server from a pool
* Z Scaling: Separate resources by type, id, or some identifier. Previously used on IDE v2 for directing students to different servers. Led to chaos when a server was moved out of rotation.
* Y Scaling: In the extreme, think AWS Lambdas.

![y-axis](https://i.imgur.com/VoevUyD.png)


The high-level definition of microservice architecture (microservices) is an architectural style that functionally decomposes an application into a set of services. A service has an API, which is an impermeable boundary that is difficult to violate. You can’t bypass the API and access an internal class. As a result, it’s much easier to preserve the modularity of the application over time.



![Keep data separated.](https://i.imgur.com/f7rWU6h.jpg)

![hexy](https://i.imgur.com/9jORZ6O.png)
Looks like the Hexagonal module but broken out into services.

Why Microservices are da best
* Continuous deployment of a mega app
* Apps are small and easier to maintain. (Are they?)
* Independently deployable and scalable
* Teams can be autonomous
* Experimentation with new languages is easier!
* Better fault isolation.

🍕🍕

Why microservices suck, or here’s why you should keep reading this book.
* Finding the right seams is hard. What’s a service?
	* Do it wrong and you’ll have a distributed monolith
* Distributed systems are a PITA
	* Partial Failure, split brain issues, distributed transactions
* Deploying features across services is hard
* Deciding when to make the move is hard.

Process and organization		
**Success inevitably means that the engineering team will grow. On the one hand, that’s a good thing because more developers can get more done. The trouble with large teams is, as Fred Brooks wrote in The Mythical Man-Month, the communication over- head of a team of size N is O(N 2). If the team gets too large, it will become inefficient, due to the communication overhead. Imagine, for example, trying to do a daily standup with 20 people. Strive for 8-12 people.** (🍕🍕-size teams)

Patterns everywhere! Patterns will be descibed in more detail in further chapters.

Change is hard, people are emotional. Whatayagonnado? ¯\\\_(ツ)\_/¯ Seems to include many stages of grief.

### Chapter 2
This Chapter Covers
* Understanding software architecture and why it’s important
* Decomposing an application into services by applying the decomposition patterns
* Decompose by business capability and Decompose by subdomain
* Using the bounded context concept from domain- driven design (DDD) to untangle data and make decomposition easier

Chapter Summary
* Architecture determines your application’s -ilities, including maintainability, testability, and deployability, which directly impact development velocity.
* The micro-service architecture is an architecture style that gives an application high maintainability, testability, and deployability.
* Services in a micro-service architecture are organized around business concerns— business capabilities or subdomains—rather than technical concerns.
* There are two patterns for decomposition:
	* Decompose by business capability, which has its origins in business architecture
	* Decompose by subdomain, based on concepts from domain-driven design
* You can eliminate god classes, which cause tangled dependencies that prevent decomposition, by applying DDD and defining a separate domain model for each service.

4+1 View Model of Software Architecture
* Logical View - Classes, Modules, OTP Apps
* Implementation View - In Ruby it’s the source code, elixir, an escript, or runnable code directory
* Process View -  Local processes running while your app runs. Phoeyonce has Docker + the Beam
* Deployment View - Where on your infrastructure your code is running.
* +  Scenarios - Describe the collaboration between parts in each view.

Architecture Style
**An architectural style, then, defines a family of such systems in terms of a pattern of structural organization. More specifically, an architectural style determines the vocabulary of components and connectors that can be used in instances of that style, together with a set of constraints on how they can be combined.**

Hexagonal Architecture Style
![](https://i.imgur.com/OOz4tTO.png)

Business Logic in the center, Inbound adapters for requests, outbound adapters for data persistence. Business logic doesn’t depend on the outside world. Ports define a set of operations. Inbound ports are methods or functions exposed via a public api, and outbound ports are how external systems will be called. Adapters call these ports. In Rails, a controller is an adapter that would call a port. We get closer to this in Phoenix with Controllers calling functions on a Context module.

**Because of this decoupling, it’s much easier to test the business logic in isolation. Another benefit is that it more accurately reflects the architecture of a modern application. The business logic can be invoked via multiple adapters, each of which implements a particular API or UI. The business logic can also invoke multiple adapters, each one of which invokes a different external system. Hexagonal architecture is a great way to describe the architecture of each service in a microservice architecture.**

#### WHAT IS A SERVICE?
A service is a standalone, independently deployable software component that implements some useful functionality. There are two types of operations: commands and queries.

#### WHAT IS LOOSE COUPLING?
All interaction with a service happens via its API, which encapsulates its implementation details. This enables the implementation of the service to change without impacting its clients.
The requirement for services to be loosely coupled and to collaborate only via APIs prohibits services from communicating via a database.

On the size of a microservice: **A much better goal is to define a well-designed service to be a service capable of being developed by a small team with minimal lead time and with minimal collaboration with other teams.**

Steps for breaking up Monolith
1. Identify System operations (an abstraction of a request that the application must handle.)
2. Identify Services (Use DDD or Business Architecture.)
3. Define Service APIs and collaborations

Obstacles to decomposing an application into services
* Network latency
	* Reduced availability due to synchronous communication
* Maintaining data consistency across services
* Obtaining a consistent view of the data
* God classes preventing decomposition

On God Classes:
**This means that each of the services in the FTGO application that has anything to do with orders has its own domain model with its version of the Order class. A great example of the benefit of multiple domain models is the Delivery Service. Its view of an Order is extremely simple: pickup address, pickup time, delivery address, and delivery time. Moreover, rather than call it an Order, the Delivery Service uses the more appropriate name of Delivery.**

Sometimes our users are leads, students, admins, instructors, coaches.

#### Resources
* [Hexagonal Rails](https://www.youtube.com/watch?v=CGN4RFkhH2M)
* [NodeJS Microservices](https://www.youtube.com/watch?v=h8ihxzfqH0A)
* [Art of Scalability](http://theartofscalability.com/)
* [Suck Rock Dichotomy](http://nealford.com/memeagora/2009/08/05/suck-rock-dichotomy.html)
* [No Haunted Forests](https://john-millikin.com/sre-school/no-haunted-forests)
* [Elixir in the times of microservices](http://blog.plataformatec.com.br/2015/06/elixir-in-times-of-microservices/)
* [DDD Quickly](http://carfield.com.hk/document/software%2Bdesign/dddquickly.pdf)

#### Questions
* The book mentions Domain Driven Design and gives some brief definitions but would be good to get a little more info on this pattern.
* What is the difference between “decomposing by business capability” vs. “decomposing by sub-domain”? In the example in chapter 2 we seem to end up with the same result.
* The book talks about using OOO design as a model for architecting your services. At Flatiron, we’ve been building more and more with functional languages (Elixir). How does this impact the use of OOO as a guiding architecture principle?
* Synchronous communication hurts availability but async message passing helps address this issue -- is this a concern that Elixir can help us solve for by building concurrent workflows?
* What is a distributed monolith? How does it differ from microservices architecture? (I think that Curriculum Deployer + Ironboard = distributed monolith btw - sophie)
* So, what could we have done better with Flatchat?
* Can we talk more about microservices vs. service-oriented architecture? It sounds like people often conflate them.
* The book mentions that there are emotional effects that accompany the change to microservices. I’ve seen this happen as tickets get drawn up and thrown over the wall. How can we maintain our helpful culture as we sorta move toward microservices?
* As far as the API Gateway goes, could we or should we imagine a single unified API that adheres to certain standards, maybe something that functions more like a public API?
* How should we think about ownership of microservices? I’ve experienced situations where teams roll off and then no one actively maintains or develops on them, and yet they sometimes need work.

* How are people liking this book compared to FunctionalJS/POODR? Feels more academic?
* Of the monolith pros/cons, which ones remind you the most of Ironboard? Which ones feel like we should mitigate the most?
* Ideas of how to decompose Ironboard into domains or services? Where would you draw lines?
