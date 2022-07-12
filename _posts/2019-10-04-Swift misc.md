---
title: Swift MISC
date: 2019-10-04
layout: post
tags: Swift, iOS
---

# Swift MISC

1. Detect motion in viewcontroller

```swift
override func motionEnded(motion: UIEventSubtype, withEvent event: UIEvent?) {
    if motion == .MotionShake {
        self.saveThisSong()
    }
}
```

2. Play system sound

```swift
let systemSoudID:SystemSoundID = 1016
AudioServicesPlaySystemSound(systemSoudID)
```

3. Read and write user default setting

```swift
let defaults = UserDefaults.standard
defaults.set(owner, forKey: RecordField.owner.rawValue)

let defaults = UserDefaults.standard
if let o = defaults.string(forKey: RecordField.owner.rawValue) {
    print (o)
}
```

4. How to use system notification

```swift
// Create notification 
let busstopNotification = Notification.Name("busStopNotification")
let notifydict = ["number": self.number]
NotificationCenter.default.post(name: busstopNotification, object: nil, userInfo: notifydict)

// Response the notification

NotificationCenter.default.addObserver(self, selector: #selector(updateSchedule), name: busstopNotification, object: nil)

func updateSchedule(notification: NSNotification) {
    if let n = notification.userInfo?["number"] as? String {
    }
}

deinit {
    NotificationCenter.default.removeObserver(self, name: NSNotification.Name(rawValue: "UIApplicationDidBecomeActiveNotification"), object: nil)
}
```

5. Random value

```swift
let random = Int(arc4random_uniform(UInt32(mrtStations.count)))
astation  = mrtStations[random]
```

6. Singleton class usage

```swift
class currentUserInfo {
    static let sharedCurrentUserInstance = currentUserInfo()
    var userRealName = ""
    private init() {}
}
let a = currentUserInfo.sharedCurrentUserInstance.userRealName
```

7. Rate app 

```swift
func rateApp(appId:String, completion: @escaping ((_ success: Bool) ->())) {
    guard let url = URL(string: "itms-apps://itunes.apple.com/app/" + appId) else {
        completion(false)
        return
    }
    guard #available(iOS 10, *) else {
        completion(UIApplication.shared.openURL(url))
        return
    }
    if #available(iOS 10.3, *) {
        SKStoreReviewController.requestReview()
        return
    }
    UIApplication.shared.open(url, options: [:], completionHandler: completion)
}
```

8. Select photo from libary

```swift
func addPhoto() {
    imagePicker.allowsEditing = false
    imagePicker.sourceType = .PhotoLibrary
    imagePicker.delegate = self
    presentViewController(imagePicker, animated: true, completion: nil)
}
extension addPhotoViewController:UIImagePickerControllerDelegate,UINavigationControllerDelegate {
    func imagePickerController(picker: UIImagePickerController, didFinishPickingImage image: UIImage, editingInfo: [String : AnyObject]?) {
        self.selectPhoto[self.selectPhotoIndex] = image
        self.photoStatus[self.selectPhotoIndex] = true
        self.dismissViewControllerAnimated(true, completion: nil)
        newphotoCollectionView.reloadData()
    }
    
    func imagePickerControllerDidCancel(picker: UIImagePickerController) {
        dismissViewControllerAnimated(true, completion: nil)
    }
}
```

9. Background dispatch queues

```swift
DispatchQueue.global().async {
      print (message)
}
```

10. Notification

```swift
  // Define identifier
let notificationName = Notification.Name("NotificationIdentifier")

// Register to receive notification
NotificationCenter.default.addObserver(self, 
    selector: #selector(YourClassName.methodOfReceivedNotification), 
    name: notificationName, object: nil)

// Post notification
NotificationCenter.default.post(name: notificationName, object: nil)

// Stop listening notification
NotificationCenter.default.removeObserver(self, name: notificationName, object: nil);
```

11. To start a timer task

```swift
var timer = Timer()
self.timer = Timer.scheduledTimer(timeInterval: 5.0,
                          target: self,
                          selector: #selector(self.checkSensor),
                          userInfo: nil,
                          repeats: true)
func checkSensor() {

}
```

9. Motion

```swift
override func motionEnded(_ motion: UIEventSubtype, with event: UIEvent?) {
    if motion == .motionShake {
        self.saveThisSong()
    }
}
```

10. Send email

```swift
func sendEmail() {
    if MFMailComposeViewController.canSendMail() {
        let mail = MFMailComposeViewController()
        mail.mailComposeDelegate = self
        mail.setToRecipients(["sgbusstop@gmail.com"])
        mail.setSubject("Feed back email")
        mail.setMessageBody("Your feedback", isHTML: true)
        present(mail, animated: true)
    }
}
```