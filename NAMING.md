# Naming Convention

Taken from [Effective Dart: Design](https://dart.dev/guides/language/effective-dart/design)

Naming is an important part of writing readable, maintainable code. The following best practices can help you achieve that goal.

### DO use terms consistently.
Use the same name for the same thing, throughout your code. If a precedent already exists outside your API that users are likely to know, follow that precedent.

Good example :+1:
```
pageCount         // A field.
updatePageCount() // Consistent with pageCount.
toSomething()     // Consistent with Iterable's toList().
asSomething()     // Consistent with List's asMap().
Point             // A familiar concept.
```

Bad example :-1:
```
renumberPages()      // Confusingly different from pageCount.
convertToSomething() // Inconsistent with toX() precedent.
wrappedAsSomething() // Inconsistent with asX() precedent.
Cartesian            // Unfamiliar to most users.
```

The goal is to take advantage of what the user already knows. This includes their knowledge of the problem domain itself, the conventions of the core libraries, and other parts of your own API. By building on top of those, you reduce the amount of new knowledge they have to acquire before they can be productive.

### AVOID abbreviations.
Unless the abbreviation is more common than the unabbreviated term, don’t abbreviate. If you do abbreviate, capitalize it correctly.

Good example :+1:
```
pageCount
buildRectangles
IOStream
HttpRequest
```

Bad example :-1:
```
numPages    // "num" is an abbreviation of number(of)
buildRects
InputOutputStream
HypertextTransferProtocolRequest
```

