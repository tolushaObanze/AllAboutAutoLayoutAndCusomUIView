###Action / Target
If you need your view to send standart events like value changed or touchUpInside, subclass UIControl instead of UIView.
And write this when some event happened in your control: `self.sendActionsForControlEvents(.TouchUpInside)`
Add Gesture Recognizers to your control for easily handling user interactions.

###Delegate
If you need more complicated events, use delegate pattern (see the UITableView as an example).

###Swift Closures
As oppisite to delegate pattern, you can use swift closures (or Objective-C blocks).</br>
**Example:**
```swift
class CustomView : UIView {
    typealias ValueChangedClosure = (_ newValue : Int?) -> Void
    
    private var centerLabel : UILabel!
    
    var valueChangedClosure : ValueChangedClosure!
    
    func doSomething() {
    
      //ValueChanged Event Happend
      if valueChangedClosure != nil {
        valueChangedClosure(newValue)
      }
    }
}

class YourViewController : UIViewController {
    var yourCustomView : YourCustomView!
    
    func viewDidLoad() {
      //1. Init Your Custom View
      //...
      //2. Setup Event handling closure
      yourCustomView.valueChangedClosure = {[unowned self](value) in
        print("Value Changed \(value)")
        self.someTextLabel.text = value
      }
    }
}
```
