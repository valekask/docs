# Architecture best practices:
### 1) Lean Components
Modern frontend application builds on top of component-based acrhitecture. Application is a collection of components. 
Each component should be simple and focused on one functionality at a time. For this goal, split components into
container and presentational components. Moreover, keep all business logic in component-specific services.

#### Container components
Contain all the state needed for the child component. 
Transform data into a shape that is most convenient for the presentational component. 
Translate events from the presentational component. 

#### Presentational components
Present piece of application state that is passed through input properties. 
Delegate user interaction up to the container components via output events.
Might keep local UI state which is part of UI behaviour.

#### Services
Keep all business or view related logic.
Transform from data to view model.
Validate data.
Handle application-specific events.


### 2) Layers
Separate concerns into layers to establish a well organized system where each part 
fulfills a meaningful and intuitive role while maximizing its ability to adapt to change.
Each layer has it's own scope of responsibility and rules how to commmunicate with other layers.

In our application, we separate functionality into these layers:
- presentation
- business logic (containers, component-specific services)
- state management (store)
- data access (http services)

### 3) Modular design
Slice the application into feature modules representing different business functionalities.
Deconstruct system into smaller pieces for better maintainability.

### 4) Data flow
Keep unidirectional data flow. Container components take application state and pass it down the component tree to the presentational component.
Presentational components never modify data locally. If it want to change application state, it has to pass action to the parent component via output events.
In this way, application state will be predictable and consistent.




--- OLD
## Goal
A goal of software architecture is to separate application concerns, define rules and constraints of the communication between different parts.
System should be decomposed into layers. Each layer has it's own scope of responsibility and rules how to commmunicate with other layers.

Good decomposition allows developer to focus on one functionality at once and to do updates without breaking other parts of the application.


## Separation of Concerns
The overall goal of Separation of Concerns is to establish a well organized system where each part 
fulfills a meaningful and intuitive role while maximizing its ability to adapt to change.

We can slice our applications vertically or horizontally. 
When slicing vertically, we group software artifacts into modules by feature. 
When slicing horizontally, we group by software layer.
Each layer should be focused on a single conrern at a time.
There are following horizontal layers in the application:

Layers:
- Persistence
- 

Concerns:
- Persistence
- State Management
- Bussiness logic
- Presentation
- User interaction


In our applications, we can categorise the software artifacts into these horizontal layers, or system concerns.






### Vertical Separation
Vertical Separation of Concerns refers to the process of dividing an application into modules of 
functionality that relate to the same feature or sub-system within an application. 
Communication between vertical layers is easy and achievable by using standard techniques like sessions, URL parameters, etc.
Each module internally has horizontal separation. 

### Horizontal Separation
Horizontal Separation of Concerns refers to the process of dividing an application into logical layers of functionally that fulfill the same role within the application.

That’s where you actually build up your Angular application and place all its building blocks. It’s important to note that each layer (and block inside a layer) only knows about the layer above itself and doesn’t care about layers underneath that are going to consume its exposed functionalities.






1) What is software architecture and why we need it?

Modern frondend application builds on top of component-based acrhitecture.
Application is a collection of components. 
Software architecture is a way of compose components, 
and define rules and constraints of the communication between those components.

2) Why we need spent time for architecture?
- Keep sustainable development speed 
- Easy of adding new features


Good architecture should quickly answer to developer following questions:
- Where do I put this piece of code?
- Where data come from?
- How do I modify data?
- How come this event changed my data and state?
