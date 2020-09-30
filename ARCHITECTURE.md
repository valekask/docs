# Architecture best practices
### 1) Lean Components
Modern frontend application builds on top of component-based acrhitecture. Application is a collection of components. 
Each component should be simple and focused on one functionality at a time. Decomple presentation layer from application logic. 
For this goal, split components into container and presentational components. Moreover, keep all business logic in component-specific services.


#### Container components
- Contain all the state needed for the child component
- Transform data into a shape that is most convenient for the presentational component
- Translate events from the presentational component
- Stateful


#### Presentational components
- Present piece of application state that is passed through input properties
- Delegate user interaction up to the container components via output events
- Stateless, but might keep local UI state which is part of UI behaviour
- Should not know about state management layer



#### Component-specific Services
- Keep behaviour logic
- Encapsulate business logic
- Transform from data to view model
- Validate data and user input
- Handle application-specific events
- Should not know about state management layer

Some architecture guides recommend to use Presenters or Facades to encapsulate complex presentational logic. 
I think such approaches more feat for very large apps and might be overcomplicated for medium-size apps. 


### 2) Layers
Separate concerns into layers to establish a well organized system where each part 
fulfills a meaningful and intuitive role while maximizing its ability to adapt to change.
Each layer has it's own scope of responsibility and rules how to commmunicate with other layers.

In our application, we separate functionality into these layers:
- presentation (presentational components, view models, pipes, directives)
- business logic (containers, component-specific services, data models)
- state management (store, actions, selectors)
- data access (data services)
- core (authorization, security, sessions, interceptors)

When organizing behavior, the following goals should be sought:
- eliminate the duplication of functionality
- restrict the scope of work to a maintainable size
- restrict the scope of work to the description of the containing boundary
- restrict the scope of work to the inherent behavior of the containing boundary
- minimize external dependencies
- maximize the potential for reuse

Main goals of concerns separation: 
- separate different concerns and levels of abstraction
- separate low level implementation details and high level policies
- easier time changing the details without breaking the component


### 3) Modular design
Slice the application into feature modules representing different business functionalities.
Deconstruct system into smaller pieces for better maintainability. Use lazy-loading technique to speed up application loading.
- Use feature-modules to encapsulate every components, directives or pipes related to business area
- Use shared-modules to encapsulate common utility components, directives or pipes
- Use core-modules to keep application-wide singleton services

### 4) Data flow
A larger Angular application is probably going to feature various states in which it can find itself. 
Clicking on a toggle can lead to the change of a tab, selection of a product and highlighting of a row in a table, all at the same time. 
Doing that on the DOM level (like jQuery manipulation) would be a bad idea because you lose the connection between your data and view.
Manage UI state using data.

Keep unidirectional data flow. Container components take application state and pass it down the component tree to the presentational component.
Presentational components never modify data locally. If it want to change application state, it has to pass action to the parent component via output events.
In this way, application state will be predictable and consistent.

Try to use reactive state management via observable steams in the system. It makes data flow simpler and allow to use OnPush strategy to detect changes. 
It's better to avoid using of ngOnChanges hook to detect value changes. 

## Q&A
#### [Difference between class and interface in Typescript](https://stackoverflow.com/questions/40973074/difference-between-interfaces-and-classes-in-typescript)

  - Use `Interface` to describe model with properties.

  - Use `Class` to describe model with properties and behaviour.


#### [How to detect when an @Input() value changes in Angular?](https://stackoverflow.com/questions/38571812/how-to-detect-when-an-input-value-changes-in-angular/44686085)

#### [Difference between constructor vs ngOnInit in Angular](https://stackoverflow.com/questions/35763730/difference-between-constructor-and-ngoninit)

#### [How can I select an element in a component template?](https://stackoverflow.com/questions/32693061/how-can-i-select-an-element-in-a-component-template)


## Style guidelines worth giving a second read
#### Extract non-presentational logic to services

[Style 05–15: Delegate complex component logic to services](https://angular.io/guide/styleguide#delegate-complex-component-logic-to-services)

It tells us to extract non-presentational logic to services. Next, it tells us to keep components simple and focused on what they’re supposed to do. In other words, we should minimise logic in templates, delegate logic away from component models, keep component small, so no 1,000 lines of code components.

#### Don’t put presentation logic in the template

[Style 05–17: Put presentation logic in the component class](https://angular.io/guide/styleguide#put-presentation-logic-in-the-component-class)

Templates should worry about declarative DOM manipulation and event binding, not about implementation details.

#### Don’t create a component when a directive will do what you need

[Style 06–01: Use directives to enhance an element](https://angular.io/guide/styleguide#use-directives-to-enhance-an-element)

This guiding principle reminds us that we should not always be jumping to using a component straightaway. In fact, if no template is needed or the DOM changes can be reflected in the host element itself, an attribute directive will do good by us.

#### Do one thing and do it well

[Style 07–02: Single responsibility](https://angular.io/guide/styleguide#single-responsibility-1)

It recommends us to create services that encapsulate logic from a single horizontal layer at a single abstraction level.

#### Component level services

[Style 07–03: Providing a service](https://angular.io/guide/styleguide#providing-a-service)

It tells us about different between root and component level services.

#### Extract non-presentational concerns to services

[Style 08–01: Talk to the server through a service](https://angular.io/guide/styleguide#talk-to-the-server-through-a-service)

It tells us to delegate task of data manupulation to a service so that components do not have to know or worry about the details.

## Resources
- [Lean components](https://indepth.dev/lean-angular-components/)
- [Container components](https://indepth.dev/container-components-with-angular/)
- [Presentational components](https://indepth.dev/presentational-components-with-angular/)
- [Presenters](https://indepth.dev/presenters-with-angular/)
- [The Art of Separation of Concerns](http://aspiringcraftsman.com/2008/01/03/art-of-separation-of-concerns/)
- [Anatomy of a large Angular application](https://medium.com/@krposlek/anatomy-of-a-large-angular-application-f098e5e36994)
