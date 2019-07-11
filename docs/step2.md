# Basics of Interface builder, Storyboards and UIKit

> Also not a real introduction but a highlight of key information we'll be using

## What's a view again?

In the world of iOS, a view is any _element_ of a user interface. See a button? it's a view. Table? also a view. Cell in a table? also a view. The user interface you interact with is built from a heirarchy of views

## Every view has a controller, except when it doesn't.

Your logic for view state will live in your scene's view controller. Each element in the view will have a controller. Sometimes, the view controller of a given element is delegated to the scene controller. At other times, you'll create a new object and set the View's controller property to your instance.

```swift
// Here we're creating a new UIImagePickerController and delegting it's methods to the current scene controller
imagePickerCtrl = UIImagePickerController()
imagePickerCtrl.delegate = self
```

## Storyboards & Segues

A Storyboard is a collection of scenes and segues. Scenes represent a completed view heirarchy. Segues represent the transition between views; including any data you pass between them. When a user navigates between two scenes UIKit automatically calls `prepare(for segue:...)`, if you've written it.

## Helpful links

- [Trailhead Xcode Essentials](https://trailhead.salesforce.com/content/learn/modules/xcode-essentials)
- [Storyboards Tutorial by Ray Wenderlich](https://www.raywenderlich.com/464-storyboards-tutorial-for-ios-part-1)

<a href="step3.html" class="btn btn-default pull-right">Next <i class="glyphicon glyphicon-chevron-right"></i></a>
