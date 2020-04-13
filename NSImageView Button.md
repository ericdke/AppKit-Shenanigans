# NSImageView as a button

Sure, to make a clickable button, you can use NSButton. But NSButton is weird, and sometimes you just want to be able to to do something when an image is clicked.

## Mouse Down

Easy: subclass NSImageView, and on `mouseDown(with event: NSEvent)`, do whatever you want.

```swift
class MyImageButton: NSImageView {

    var someDelegate: SomeDelegate?
    
    override init(frame frameRect: NSRect) {
        super.init(frame: frameRect)
        translatesAutoresizingMaskIntoConstraints = false // if using autolayout by code
        wantsLayer = true
        layer?.cornerRadius = 0
        isEnabled = true
        image = NSImage(named: "someImage")
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    override func mouseDown(with event: NSEvent) {
        super.mouseDown(with: event)
        someDelegate?.doSomething()
    }
    
}
```