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

By default, Xcode will open your new Swift file as soon as it's created. Add these two lines just below the default `import Foundation` statment at the top:

```swift
import UIKit
import SalesforceSDKCore

class AccountSceneController: UITableViewController {
  var dataRows = [Dictionary<String, Any>]()

}
```

The first thing we do in this class, is create an array of dictionaries. Why? This is going to hold our account rows we get back from Salesforce. Each account we get back will be a dictionary (in Apex we call these Maps) of key (field name), value pairs. We need an array of these dictionaries because we expect to get more than one account back.

> Now's a good time to save this file.

These lines give us access to UIKit, which we'll use for data display, and the Salesforce SDK. For now, save your file.

Open the Main.storyboard. By default, Xcode includes a tab-based navigation controller. You'll also notice a new 'Navigator' style pane.

![Storyboard Navigator](https://codefriar.github.io/IOSAndSalesforce/img/storyboardScenes.png "Storyboard Navigator")

Clicking on each of the scenes and hit your delete key to remove them. We want to start with a blank slate.

Once you've got a blank slate, click the library button and search for navigation. You should see this:

![Library](https://codefriar.github.io/IOSAndSalesforce/img/addNavigationController.png "add Navigation Controller")

Click on, and drag the navigation controller to your storyboard. Congrats, you've just started your User Interface. Adding a Navigation Controller gives you the ability to add additional scenes, and navigate between them. Looking carefully, you'll see that adding the Navigation Controller actually put two objects on the storyboard canvas. The first is the Navigation Controller itself, and the second, on the right, is the first scene. This is the initial scene that users will see when your app boots. By default that initial scene is a UITableView. We need to link the storyboard scene to the Swift class we just created.

This is pretty easy to do in Xcode, but it can be a bit confusing for new Xcode users because Xcode packs so much funcationality into it's user interface. Here's how to do it.

1. While you have the Main storyboard open, use the scene navigator, and select `Root View Controller`.
2. Using the right hand navigator, select the `Identity Inspector` - the icon looks like a little drivers license.
3. The top section is titled 'Custom Class' and has a 'class' drop down. Using that drop down, select AccountSceneController. Note, if you don't see your AccountSceneController listed, make sure you've saved your file and that you've coded your class to conform to the `UITableViewController` protocol.

![Identity Inspector](https://codefriar.github.io/IOSAndSalesforce/img/changeAssociatedController.png "Change the assocaited controller class")

That'ts it. You just used Interface Builder to drag and drop a Navigation Controller into your app! Additionally, you tied your new scene to your custom class! Now, lets make it do something interesting!

## Walking through the class logic

For this step, we're providing you the class logic, but we're going to do it one function at a time so we can walk through it.

### LoadView()

```swift
// MARK: - View lifecycle
override func loadView() {
    super.loadView()
    self.title = "Accounts"
    let request = RestClient.shared.request(forQuery: "SELECT Id, Name FROM Account LIMIT 10")

    RestClient.shared.send(request: request, onFailure: { (error, urlResponse) in
        SalesforceLogger.d(type(of:self), message:"Error invoking: \(request)")
    }) { [weak self] (response, urlResponse) in

        guard let strongSelf = self,
            let jsonResponse = response as? Dictionary<String,Any>,
            let result = jsonResponse ["records"] as? [Dictionary<String,Any>]  else {
                return
        }

        SalesforceLogger.d(type(of:strongSelf),message:"Invoked: \(request)")

        DispatchQueue.main.async {
            strongSelf.dataRows = result
            strongSelf.tableView.reloadData()
        }
    }
}
```

`loadView()` is a lifecycle method. This means that whenever iOS loads a view, it's associated controller's lifecycle methods are automatically run in a prescribed order. If you'd like to know more about the View Controller lifecyle methods, [click here](https://medium.com/@amyjoscelyn/the-life-cycle-of-a-view-c98f296fd84e) For now, it's important to understand that `loadView()` acts as a kind of constructor, called immediately as the view loads.

It's in this `loadView()` method that the app will reach out to Salesforce for data. Specifically, it's going to query for 10 Accounts. In this case, it's accomplished in three distinct steps.

1. Creating a request. `let request = RestClient.shared.request(forQuery: "SELECT Id, Name FROM Account LIMIT 10")`. The SDK has a number of helper methods for generating requests. Discovering what helpers are avaialble is as simple as letting Xcode autocomplete for you.

![XCode Autocomplete](https://codefriar.github.io/IOSAndSalesforce/img/requestAutocomplete.png "Shows Xcode autocomplete for request methods")

2. Execute the request. Because we have no idea how strong the network signal is, we can never assume how quickly -- or if -- an API request will complete. To prevent API requests from locking up the app, all API requests are asynchronous. In Swift, that means using completion callbacks. In most cases these are closures, and with regards to the SDK, there's usually two: an error closure and a success or completion closure.

```swift

    // MARK: - Table view data source
    func numberOfSectionsInTableView(tableView: UITableView) -> Int {
        return 1
    }

    override func tableView(_ tableView: UITableView?, numberOfRowsInSection section: Int) -> Int {
        return self.dataRows.count
    }

    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cellIdentifier = "AccountNameCellIdentifier"

        // Dequeue or create a cell of the appropriate type.
        let cell: UITableViewCell = tableView.dequeueReusableCell(withIdentifier:cellIdentifier) ?? UITableViewCell(style: .subtitle, reuseIdentifier: cellIdentifier)

        // If you want to add an image to your cell, here's how.
        let image = UIImage(named: "icon.png")
        cell.imageView?.image = image

        // Configure the cell to show the data.
        let obj = dataRows[indexPath.row]
        cell.textLabel?.text = obj["Name"] as? String

        // This adds the arrow to the right hand side.
        cell.accessoryType = UITableViewCell.AccessoryType.disclosureIndicator
        return cell
    }

    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if segue.identifier == "ViewContacts" {
            let destination = segue.destination as! ContactListController
            let cell = sender as! UITableViewCell
            let indexPath = self.tableView.indexPath(for: cell)!
            if let accountName = self.dataRows[indexPath.row]["Name"] as? String {
                destination.name = accountName
            }
            if let accountId = self.dataRows[indexPath.row]["Id"] as? String {
                destination.accountId = accountId
            }
        }
    }
```

## Closures-as-callbacks

## Your first protocol
