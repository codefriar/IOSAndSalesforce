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
            SalesforceLogger.e(type(of: self), message: "Got a Photo. \(errorDescription)")
        }
    }

```

Functionally, the first method specifies what happens once the user has said "Ok, I'm done taking photos." and in our case we want to upload the file. We'll get to uploading the photos in our next step, but for now, lets just log that we got a picture.

Ok, we've got our delegate methods setup, but how do we actually launch the image picker? First, we'll need to add a Navigation Item, and a Button to our interface.

1. Open the main storyboard, and navigate to your ContactDetailScene.
2. Using the Library drag a Navigation Item to the top of your scene. It will default to "Title" and should look like this, once added.
   ![Added a Navigation Item](https://codefriar.github.io/IOSAndSalesforce/img/addedNavigationItem.png "Added Navigation Item")
3. Using the Library, drag a Button to the right of the navigation item.
4. Useing the right side navigator's attributes pane, use the drop-down for "System Item" to select "Camera". Afterwards, it should look like this:
   ![With A Camera Button](https://codefriar.github.io/IOSAndSalesforce/img/cameraButton.png "Added a Camera Button")
5. Open your Swift class file and add this new function to the class:

   ```swift
   @IBAction func didTapPhotoButton(_ sender: Any){
       imagePickerCtrl = UIImagePickerController()
       imagePickerCtrl.delegate = self

       if UIImagePickerController.isSourceTypeAvailable(.camera) {
           imagePickerCtrl.sourceType = .camera
       } else {
           // Device camera is not available. Use photo album instead.
           imagePickerCtrl.sourceType = .savedPhotosAlbum
       }

       present(imagePickerCtrl, animated: true, completion: nil)
   }
   ```

   This function is annotated with @IBAction or Interface Builder Action. This means we can tie a button tap to invoking this function. Lets do that now. In the Main storyboard, use the storyboard navigator to select the top-level ContactDetailScene, and then view the right-side Navigator's Connections Inspector.
   Find the received actions section, and Click-and-drag from the circle next to didTapPhotoButton to the camera button you just added.

With this done, save and run your app. You should be able to click the camera button and launch the UIImagePickerController to take or select a photo.

<a href="step6.html" class="btn btn-default pull-right">Next <i class="glyphicon glyphicon-chevron-right"></i></a>
