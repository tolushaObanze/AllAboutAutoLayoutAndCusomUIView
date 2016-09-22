##Custom View / Control Tips And Trics

###Tip 2 - About Constraints 
When you developing on iOS 8 and above, and creating Constraints in code, instead of adding or deleting constraint like this: `self.addConstraint(constraint)`, `self.removeConstraint(constraint)`, use constraint `active` property. This automatically calls `addConstraint` or `removeConstraint` on needed view!
If you need add or delete multiple constraints, use `NSLayoutConstraint.activateConstraints()` or `NSLayoutConstraint.deactivateConstraints()` 
>**Note:** 
* Deactivating constraint or changing it's `constant`property not trigger new layout pass automatically, so after deactivating constraints or changing `constant` property, you need call `setNeedsLayout` and `layoutIfNeeded`. 
*Maybe you don't need to call first method because maybe when you change constraint `active` or `constant` property, this method automatically get called, I don't know. For now safer approach for me to call both methods*
* Activating new constraint automatically trigger new layout pass and calls `updateLayout`so you don't need to write anything(like `setNeedsLayout` etc) after activating new constraint.

Try to use `constraint.identifier` instead of adding property for needed constraint to your class. You will write less code!

###Tip 3 - About QuartzCore animatable properties
If you need to animate properties, that regular UIView animations can't animate, you need to override following method in your view
```Swift
override func actionForLayer(layer: CALayer, forKey event: String) 
    -> CAAction? {
        if event == "cornerRadius" {
            if needAnimate {
                //print("")
                let cornerRadiusAction = CABasicAnimation(keyPath: "cornerRadius")
                cornerRadiusAction.timingFunction = timingFunction
                cornerRadiusAction.duration = animationDuration
                
                self.layer.addAnimation(cornerRadiusAction, forKey: "cornerRadius")
                needAnimate = false
            }
        }
        return super.actionForLayer(layer, forKey: event)
    }
```
This method overrides QuartzCore method actionForLayer. 
Make additional properties for your class, to setup animation:
```swift
//For Animation Purposes
    var needAnimate = false //Trigger, if true next corner radius change
    //will be animatable, after animation, trigger will be false
    var animationDuration : CFTimeInterval = 0.5
    var timingFunction : CAMediaTimingFunction =
        CAMediaTimingFunction(name: kCAMediaTimingFunctionEaseOut)
```
The example above shows, how to animate circleRadius change, when frame of the view changes.

 **Limitations:**
 Outler animation duration and animation curve must be identical with inner properties of your class. And also no spring animations supported. So you need to point it out in your custom control documentation. Example animation setup:
```swift
//Pre Animation Setup
        let animationDuration = 3.0
        customView.animationDuration = animationDuration
        customView.timingFunction = CAMediaTimingFunction(
          name: kCAMediaTimingFunctionEaseOut)
        
        UIView.animateWithDuration(animationDuration, 
          delay: 0.0, options: .CurveEaseOut, animations: { () -> Void in
            //New width of the customView
            self.widthConstraint.constant = 150
            self.headerView.layoutIfNeeded()
            }) { (completed: Bool) -> Void in
        }
```

###Tip 4 - About Constraint Animation
There are some questions about whether the constraint should be changed before the animation block, or inside it.

The following is a Twitter conversation between Martin Pilkington who teaches iOS, and Ken Ferry who wrote Auto Layout. Ken explains that though changing constants outside of the animation block may currently work, it's not safe and **they should really be change inside the animation block.** https://twitter.com/kongtomorrow/status/440627401018466305

###Tip 5
If we need, that your subview has aspect ratio to self view, (like subview.width = self.width / 5), Support animations and not break constraints.
Instead of adding top, bottom, trailing, and leading constraints to self and then in layoutSubviews calculate properly constant (this will go to issue of breaking constraint when you start to animate). You need to add constraints this constraints: centerX, centerY, widthEqualHeight and the last most important one:
```swift
let aspectRatioLabelWidthSelfWidthConstraint = NSLayoutConstraint(
            item: imageView,
            attribute: .Width,
            relatedBy: .Equal,
            toItem: self,
            attribute: .Width,
            multiplier: imageInsects,
            constant: 0)
```
### Tip 6
In you need to create complicated custom view, try to break it on smaller you custom subviews and then connect everything.
### Tip 7 - About Tag and Identifier
If you have many subviews of your view, and need to indicate them. Use `tag` or add `identifier` property to them
