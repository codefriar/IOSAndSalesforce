# Using iOS hardware resources - Taking photos

In this step, we're going to modify the `ContactDetailSceneController.swift` file that you created. Our goal is to add a button to our interface that, when tapped, allows the user to take a photo with the iOS Camera.

> Note, don't worry, the iOS simulator has a built-in set of lovely icelandic pictures that are offered instead.

To do this, we're going to utilize a built-in bit of functionality called `UIImagePickerController`. This requires a delegate class that conforms to the `UIImagePickerControllerDelegate` and `UINavigationControllerDelegate` protocols.

Go ahead and add those two protocols to your ViewController class by appending `, UINavigationControllerDelegate, UIImagePickerControllerDelegate` after UITableViewController. Your final product should look something like this:

```swift
class ContactDetailController : UITableViewController, UINavigationControllerDelegate, UIImagePickerControllerDelegate { ... }
```

Xcode is likely to start showing error icons at this point because we're missing a few methods. Let's add them now.

```swift
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [String : Any]) {
        imagePickerCtrl.dismiss(animated: true, completion: nil)
        if let image = info["UIImagePickerControllerOriginalImage"] as? UIImage {
            upload(image: image)
        }
    }

    func upload(image: UIImage){
        guard let contactId = self.contactId else {return}
        let attachmentRequest = RestClient.shared.requestForCreatingImageAttachment(from: image, relatingTo: contactId)
        RestClient.shared.send(request: attachmentRequest, onFailure: self.handleError){ result, _ in
            SalesforceLogger.d(type(of: self), message: "Completed upload of photo")
        }
    }
```

Additionally, to make it easier to reason about what's happening, let's create an _extension_. In your ContactDetailSceneController file, but _after_ the definition of your class, add the following code:

```swift
extension RestClient {
    func requestForCreatingImageAttachment(from image: UIImage, relatingTo: String, fileName: String? = nil) -> RestRequest {
        let imageData = UIImagePNGRepresentation(image.resizedByHalf())!
        let uploadFileName = fileName ?? UUID().uuidString + ".png"
        return self.requestForCreatingAttachment(from: imageData, withFileName: uploadFileName, relatingTo: relatingTo)
    }

    private func requestForCreatingAttachment(from data: Data, withFileName fileName: String, relatingTo: String) -> RestRequest {
        let record = ["VersionData": data.base64EncodedString(options: .lineLength64Characters), "Title": fileName, "PathOnClient": fileName, "FirstPublishLocationId": relatingTo]
        return self.requestForCreate(withObjectType: "ContentVersion", fields: record)
    }
}
```
