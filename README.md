## Pilot - Build an IOS app with Swift and the Salesforce Mobile SDK for iOS

### What is this repo?
This repo is the pilot version of an iOS development project with the Salesforce Mobile SDK for iOS. 

### What will I learn?
You'll learn:

* Basics of Swift Development
* Basics of Interface Builder, Storyboards and UIKit development
* Enabling Biometric Authorization (TouchID / FaceID)
* Getting a List of Accounts - SOQL from iOS
* Navigation between Views with Seques & passing data between view controllers
* Using hardware resources - Taking pictures
* Uploading data to Salesforce

### How do I start?
You'll need to clone this repo, but in a slightly different way than normal. Specifically, you'll need to clone this repo and it's submodules. Without the submodules, you won't have the mobile sdk available. Here's how to clone with submodules:

```console
git clone --recurse-submodules https://github.com/codefriar/IOSAndSalesforce.git
```

If you've already cloned this repositiory, without submodules, please use this command to pull them:

```console
git submodule update --init --recursive
```
