# Getting a list of Accounts

## Overview

This step is jam packed with functionality. You'll query for data from Salesforce, handle the asynchronous result, and display the data in a table view. This step starts off with a concrete copy/paste step, but in the end you'll need to do this same kind of work a few more times.

## Using the SDK to query for information

The SDK has a number of built in methods that make it easy to create, read, update, destroy and query your Salesforce data. The SDK relies on standard REST API's to accomplish these tasks. This means anytime your Org automations (Triggers, Processes etc.) would normally kick in, apply here as well.

First things first. Before we can interact with Salesforce data we need to create a new scene controller and setup our storyboard. Create a new Swift file by:

1. Click `File > New > File` (or click `cmd-n`).
2. Make sure to choose Swift file.
3. Click Next.
4. Name your file AccountSceneController.
5. Ensure the target checkbox for 'SFSDKStarter' is checked.

By default, Xcode will open your new Swift file as soon as it's created. Add these two lines to the top:

```swift
import UIKit
import SalesforceSDKCore
```

These lines give us access to UIKit, which we'll use for data display, and the Salesforce SDK. For now, save your file.

Open the Main.storyboard. By default, Xcode includes a tab-based navigation controller. You'll also notice a new 'Navigator' style pane. ![Storyboard Navigator](https://codefriar.github.io/IOSAndSalesforce/img/storyboardScenes.png "Storyboard Navigator")
Clicking on each of the scenes and hit your delete key to remove them. We want to start with a blank slate.

## Closures-as-callbacks

## Your first protocol
