# NSTextField

To display simple text that doesn't need user interaction, you're supposed to use an NSTextField.

But this is a weird beast: for example, try to set its background color to "NSColor.clear" to make the background transparent, as you would do with another object... it doesn't work.

The solution is to set these three attributes on the text field:

```swift
myTextField.drawsBackground = true
myTextField.isEditable = false
myTextField.isBezeled = false
```

And now this will work:

```swift
myTextField.backgroundColor = .clear
myTextField.textColor = some color
```

So, why do we have to set these three exact things to make it work?! Well, NSTextField is ancient. It comes from NextStep and hasn't change a lot since. And it looks like that in NextStep, non-layered editable text fields with bezel couldn't have transparent/colored background. So here we are today... :p