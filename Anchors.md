Anchors (iOS 9 and above)

In iOS 9 Apple introduces the easier syntax to write constraints : **Anchors**

With Anchors syntax like this:
 ```swift
 // Creating constraints using NSLayoutConstraint
NSLayoutConstraint(item: subview,
                   attribute: .Leading,
                   relatedBy: .Equal,
                   toItem: view,
                   attribute: .LeadingMargin,
                   multiplier: 1.0,
                   constant: 0.0).active = true
 
NSLayoutConstraint(item: subview,
                   attribute: .Trailing,
                   relatedBy: .Equal,
                   toItem: view,
                   attribute: .TrailingMargin,
                   multiplier: 1.0,
                   constant: 0.0).active = true
```
becomes like that:
```swift
 
// Creating the same constraints using Layout Anchors
let margins = view.layoutMarginsGuide
 
subview.leadingAnchor.constraintEqualToAnchor(margins.leadingAnchor).active = true
subview.trailingAnchor.constraintEqualToAnchor(margins.trailingAnchor).active = true
```

TODO: Write more about Layout Anchors and Layout Margins
