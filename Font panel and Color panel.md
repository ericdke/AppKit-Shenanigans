# Using the Font panel and the Color panel

If you want to use the Font panel and the Color panel in your NSTextView, make a subclass and implement `changeFont(_ sender: Any?)` and `changeColor(_ sender: Any?)`.

```swift
class MyTextView: NSTextView {

    override func changeFont(_ sender: Any?) {
        guard let fm = sender as? NSFontManager, let currentFont = self.font else {
            return
        }
        let newFont = fm.convert(currentFont)
        self.font = newFont
    }
    
    override func changeColor(_ sender: Any?) {
        guard let cp = sender as? NSColorPanel else {
            return
        }
        let newColor = cp.color
        textColor = newColor
        // inverted for selected text:
        selectedTextAttributes = [NSAttributedString.Key.foregroundColor: backgroundColor, NSAttributedString.Key.backgroundColor: newColor]
    }
    
}
```

These panels are singletons, automatically available to any NSTextView that implement these methods. 

If Xcode didn't connect these panels properly, do it yourself:

The "Format > Font > Show Fonts" menu item must be connected to Font Manager's `orderFrontPanel`.

The "Format > Font > Show Colors" menu item must be connected to First Responder's `orderFrontColorPanel`.