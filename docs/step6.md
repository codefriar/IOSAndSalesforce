# Uploading our images to Salesforce

One of the most powerful features of Swift is the ability to extend classes, structs and enums with additional properties and methods after they're defined. As we discovered earlier the SDK has a number of helper methods for building requests, but it doesn't (currently) offer helpers for creating File attachments (Lightning style). Lets write an extension to help with that.

1. Create a new Swift file with the name `RestClient+attachments.swift`. Convention is to name extensions with the name of what is extended - in this case RestClient, + the description of the extenison - in this case attachments.
2. Put this code in your new file:

   ```swift
   import Foundation
   import SalesforceSDKCore
   import UIKit

   extension RestClient {
     func requestForCreatingImageAttachment(from image: UIImage, relatingTo: String, fileName: String? = nil) -> RestRequest {
         let imageData = UIImagePNGRepresentation(image)!
         let uploadFileName = fileName ?? UUID().uuidString + ".png"
         return self.requestForCreatingAttachment(from: imageData, withFileName: uploadFileName, relatingTo: relatingTo)
     }

     private func requestForCreatingAttachment(from data: Data, withFileName fileName: String, relatingTo: String) -> RestRequest {
         let record = ["VersionData": data.base64EncodedString(options: .lineLength64Characters), "Title": fileName, "PathOnClient": fileName, "FirstPublishLocationId": relatingTo]
         return self.requestForCreate(withObjectType: "ContentVersion", fields: record)
     }
   }
   ```

   > Make sure to save your file

Because these two methods are part of an extension, they become instance methods on RestClient! We can now use these two methods to upload our images to Salesforce. In our previous step, we created the
`func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [String : Any]) {... }` method and set it to log that we got a photo. Lets modify that to call our new requesForCreatingImageAttachment method.

1. Remove the log entry, and replace it with this code:
   ```swift
   guard let contactId = self.contactId else {return}
       let attachmentRequest = RestClient.shared.requestForCreatingImageAttachment(from: image, relatingTo: contactId)
       RestClient.shared.send(request: attachmentRequest, onFailure: self.handleError){ result, _ in
           SalesforceLogger.d(type(of: self), message: "Completed upload of photo")
       }
   ```

You'll also need to include the Handle Error named Closure. It Looks like this:

```Swift
private func handleError(_ error: Error?, urlResponse: URLResponse? = nil) {
        let errorDescription: String
        if let error = error {
            errorDescription = "\(error)"
        } else {
            errorDescription = "An unknown error occurred."
        }

        SalesforceLogger.e(type(of: self), message: "Failed to successfully complete the REST request. \(errorDescription)")
    }
```

As we've seen before, the code follows the pattern of creating a request, and then sending it. The secret here, is that a File attachment is just a record! Creating the record is built-in to the SDK, our request method simply needs to prepare the data.

## Try it out

Build your app and run it in the simulator. Attach an image to a contact and see how it works
