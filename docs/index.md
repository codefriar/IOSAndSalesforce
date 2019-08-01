### What is this repo?

This repo is the pilot version of an iOS development project with the Salesforce Mobile SDK for iOS.

### What will I learn?

You'll learn:

- Basics of Swift Development
- Basics of Interface Builder, Storyboards and UIKit development
- Setting up our project
- Getting a List of Accounts - SOQL from iOS
- Using hardware resources - Taking pictures
- Uploading data to Salesforce

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

[get started](https://codefriar.github.io/IOSAndSalesforce/step1)
