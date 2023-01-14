# Understanding Event-Driven Architecture

# Introduction

Event-driven Architecture is a software architectural pattern that is made up of highly decoupled, single-purpose event processing components that asynchronously receive and process events. These event processing components work to achieve a single purpose and don't depend on other event processors.

The event-driven software architectural pattern consists of two main topologies

- The Mediator Topology
- The Broker Topology

In this article, you will learn how event-driven architecture works on a high level as well as the several components that make us the architecture


# Mediator Topology

The mediator topology is often used when you need to orchestrate or perform several steps within an event through a central mediator. For example, a single event that takes place on the stock trade might require you to first validate the trade, check trade compliance, assign trade to a broker, calculate commission before finally placing the trade with the broker. As you can see, this event involves performing several steps before the final action takes place. Such events are very suitable for the mediator topology. The mediator topology is made up of four(4) components

- Event Queues
- Event Mediator
- Event Channels
- Event Processors

When the client sends an event, the flow goes straight to the Event Queue.

**Event queues** receive initial events from clients and serve as a pathway to the Event Mediator which acts like the central processor taking events from the queue and processing them. 

**Event Mediator** is usually aware of the several steps needed to be taken for the particular event to be completely processed. Due to this level of awareness and intelligence, the Event Mediator knows the right channel to send a particular step in the event to. 

**Event Channels** receives events in form of notifications from the Event Mediator which are then handed over to the Event Processors

**Event Processors** are self-contained and stand-alone entities that listen on the Event channel and execute a specific business logic to process the event


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1622814040288/YxB4N3E2K.png)

In a real-life scenario, the Event queue can be made of a dozen to several hundred event queues and the implementation of the event queue depends on the organizational requirements. The implementation of the queue can be a message-queue, a web-service endpoint, or a combination of both


# Broker Topology

The Broker topology is different from the Mediator topology in that, there is no central mediator: rather, the message flow is distributed across the event processor in a chain-like fashion through a lightweight broker. This topology is usually useful for simple event processing flow and when you don't need a central mediator

The Broker topology is made up of two main components
- Broker 
- Event Processors

**The Broker** component can be centralized or federated and it usually serves as the view or dashboard for Event processors to do their Work.  The broker components usually contain all of the event channels that are used in the event flow. The event channel contained within the broker component can be a message queue, message topics, or a combination of both

Another way to understand the Broker Topology is to think of a relay race. In a relay race - You have four sprinters, each run a specific lap and hand over the baton to the next - in a chain-like fashion. The same concept applies here. Each athlete runs their own race which in this case is the Event Processor. When the athlete is done, they usually pass their baton to the next - so the race continues till the end.

The **Event Processors** in the Broker topology serve as the athlete in the relay race analogy, They perform a specific operation and notify the Broker that they are done via an Event Channel. Once they notify the broker via the Event Channel the next Event Processor picks up the baton and performs the next specific operation till all actions in the Event is complete

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1622815642706/e6E8C8erc.png)

The broker topology is all about the chaining of events to perform a business function

# Considerations

The Event-driven software architectural pattern is a relatively complex pattern to implement due to its asynchronous distributed nature. There are a couple of distributed issues you must address when implementing this pattern

- Remote process availability
- Lack of responsiveness from a processor or component
- Broker reconnection logic in the event of broker or mediator failure
- Planning the granularity of event processor components etc

# Conclusion

While there are several other software architectural patterns, in this article you have been able to understand at a high level how event-driven architecture works and some of the tradeoffs to consider while using the pattern. The pattern is not a one size fit, but it addresses some issues of distributed architecture like service agility, high performance, ease of deployment, etc. You can also read more about Software Architectural patterns [here](https://tianpan.co/notes/145-introduction-to-architecture).

