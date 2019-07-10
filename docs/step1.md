# Basics of Swift Development

> This isn't truly an overview of the basics of Swift development, but a recap of important features that will be important shortly. For a full intro to Swift, check out [Swift Essentials](https://trailhead.salesforce.com/en/content/learn/modules/swift-essentials) on Trailhead and [Swift](https://developer.apple.com/swift/)

## Objects and Pass by

Swift is Object Oriented. There are Classes and, _importantly_ classes are passed by _reference_. In Swift, however, we also have Structs. Struct's are pass by _value_. This is an important distinction, and one you need to keep in mind.

Importantly, both Classes and Structs (as well as Enums) can have functions encoded with them.

Structs offer the following advantages:

- Thread Safe
- No references means no Memory leaks
- Value types are faster than References types. (I.e. Structs are faster than Classes)

> Key Takeaway: Use a Struct when you want to use data across multiple threads or you want each copy to have independent state. Use a Class when you want shared state.

## Closures

Closures are functions that are passed around or used as parameters in other functions. Similar to Javascript's Anonymous Functions. Swift has many ways of declaring closures. Too many to highlight here. For a complete cheat-sheet of closure syntax in Swift check this out: [GoshDarnClosureSyntax.com](http://goshdarnclosuresyntax.com/).

Here are the two _primary_ ways of declaring a closure in Swift.

When the Closure is _not_ the final parameter to a method:

```swift
array.sorted(by: { (item1: Int, item2: Int) -> Bool in return item1 < item2 })
```

In the example above `swift array.sorted(by:)` is the method being called, and `swift{ (item1: Int, item2: Int) -> Bool in return item1 < item2 }` is the closure. Anything after the `in` keyword is the body of the closure.

When the Closure is the _final_ parameter in the calling method:

```swift
array.sorted { (item1, item2) in return item1 < item2 }
```

## Protocols

Protocols have no immediate analog to Apex, or other common OO lanaguages aside from Obj-c. They are similar in concept to Interfaces, and used to compose an object (struct or class). When an object conforms to a protocol, it implements properties and methods defined by the protocol. Generally speaking, protocols are the correct abstraction when the functionality defined by the protocol can be described as '-able'. Ie: rideable, sharable, or editable. For instance, a protocol named `ridable` may define a 'goForward' method for movement that a horse, bike and car would all be able to implemnt, even though the implementation is different.

Protocols are the primary method of object composition in Swift. Protocols can extend other protocols. For example, a `Ridable` protocol can implement a `Moveable` protocol. Any object conforming to `Ridable` would also have to conform to `Moveable`

Defining a Protocol:

```swift
//This Ridable protocol not conform to any parent protocols
protocol Ridable {}
```

```swift
//This Ridable protocol also conforms to Movable
protocol Ridable: Movable {}
```

Defining a conforming object:

```swift
// this struct would need to conform to both protocols
struct SomeStruct: Protocol1, Protocol2 {}`
```

```swift
// This class needs to conform to Ridable, which also requires conformance with Movable
class awesomeClass: Ridable {}
```

## Playgrounds

One of my favorite things about Swift is the built-in REPL (Read, Eval, Print, Loop). In Xcode these are called `playgrounds`. I encourage you to create a new playground `file > new > playground` from within Xcode.

## Helpful links.

These links are supplemental, and clarify the edge cases and nuances of things like classes vs. structs.

- [Swift Basics: Struct vs Class](https://blog.usejournal.com/swift-basics-struct-vs-class-31b44ade28ae)

<a href="step2.html" class="btn btn-default pull-right">Next <i class="glyphicon glyphicon-chevron-right"></i></a>
