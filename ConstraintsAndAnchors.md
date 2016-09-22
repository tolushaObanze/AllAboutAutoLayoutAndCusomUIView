## Constraints and Anchors
### Creating Constraints

Constraints setting up dependencies with two objects</br>
Constraints represent **`NSLayoutConstraint`** class

To create a Constraint between two objects, use:
`let constraint = NSLayoutConstraint(item: firstObject, attribute: .NSLayoutAttibute, relatedBy: NSLayoutRelation, toItem: secondObject, attribute: .NSLayoutAttribute, multiplier: 1.0, constant: 0.0)`

### Activating and Deactivating Constraints
In iOS 8 and above, you activate or deactivate constraint like that: `constraint.isActive = true // or false`. This automatically calls `addConstraint` or `removeConstraint` on needed view! In versions prior to iOS 8, you need manually call `addConstraint` or `removeConstraint` on needed view.

If you need activate or deactivate multiple constraints, use `NSLayoutConstraint.activateConstraints()` or `NSLayoutConstraint.deactivateConstraints()` 

>**Note:** 
* Deactivating constraint or changing it's `constant`property not trigger new layout pass automatically, so after deactivating constraints or changing `constant` property, you need call `setNeedsLayout` and `layoutIfNeeded`.</br> 
*Maybe you don't need to call first method because maybe when you change constraint `active` or `constant` property, `setNeedsLayout` get called automatically, I don't know. For now safer approach for me to call both methods*</br>
* Activating new constraint automatically trigger new layout pass and calls `updateLayout`so you don't need to write anything(like `setNeedsLayout` etc) after activating new constraint.

When you call `removeFromSuperview()` method on your particular view, all constraints also removed!
>**From Apple Docs:**
*Calling this method removes any constraints that refer to the view you are removing, or that refer to any view in the subtree of the view you are removing.*

So, when you recreate the view again, you need to re-add constraints.

### Constraints Priority
To all Constraints you can specify priority.
This helped you in situations , when you have conflicting constraints

### Constraints Animation

TODO: Write about animations
