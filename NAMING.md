# Naming Convention

Taken from [Effective Dart: Design](https://dart.dev/guides/language/effective-dart/design)

Naming is an important part of writing readable, maintainable code. The following best practices can help you achieve that goal.

##### Content:
- [DO use terms consistently](#do-use-terms-consistently)
- [AVOID abbreviations](#avoid-abbreviations)
- [PREFER putting the most descriptive noun last](#prefer-putting-the-most-descriptive-noun-last)
- [CONSIDER making the code read like a sentence](#consider-making-the-code-read-like-a-sentence)
- [PREFER a noun phrase for a non-boolean property or variable](#prefer-a-noun-phrase-for-a-non-boolean-property-or-variable)
- [PREFER a non-imperative verb phrase for a boolean property or variable](#prefer-a-non-imperative-verb-phrase-for-a-boolean-property-or-variable)
- [CONSIDER omitting the verb for a named boolean parameter](#consider-omitting-the-verb-for-a-named-boolean-parameter)
- [PREFER the “positive” name for a boolean property or variable](#prefer-the-positive-name-for-a-boolean-property-or-variable)
- [PREFER an imperative verb phrase for a function or method whose main purpose is a side effect](#prefer-an-imperative-verb-phrase-for-a-function-or-method-whose-main-purpose-is-a-side-effect)
- [PREFER a noun phrase or non-imperative verb phrase for a function or method if returning a value is its primary purpose](#prefer-a-noun-phrase-or-non-imperative-verb-phrase-for-a-function-or-method-if-returning-a-value-is-its-primary-purpose)
- [CONSIDER an imperative verb phrase for a function or method if you want to draw attention to the work it performs](#consider-an-imperative-verb-phrase-for-a-function-or-method-if-you-want-to-draw-attention-to-the-work-it-performs)
- [AVOID starting a method name with get](#avoid-starting-a-method-name-with-get)
- [PREFER naming a method to___() if it copies the object’s state to a new object](#prefer-naming-a-method-to___-if-it-copies-the-objects-state-to-a-new-object)
- [PREFER naming a method as___() if it returns a different representation backed by the original object](#prefer-naming-a-method-as___-if-it-returns-a-different-representation-backed-by-the-original-object)
- [AVOID describing the parameters in the function’s or method’s name](#avoid-describing-the-parameters-in-the-functions-or-methods-name)
- [DO follow existing mnemonic conventions when naming type parameters](#do-follow-existing-mnemonic-conventions-when-naming-type-parameters)


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

_Good example_ :+1:
```
Isolate.spawn(entryPoint, message, paused: false);
var copy = List.from(elements, growable: true);
var regExp = RegExp(pattern, caseSensitive: false);
```

### PREFER the “positive” name for a boolean property or variable
Most boolean names have conceptually “positive” and “negative” forms where the former feels like the fundamental concept and the latter is its negation—“open” and “closed”, “enabled” and “disabled”, etc. Often the latter name literally has a prefix that negates the former: “visible” and “in-visible”, “connected” and “dis-connected”, “zero” and “non-zero”.


When choosing which of the two cases that true represents — and thus which case the property is named for — prefer the positive or more fundamental one. Boolean members are often nested inside logical expressions, including negation operators. If your property itself reads like a negation, it’s harder for the reader to mentally perform the double negation and understand what the code means.

_Good example_ :+1:
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

_Good example_ :+1:
```
list.add("element");
queue.removeFirst();
window.refresh();
```
This way, an invocation reads like a command to do that work.

### PREFER a noun phrase or non-imperative verb phrase for a function or method if returning a value is its primary purpose
Other callable members have few side effects but return a useful result to the caller. If the member needs no parameters to do that, it should generally be a getter. But, sometimes a logical “property” needs some parameters. For example, elementAt() returns a piece of data from a collection, but it needs a parameter to know which piece of data to return.


This means the member is syntactically a method, but conceptually it is a property, and should be named as such using a phrase that describes what the member returns.

_Good example_ :+1:
```
var element = list.elementAt(3);
var first = list.firstWhere(test);
var char = string.codeUnitAt(4);
```

This guideline is deliberately softer than the previous one. Sometimes a method has no side effects but is still simpler to name with a verb phrase like `list.take()` or `string.split()`.

### CONSIDER an imperative verb phrase for a function or method if you want to draw attention to the work it performs
When a member produces a result without any side effects, it should usually be a getter or a method with a noun phrase name describing the result it returns. However, sometimes the work required to produce that result is important. It may be prone to runtime failures, or use heavyweight resources like networking or file I/O. In cases like this, where you want the caller to think about the work the member is doing, give the member a verb phrase name that describes that work.

_Good example_ :+1:
```
var table = database.downloadData();
var packageVersions = packageGraph.solveConstraints();
```

Note, though, that this guideline is softer than the previous two. The work an operation performs is often an implementation detail that isn’t relevant to the caller, and performance and robustness boundaries change over time. Most of the time, name your members based on what they do for the caller, not how they do it.

### AVOID starting a method name with get
In most cases, the method should be a getter with get removed from the name. For example, instead of a method named `getBreakfastOrder()`, define a getter named `breakfastOrder`.


Even if the member does need to be a method because it takes arguments or otherwise isn’t a good fit for a getter, you should still avoid get. Like the previous guidelines state, either:

- Simply drop get and use a noun phrase name like `breakfastOrder()` if the caller mostly cares about the value the method returns.

- Use a verb phrase name if the caller cares about the work being done, but pick a verb that more precisely describes the work than get, like create, download, fetch, calculate, request, aggregate, etc.

### PREFER naming a method to___() if it copies the object’s state to a new object
A conversion method is one that returns a new object containing a copy of almost all of the state of the receiver but usually in some different form or representation. The core libraries have a convention that these methods are named starting with to followed by the kind of result.


If you define a conversion method, it’s helpful to follow that convention.

_Good example_ :+1:
```
list.toSet();
stackTrace.toString();
dateTime.toLocal();
```

### PREFER naming a method as___() if it returns a different representation backed by the original object
Conversion methods are “snapshots”. The resulting object has its own copy of the original object’s state. There are other conversion-like methods that return views—they provide a new object, but that object refers back to the original. Later changes to the original object are reflected in the view.


The core library convention for you to follow is `as___()`.

_Good example_ :+1:
```
var map = table.asMap();
var list = bytes.asFloat32List();
var future = subscription.asFuture();
```

### AVOID describing the parameters in the function’s or method’s name
The user will see the argument at the callsite, so it usually doesn’t help readability to also refer to it in the name itself.

_Good example_ :+1:
```
list.add(element);
map.remove(key);
```

_Bad example_ :-1:
```
list.addElement(element)
map.removeKey(key)
```

However, it can be useful to mention a parameter to disambiguate it from other similarly-named methods that take different types:

_Good example_ :+1:
```
map.containsKey(key);
map.containsValue(value);
```

### DO follow existing mnemonic conventions when naming type parameters
Single letter names aren’t exactly illuminating, but almost all generic types use them. Fortunately, they mostly use them in a consistent, mnemonic way. The conventions are:


`E` for the __element__ type in a collection:

```
class IterableBase<E> {}
class List<E> {}
class HashSet<E> {}
class RedBlackTree<E> {}
```


`K` and `V` for the __key__ and __value__ types in an associative collection:

```
class Map<K, V> {}
class Multimap<K, V> {}
class MapEntry<K, V> {}
```


`R` for a type used as the __return__ type of a function or a class’s methods. This isn’t common, but appears in typedefs sometimes and in classes that implement the visitor pattern:

```
abstract class ExpressionVisitor<R> {
  R visitBinary(BinaryExpression node);
  R visitLiteral(LiteralExpression node);
  R visitUnary(UnaryExpression node);
}
```


Otherwise, use `T`, `S`, and `U` for generics that have a single type parameter and where the surrounding type makes its meaning obvious. There are multiple letters here to allow nesting without shadowing a surrounding name. For example:

```
class Future<T> {
  Future<S> then<S>(FutureOr<S> onValue(T value)) => ...
}
```
Here, the generic method `then<S>()` uses `S` to avoid shadowing the `T` on `Future<T>`.


If none of the above cases are a good fit, then either another single-letter mnemonic name or a descriptive name is fine:
```
class Graph<N, E> {
  final List<N> nodes = [];
  final List<E> edges = [];
}

class Graph<Node, Edge> {
  final List<Node> nodes = [];
  final List<Edge> edges = [];
}
```
In practice, the existing conventions cover most type parameters.
