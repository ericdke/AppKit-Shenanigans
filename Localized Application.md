# Localized Application

Say you've made an app where everything is in English, and now you have to also provide a version for French.

Here are the essential steps.

## Storyboards and Xibs

Select your project (top left of the main window) then check the "Use Base Internationalization" box.

Just above it, in the "Localizations" field, click the "+" sign and add the new language (French in our example).

Xcode will automatically create localized files for your storyboards (or xibs).

But be careful: once this is done, if you add new objects in the storyboards and xibs, Xcode will not update the strings files, so you will have to add the new entries yourself.

## Text declared in code

Now you have to create Localization files for the text that's declared in your code, such as:

```swift
myLoginButton.title = "Click here"
```

Go to menu "File > New > File", choose "Strings File", name it "Localizable.strings", and save it at the root of your project.

Once saved, select the file, and in the info panel click on "Localize" and check the new language box. Xcode creates two files, one for English, and another one for the new language.

Now you have to fill up these files with localized text, and use `NSLocalizedString` in the code.

My advice is to use specific keys for the entries, not the actual text (just like what Xcode does for storyboards and xibs).

For example, in "Localizable.strings (Base)":

	"loginButton.title" = "Click here";

And in "Localizable.strings (French)":

	"loginButton.title" = "Cliquez ici";

Be careful to not forget the semicolon at the end of each line in the strings files, otherwise nothing will work.

Then in your code:

```swift
myLoginButton.title = NSLocalizedString("loginButton.title", comment: "")
```

At runtime, depending of the system language, the app will display the correct version of the text.

It's a bit tedious to type this every time for every object, so I like to make a String extension:

```swift
extension String {

    func localized() -> String {
        return NSLocalizedString(self, comment: "")
    }

}
```

Then in your code:

```swift
myLoginButton.title = "loginButton.title".localized()
```

To avoid typos and having to remember the exact keys, you can make a class with static values for explicit namings:

```swift
class Literals {

	static let loginButtonTitle = "loginButton.title".localized()

}
```

Then in your code:

```swift
myLoginButton.title = Literals.loginButtonTitle
anotherLoginButtonElsewhere.title = Literals.loginButtonTitle
```

This way you only have to type the string key once, in the Literals class. 

For testing while developing, you don't have to actually change your system's language, just do it in Xcode: select "Edit Scheme", then select "Run" and choose the appropriate language in the "Application Language" popup menu.

---

Using strings files like this creates two levels of indirection, but it's much better and safer this way. If you don't like it, just use the strings files with literal content:

In "Localizable.strings (Base)":

	"Click here" = "Click here";

And in "Localizable.strings (French)":

	"Click here" = "Cliquez ici";

And in code:

```swift
myLoginButton.title = NSLocalizedString("Click here", comment: "")
```

But I really don't recommend it, it's not safe at all, typos will happen and issues that are hard to debug will make you lose precious time.