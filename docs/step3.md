# Setting up your Project

## Prerequisites <a name="pre"></a>

In order to complete this challenge you'll need:

1. A working installation of Git.
2. A Mac with Xcode 10.2+ installed.
3. If you want to install the sample app on a physical iOS device, you'll need an Apple Developer Account.
4. A Salesforce developer edition org (Signup at [developer.salesforce.com](https://developer.salesforce.com/signup))

> Note: This project tries to be less copy/paste than some. However, the settings below are critical to your success! Please ensure you do clone this repo with submoduels and ensure the callback URL is exactly as it's listed.

## Source Control Setup <a name="download"></a>

This project makes use of [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), in addition to Xcode build dependencies to incorporate the SDK. This means you must not only clone _this_ repository, but the SDK as a submodule repository as well. If you have not yet cloned this repository, this clone command will clone not only this repo, but the submodules as well.

```console
git clone --recurse-submodules https://github.com/codefriar/IOSAndSalesforce.git
```

If you've already cloned this repository, please initialize the submodules using this command

```console
git submodule update --init --recursive
```

## Creating a Connected App

To complete part of this lab you'll need to create a new connected app in your Salesforce Org. Create one now by:

1. Log in to your Dev Hub org.
2. From Setup, enter App Manager in the Quick Find box to get to the Lightning Experience App Manager.
3. In the top-right corner, click New Connected App.
4. Update the basic information as needed, such as the connected app name and your email address.
5. Select Enable OAuth Settings.
6. For the callback URL, enter `testsfdc:///mobilesdk/detect/oauth/done`
7. Add these OAuth scopes:
   1. Access and manage your data (api)
   2. Perform requests on your behalf at any time (refresh_token, offline_access)
   3. Provide access to your data via the Web (web)
8. Under Mobile App Settings check the 'PIN Protect' checkbox.
9. Click Save.
10. Click Continue.
11. When the page loads, find and copy the Consumer Key to a safe place. You'll need this later.

## Opening the project in Xcode

Open the `SFSDKStarter.xcodeproj` file in the root of your clone by double clicking on it.

When your Project opens in Xcode, go ahead and start it building by pressing `cmd-b`. The initial build can take a bit, so lets get that started. The initial build should succeed, if you've followed the cloning steps above.

## Our first change

While the sample project we're starting worth ships with credentials to a serviceable connected app, you did just create your own. The `bootconfig.plist` file you'll find in the SFSDKStarter folder contains the SDK configuration settings. One of which is listed as `remoteAccessConsumerKey`. Go ahead and replace the existing consumer key with the one you copied earlier from your connected app.

### Our first win

That's it. You've now created a connected app, and tied your ios project to that connected app. Let's see what else we can do!

## Switching to Storyboards

> When an iOS app starts up, control passes to the _appDelegate_ class found in `appDelegate.swift`. It's here that the application bootstraps itself.

Open the `appDelegate.swift` file. Here you'll find a wealth of configuration options for how the Salesforce Mobile SDK for iOS functions, including how to enable optional features like push notifications. It's worth taking a few moments and reading through this file's comments to get a feel for what's possible. Feel free to (optionally) uncomment the configuration settings and customize your login view. _If you do that, however, don't forget to uncomment the line that creates the loginViewConfig variable._

Further down in the appDelegate file is a method called `setupRootViewController()`. This method is responsible for creating the first 'screen' the user sees. By default, it's configured to create a simple nav setup with a single element. That's functional, but it bypasses one of the most powerful features of iOS development: Storyboards. To utilize storyboards, we need to replace the contents of `setupRootViewController()`. Let's replace the contents with this:

```swift
self.window!.rootViewController = UIStoryboard(name: "Main", bundle: nil).instantiateInitialViewController()
self.window!.makeKeyAndVisible()
```

These two lines are responsible for loading a storyboard and making the initial scene visible to the user. Note that we're loading the "Main" storyboard; found in the coresponding file named `Main.storyboard`.

## Things you didn't do

You've done a lot of work in this step. That said, there are some things that we bootstrapped for you. Some things because they're outside the scope of what this project is about, and some because, well, the Salesforce Platform takes care of them for you. Specifically, I want to draw your attention to two specific things:

1. You didn't have to do any of the Xcode configuration for incorporating the Mobile SDK. The starter project in this repo bootstraps that for you, so long as you cloned the submodules. Why? Because the vagarities of Xcode configuration and external libraries are out of scope for this lab. Want to learn more? Check out [Creating an iOS Swift Project Manually](https://developer.salesforce.com/docs/atlas.en-us.mobile_sdk.meta/mobile_sdk/ios_new_native_project_manual.htm)
2. One of the key features of the app you're building is biometric authentication. Due to the way the SDK works, you did everything needed to implement Biometric authentication when you checked the 'PIN Protect' checkbox. Now, when your app launches, you'll be asked to provide a PIN as an additional layer of security. If you run your app on a device with TouchID or FaceID, you'll automatically be prompted to use them. Sometimes it's nice to have things just work.

<a href="step4.html" class="btn btn-default pull-right">Next <i class="glyphicon glyphicon-chevron-right"></i></a>
