# Menu "Open Recent"

If your app is a Document-based app, you benefit from this menu automatically, as well as the other items in this menu.

But what about apps that are not Document-based?

## Open Recent

Make a subclass of NSDocumentController.

In this subclass, override `maximumRecentDocumentCount`.

```swift
class MyDocumentController: NSDocumentController {

    override var maximumRecentDocumentCount: Int {
        return 10
    }

}
```

To actually open a file from the Recent menu item, you must implement `application(_ sender: NSApplication, openFile filename: String)` in your AppDelegate.

To redirect this to any part of your app, you can use a delegate of your own.

```swift
protocol MenuDelegate {
    func open(path: String)
}
```

In AppDelegate:

```swift
var menuDelegate: MenuDelegate?

func application(_ sender: NSApplication, openFile filename: String) -> Bool {
    menuDelegate?.open(path: filename)
    return true
}
```

In ViewController:

```swift
var appDelegate: AppDelegate?
let myDocumentController = MyDocumentController()

override func viewDidAppear() {
    super.viewDidAppear()
    if let appDel = NSApplication.shared.delegate as? AppDelegate {
    	appDelegate = appDel
        appDelegate?.menuDelegate = self
    }
}
```

When reading a file (from an NSOpenPanel, or drag&drop), pass its URL to the NSDocumentController subclass with `noteNewRecentDocumentURL`.

```swift
extension ViewController: MenuDelegate {
    func open(path: String) {
        // make an URL from the path, then
        myDocumentController.noteNewRecentDocumentURL(url)
        // then use the file
    }
}
```
