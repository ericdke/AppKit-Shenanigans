# About Window

AppKit automatically generates an "About" window for your app.

But what if you want to customize it? Two ways.

## Credits.rtf

If you add an RTF file named "Credits.rtf" at the root of your project in Xcode, its content will automatically be added to the about window!

## Make Your Own Window

*Solution by [Nicolas Miari](https://stackoverflow.com/a/35353660/2227743)*

Create a custom "About" window in Interface builder, with all the subviews and labels you want. 

Create a custom NSWindowController subclass as a singleton, initialized with your custom window's nib or storyboard name:

```swift
static let defaultController: AboutWindowController = {
    let storyboard = NSStoryboard(name: NSStoryboard.Name("AboutWindow"), bundle:nil)
    guard let windowController = storyboard.instantiateInitialController() as? AboutWindowController else {
        fatalError("Storyboard inconsistency")
    }
    return windowController
}()
```
Connect your About window's subviews to outlets in the window controller class. 

Also, specify the class for File Owner as your custom NSWindowController subclass and connect the window's "New Referencing Outlet" to File Owner's window property.

Go to MainMenu.xib (or storyboard) in Interface Builder. Remove the action that is wired to the menu item "About ...", and re-wire a new one to the about: method of the placeholder object "First Responder".

Make an IBAction in AppDelegate from this menu item:

```swift
@IBAction func about(_ sender: Any) {
    AboutWindowController.defaultController.window?.orderFront(self)
}
```

In your subclass of NSViewController, do your own content. If you want to access the app's informations:

```swift
class AboutViewController: NSViewController {

    @IBOutlet weak var appNameLabel: NSTextField!
    @IBOutlet weak var appVersionLabel: NSTextField!
    @IBOutlet weak var appCopyrightLabel: NSTextField!
    @IBOutlet weak var appIconImageView: NSImageView!

    override func viewDidLoad() {
        super.viewDidLoad()
        self.appIconImageView.image = NSApp.applicationIconImage
        if let infoDictionary = Bundle.main.infoDictionary {
            self.appNameLabel.stringValue = infoDictionary["CFBundleName"] as? String ?? ""
            self.appVersionLabel.stringValue = infoDictionary["CFBundleShortVersionString"] as? String ?? ""
            self.appCopyrightLabel.stringValue = infoDictionary["NSHumanReadableCopyright"] as? String ?? ""
        }
    }
}
```

