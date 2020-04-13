# NSUnderlineStyle

It's easy to add change the color or font style of a text (for example the title of an NSButton) by using NSAttributedString.

But what if you want to add an underline to this text? Use NSUnderlineStyle, right? Except it mysteriously crashes, evey time. Try it: add an attribute of `NSUnderlineStyle.single`, and watch your app go down in fire.

The solution is to use `NSUnderlineStyle.single.rawValue`. Yep. Why? Don't ask. Just do it.

Also note that sometimes the length of an attributed text can be different from the length of the text itself, because reasons. So for NSRange, better use the attributed one once it's set.

```swift
class MyButton: NSButton {
    init(title: String, enabled: Bool) {
    super.init(frame: .zero)
    self.title = title
    self.font = NSFont.systemFont(ofSize: 16)
    self.translatesAutoresizingMaskIntoConstraints = false // if doing autolayout by code
    self.isBordered = false
    self.focusRingType = .none

    let attrTitle = NSMutableAttributedString(string: self.title)
    attrTitle.setAttributes([NSAttributedString.Key.backgroundColor : NSColor.clear, NSAttributedString.Key.foregroundColor: Config.shared.orangeColor], range: NSRange(location: 0, length: (title as NSString).length))
    self.attributedTitle = attrTitle

    let attrUnder = NSMutableAttributedString(attributedString: self.attributedTitle)
    attrUnder.addAttribute(NSAttributedString.Key.underlineStyle, value: NSUnderlineStyle.single.rawValue, range: NSRange(location: 0, length: (attributedTitle.string as NSString).length))
    attrUnder.addAttribute(NSAttributedString.Key.underlineColor, value: Config.shared.orangeColor, range: NSRange(location: 0, length: (attributedTitle.string as NSString).length))
    self.attributedTitle = attrUnder
    }
}
```