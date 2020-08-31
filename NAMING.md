# Naming Convention

Taken from [Effective Dart: Design](https://dart.dev/guides/language/effective-dart/design)

Naming is an important part of writing readable, maintainable code. The following best practices can help you achieve that goal.

### DO use terms consistently
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

### AVOID abbreviations
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

### PREFER putting the most descriptive noun last
The last word should be the most descriptive of what the thing is. You can prefix it with other words, such as adjectives, to further describe the thing.


Good example :+1:
```
pageCount             // A count (of pages).
ConversionSink        // A sink for doing conversions.
ChunkedConversionSink // A ConversionSink that's chunked.
CssFontFaceRule       // A rule for font faces in CSS.
```

Bad example :-1:
```
numPages                  // Not a collection of pages.
CanvasRenderingContext2D  // Not a "2D".
RuleFontFaceCss           // Not a CSS.
```

### CONSIDER making the code read like a sentence
When in doubt about naming, write some code that uses your API, and try to read it like a sentence.

_Good example_ :+1:
```
// "If errors is empty..."
if (errors.isEmpty) ...

// "Hey, subscription, cancel!"
subscription.cancel();

// "Get the monsters where the monster has claws."
monsters.where((monster) => monster.hasClaws);
```

_Bad example_ :-1:
```
// Telling errors to empty itself, or asking if it is?
if (errors.empty) ...

// Toggle what? To what?
subscription.toggle();

// Filter the monsters with claws *out* or include *only* those?
monsters.filter((monster) => monster.hasClaws);
```

It’s helpful to try out your API and see how it “reads” when used in code, but you can go too far. It’s not helpful to add articles and other parts of speech to force your names to literally read like a grammatically correct sentence.

_Bad example_ :-1:
```
if (theCollectionOfErrors.isEmpty) ...

monsters.producesANewSequenceWhereEach((monster) => monster.hasClaws);
```
### PREFER a noun phrase for a non-boolean property or variable
The reader’s focus is on what the property is. If the user cares more about how a property is determined, then it should probably be a method with a verb phrase name.

_Good example_ :+1:
```
list.length
context.lineWidth
quest.rampagingSwampBeast
```

_Bad example_ :-1:
```
list.deleteItems
```

### PREFER a non-imperative verb phrase for a boolean property or variable
Boolean names are often used as conditions in control flow, so you want a name that reads well there. Compare:

```
if (window.closeable) ...  // Adjective.
if (window.canClose) ...   // Verb.
```

Good names tend to start with one of a few kinds of verbs:

- a form of “to be”: isEnabled, wasShown, willFire. These are, by far, the most common.

- an auxiliary verb: hasElements, canClose, shouldConsume, mustSave.

- an active verb: ignoresInput, wroteFile. These are rare because they are usually ambiguous. loggedResult is a bad name because it could mean “whether or not a result was logged” or “the result that was logged”. Likewise, closingConnection could be “whether the connection is closing” or “the connection that is closing”. Active verbs are allowed when the name can only be read as a predicate.

What separates all these verb phrases from method names is that they are not imperative. A boolean name should never sound like a command to tell the object to do something, because accessing a property doesn’t change the object. (If the property does modify the object in a meaningful way, it should be a method.)

_Good example_ :+1:
```isEmpty
hasElements
canClose
closesWindow
canShowPopup
hasShownPopup
```

_Bad example_ :-1:
```empty      // Adjective or verb?
withElements  // Sounds like it might hold elements.
closeable     // Sounds like an interface.
              // "canClose" reads better as a sentence.
closingWindow // Returns a bool or a window?
showPopup     // Sounds like it shows the popup.
```

**Exception**: Input properties in Angular components sometimes use imperative verbs for boolean setters because these setters are invoked in templates, not from other Dart code.
