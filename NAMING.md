# Naming Convention

Taken from [Effective Dart: Design](https://dart.dev/guides/language/effective-dart/design)

Naming is an important part of writing readable, maintainable code. The following best practices can help you achieve that goal.

### DO use terms consistently
Use the same name for the same thing, throughout your code. If a precedent already exists outside your API that users are likely to know, follow that precedent.

_Good example_ :+1:
```
pageCount         // A field.
updatePageCount() // Consistent with pageCount.
toSomething()     // Consistent with Iterable's toList().
asSomething()     // Consistent with List's asMap().
Point             // A familiar concept.
```

_Bad example_ :-1:
```
renumberPages()      // Confusingly different from pageCount.
convertToSomething() // Inconsistent with toX() precedent.
wrappedAsSomething() // Inconsistent with asX() precedent.
Cartesian            // Unfamiliar to most users.
```

The goal is to take advantage of what the user already knows. This includes their knowledge of the problem domain itself, the conventions of the core libraries, and other parts of your own API. By building on top of those, you reduce the amount of new knowledge they have to acquire before they can be productive.

### AVOID abbreviations
Unless the abbreviation is more common than the unabbreviated term, don’t abbreviate. If you do abbreviate, capitalize it correctly.

_Good example_ :+1:
```
pageCount
buildRectangles
IOStream
HttpRequest
```

_Bad example_ :-1:
```
numPages    // "num" is an abbreviation of number(of)
buildRects
InputOutputStream
HypertextTransferProtocolRequest
```

### PREFER putting the most descriptive noun last
The last word should be the most descriptive of what the thing is. You can prefix it with other words, such as adjectives, to further describe the thing.


_Good example_ :+1:
```
pageCount             // A count (of pages).
ConversionSink        // A sink for doing conversions.
ChunkedConversionSink // A ConversionSink that's chunked.
CssFontFaceRule       // A rule for font faces in CSS.
```

_Bad example_ :-1:
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

### CONSIDER omitting the verb for a named boolean parameter
This refines the previous rule. For named parameters that are boolean, the name is often just as clear without the verb, and the code reads better at the call site.

Good example :+1:
```
Isolate.spawn(entryPoint, message, paused: false);
var copy = List.from(elements, growable: true);
var regExp = RegExp(pattern, caseSensitive: false);
```

### PREFER the “positive” name for a boolean property or variable
Most boolean names have conceptually “positive” and “negative” forms where the former feels like the fundamental concept and the latter is its negation—“open” and “closed”, “enabled” and “disabled”, etc. Often the latter name literally has a prefix that negates the former: “visible” and “in-visible”, “connected” and “dis-connected”, “zero” and “non-zero”.


When choosing which of the two cases that true represents — and thus which case the property is named for — prefer the positive or more fundamental one. Boolean members are often nested inside logical expressions, including negation operators. If your property itself reads like a negation, it’s harder for the reader to mentally perform the double negation and understand what the code means.

Good example :+1:
```
if (socket.isConnected && database.hasData) {
  socket.write(database.read());
}
```

_Bad example_ :-1:
```
if (!socket.isDisconnected && !database.isEmpty) {
  socket.write(database.read());
}
```

For some properties, there is no obvious positive form. Is a document that has been flushed to disk “saved” or “un-changed”? Is a document that hasn’t been flushed “un-saved” or “changed”? In ambiguous cases, lean towards the choice that is less likely to be negated by users or has the shorter name.


__Exception__: With some properties, the negative form is what users overwhelmingly need to use. Choosing the positive case would force them to negate the property with ! everywhere. Instead, it may be better to use the negative case for that property.

### PREFER an imperative verb phrase for a function or method whose main purpose is a side effect
Callable members can return a result to the caller and perform other work or side effects. In an imperative language like Dart, members are often called mainly for their side effect: they may change an object’s internal state, produce some output, or talk to the outside world.


Those kinds of members should be named using an imperative verb phrase that clarifies the work the member performs.
Good example :+1:
```
list.add("element");
queue.removeFirst();
window.refresh();
```
This way, an invocation reads like a command to do that work.
