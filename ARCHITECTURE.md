A goal of software architecture is to separate application concerns, and define rules and constraints of the communication between different parts.
System should be decomposed into layers. Each layer has it's own scope of responsibility and rules how to commmunicate with other layers.
Good decomposition allows developer to focus on one functionality at once and to do updates without breaking other parts of the application.





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

