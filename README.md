![software-architecture.jpg](software-architecture.jpg)

<h3 align="center">Software Architecture</h3>
<p align="center">
  Notes on Head First Software Architecture
</p>

# Content

- [Summary](#summary)
- [Notes](#notes)

# Summary
Head First Software Architecture teaches how to think architecturally, by explaining about the laws of architecture and the dimensions that describe an architecture. 
# Notes

- [Chapter 1](#chapter-1)
- [Chapter 2](#chapter-2)
- [Chapter 3](#chapter-3)
- [Chapter 4](#chapter-4)
- [Chapter 5](#chapter-5)
- [Chapter 6](#chapter-6)
- [Chapter 7](#chapter-7)
- [Chapter 8](#chapter-8)
- [Chapter 9](#chapter-9)
- [Chapter 10](#chapter-10)
- [Chapter 11](#chapter-11)
- [Chapter 12](#chapter-12)
- [Chapter 13](#chapter-13)

---

### Chapter 1

Software architecture consists of four dimensions:

- Architectural characteristics (which aspects of the system need to be supported).

Defined by architectural characteristics, which are non-functional requirements such as performance, scalability and reliability.

- Architectural decisions.

Choices we make about structural aspects of the system that have long-term or significant implications.

- Logical components (describing the building blocks).

Logical components are the building blocks of a system. It performs some sort of function, such as processing a payment. A logical component should always have a well-defined role and responsibility in the system.

- Architectural style (defines the physical shape).

Defines the overall shape and structure of a software system. For instance a microservice style, layered architectural style or event-driven styles.

The more strategic a decision is, the more architectural the decision becomes. The opposite of a strategic decision, is a tactical decision.

Functional requirements define the functionalities and features that a system must have. These requirements are expressed as use cases. It represents the desired functionalities that users expect from the software.

### Chapter 2

Our framework:

- Architectural characteristics: these are the nonfunctional requirements.
- Architectural decisions: These represent the problem domain.
- Architectural style: These are the decisions we make after doing a
  trade-off analysis.
- Logical components: Architectural characteristics and logical components together help us decide on the architectural style.

This chapter is about architectural characteristics. If we combine architectural characteristics with logical components (chapter 4), we have the structural considerations for an architecture, we can categorize architectural characteristics into 3 parts:

- Explicit: this represents how architectural characteristics differ from the domain.
- Implicit: some problems (like scalability), can only be solved through architecture.
- Check what is critical to the application success: to avoid over engineering. We should keep the architectural characteristics as minimum as possible (7 is a magical number here)!

### Chapter 3

When communicating with downstream services, we can usually use a publisher and a consumer. We have two options:

- Queue: point to point communication model. Publisher knows who is receiving the message. Each consumer has its own queue.
- Topic: broadcasting model. If another service wants to hear from the publisher, it subscribes to the publisher to receive the message.

A queue, can work with customized messages for each consumer, and it is safer. We know who is the recipient. It needs close coupling and if a new consumer wants to get the messages that needs to be built.
A topic is less safe, but can be extended more easily. It has low coupling, unlike the queue it is hard to monitor though we do not know where the messages will go.

### Chapter 4

Logical components are the functional building blocks of the system, also known as the problem domain. This translates to the architecture in the code:

- app (domain)
    - order (sub domain)
        - tracking (logical component = order tracking)
            - source_code_file
            - source_code_file


A logical architecture shows all the building blocks of a system and how they interact with each other (who calls what?), a physical architecture shows architectural style, services, databases etc. used.

To design an architecture we go through the following steps:

- Identifying initial core components:

We have to be careful of the entity trap: a component should be clearly defined, which makes sure its role and responsibilities are clear too. If that is not the case, then the component will get into the entity trap, where it takes on all the responsibility for that entity which makes it hard to maintain - for instance a "bid manager" can do 100 things such as accepting a bid, generate bid reports, determine a bid winner, and more.

- Assign requirements.
- Analyze roles and responsibilities:

If one component is doing too much, try to put some of these responsibilities in other (or new) components.

- Analyze characteristics:
    - Scalability: It has to support thousands of bidders per second.
    - Availability: It must be up and running while auctions take place.
    - Performance: it must accept a bid and get it to the auctioneer as fast as possible.\

Component coupling is about the degree of which components know about and rely on each other. The more they interact, the more tightly coupled the system is, and the harder to maintain (ball of mud). The Law of Demeter can help to make loosely coupled systems:
- We use this law to decouple systems. It does not matter that we change the component that knows about the rest of the system just to another location, that is fine. Most important is that we have individual components that know "as less as possible" about the rest of the system:
    - Tightly coupled: centralized workflow, but more risk with each change.
    - Loosely coupled: distributed workflow, but less risk with each change.

### Chapter 5

There are different types of architectures:

|             | Technical            | Domain           |
|-------------|----------------------|------------------|
| Monolith    | Layered, microkernel | Modular monolith |
| Distributed | Event-driven         | Microservices    |
|             |                      |                  |
(Top columns are related to partitioning, left rows are related to Deployment model)

- In a technically partitioned architecture, we have separation of technical concerns (such as presentation, services, persistence).
- In a Domain partitioned architecture, we separate in ways aligned with the domain (such as customer, payments, shipping).

- In a monolithic architecture, the entire application runs as one process.
- In a distributed architecture, components represent smaller units. They talk with each other over the network.

### Chapter 6

A common architecture design pattern is MVC:

- View concerns the UI and how the user interacts with the system.
- Model, which contains most of the code, business logics and workflows. Usually a persistence layer is used to map code based hierarchy to database structures.
- Controller sits in between and connects interfaces with each other (think about business logics that we only want to write once, while communication within the app can be done in json, xml or other formats that need to be supported by the controller + some validations on that).

MVC has many similarities with the layered architecture. Layered architecture, also represents the monolithic structure with separation of layers according to technical capabilities: presentation, business rules and persistence.

### Chapter 7

Contrary to the layered architecture, a modular architecture splits the codebase based on business domains. A module here means (high level), how we organize our code. It represents an independent unit that fulfills one piece of functionality. In a layered architecture (MVC) all layers are modules:

- app
    - orderapp
        - order (module)
            - presentation
                - source_code_file
            - workflow
                - source_code_file
            - persistence
                - source_code_file
        - recipe (module)
            - ...

Once the codebase has been split across different business domains (modules), the modules should still be able to talk with each other. Each module should expose public APIs so that different modules can talk with each other. In that way (publicly exposed APIs) we can change logics in each and individual module.
The next step would be to modularize the database as well, the only rule here is: every module should access only its own tables!

### Chapter 8

A microkernel architecture derives its name from the operating system design. The kernel, or core, is very small offering only the most basic functionality. This architecture consists of two parts: its core and its plugins.

### Chapter 9

Designing of the TripEZ app.

### Chapter 10

A microservice is a single-purpose, separately deployed unit of software that does one thing really well. Another feature that makes microservices special is that they own their own data. Each microservice is the only one that can directly access its data.
Physically associating a microservice with its data is known as creating a physical bounded context. If another service wants to access your data, it has to ask for it.

How micro should a microservice be? Not too small, like a grain of sand (Grains of Sand antipattern), and not too big. We use the following forces:

- Granularity disintegrators
- Granularity integrators

| Disintegrators                                                                                                       | Integrators                                                                                         |
|----------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Cohesion - all things should relate to each other.                                                                   | Database transactions - if data consistency and integrity is important, it needs a single database. |
| Fault tolerance and availability - errors wont affect other services.                                                | Data dependencies - with highly coupled data (foreign keys) its better to combine the database.     |
| Access control - the larger the service the harder to control access.                                                | Workflow and geography - too much coupling between services can lead to issues.                     |
| Code volatility - the more changes within a specific part of the service the more we need to test with redeployment. |                                                                                                     |
| Scalability and throughput - do some parts of the service need to be more scalable than others?                      |                                                                                                     |

With microservices, common logics (such as logging) lives in a shared library (deployed within the microservice as part of a JAR file) or shared service (a new and separate microservice).
If we need to make a request that needs multiple microservices to be involved, it can either be carried out in an orchestration way (a new microservice is created calling all separate services in order) or choreography (existing microservices call each other).

### Chapter 11

Reducing a customer's waiting time is called responsiveness. Handling more customers within a specific amount of time is called scalability. These two concepts are fundamental in event-driven architecture (EDA), which is about breaking up processing into separate services with each of those services performing its function at the same time by responding to an event. 
In EDA, services communicate asynchronously through an event channel, meaning they do not wait for responses of other channels to complete their work.

Events are a way for a service to let the rest of the system know something important has just happened. Events are the means of passing information to other services. Two important differences between events and messages:

- Events are broadcast to other services using topics, whereas messages are sent to a single service using queues. 
- Events always broadcast something that has already happened, whereas messages request something that needs to be done.

EDA and microservices are not the same. For EDA databases across services can be shared. EDA responds to something that has happened, microservices responds to something that has to happen. Whereas EDA relies on async communication, microservices rely on sync communication. When combining the two, it is called event-driven microservices.

### Chapter 12

Make the Grade practice assignment.

### Chapter 13

Appendix.