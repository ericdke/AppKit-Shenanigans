# Drop a file on a view

Of course the classic example is to drop an image on NSImageView to change its content.

But what if you want to drop any specific type of file on any view?

## Drop any file on any NSView

Subclass NSView. 

Pass the type of drop you want to watch to `registerForDraggedTypes`.

In `draggingEntered(_ sender: NSDraggingInfo)`, check if the dropped file extension is one you accept.

Then pass the dropped file URL to a delegate (or NSNotification, or anything you want).

```swift
protocol DragDropDelegate {
    func dropped(path: String)
}

class MainView: NSView {

    var delegate: DragDropDelegate?
    let allowed = ["txt", "md", "markdown", "rtf"]
    let type = NSPasteboard.PasteboardType("NSFilenamesPboardType")

    override init(frame frameRect: NSRect) {
        super.init(frame: frameRect)
        registerForDraggedTypes([type])
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    override func draggingEntered(_ sender: NSDraggingInfo) -> NSDragOperation {
        if checkExtension(sender) {
            return .copy
        }
        return []
    }
    
    override func performDragOperation(_ sender: NSDraggingInfo) -> Bool {
        guard let board = sender.draggingPasteboard.propertyList(forType: type) as? [String], let path = board.first
        else { return false }
        
        delegate?.dropped(path: path)
        return true
    }
    
    private func checkExtension(_ drag: NSDraggingInfo) -> Bool {
        guard let board = drag.draggingPasteboard.propertyList(forType: type) as? [String], let path = board.first
        else { return false }

        let suffix = URL(fileURLWithPath: path).pathExtension
        return allowed.contains(suffix.lowercased())
    }
    
}
```