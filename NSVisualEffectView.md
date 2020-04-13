# NSVisualEffectView

Making the background of your window a visual effect view is easy in Interface Builder: grab the object then add it behind the window's view (right on top of the view in the objects list).

But what about doing it programmatically?

## NSVisualEffectView In Code

In the window's view controller:

```swift
override func loadView() {
    super.loadView()
    let v = MainView(frame: .zero)
    view = v
    view.wantsLayer = true
    let visu = NSVisualEffectView(frame: .zero)
    visu.translatesAutoresizingMaskIntoConstraints = false
    visu.blendingMode = .behindWindow
    visu.material = .dark
    visu.state = .active
    view.addSubview(visu)
    NSLayoutConstraint.activate([
        visu.topAnchor.constraint(equalTo: view.topAnchor),
        visu.leadingAnchor.constraint(equalTo: view.leadingAnchor),
        visu.bottomAnchor.constraint(equalTo: view.bottomAnchor),
        visu.trailingAnchor.constraint(equalTo: view.trailingAnchor)
    ])
}
```