# SVG Images

You may want to use SVG files in you macOS app, but, unfortunately, those are not supported natively.

The easiest way is to use the [SVGKit](https://github.com/SVGKit/SVGKit) framework.

Once you've added the framework to your project (Cocoapod or else), my advise is to make an Assets class that holds your SVG images.

Drop an svg file in your project (say, "logo.svg") then:

```swift
import SVGKit

class Assets {
	
    static func logoIcon(color: UIColor) -> SVGKFastImageView {
        return make(name: "logo", color: color)
    }	

    private static func make(name: String, color: UIColor) -> SVGKFastImageView {
        let img = SVGKImage(named: name)!
        let icon = SVGKFastImageView(svgkImage: img)!
        icon.setTintColor(color: color)
        icon.translatesAutoresizingMaskIntoConstraints = false // assuming you're using autolayout programmatically
        return icon
    }

}
```

Then in your view controller:

```swift
import SVGKit

class MyViewController: NSViewController {

	let logo: SVGKFastImageView!

	override func viewDidLoad() {
        super.viewDidLoad()
        logo = Assets.logoIcon(color: .white)
        view.addSubview(logo)
        NSLayoutConstraint.activate([
        	logo.topAnchor.constraint(equalTo: view.topAnchor),
        	logo.leadingAnchor.constraint(equalTo: view.leadingAnchor),
        	logo.widthAnchor.constraint(equalToConstant: 128),
        	logo.heightAnchor.constraint(equalToConstant: 128)
        ])
    }

}
```

