## Constraints and Anchors
### Creating Constraints

Constraints setting up dependencies with two objects

Constraints represent **`NSLayoutConstraint`** class


To create a Constraint between two objects, use:
`let constraint = NSLayoutConstraint(item: firstObject, attribute: .NSLayoutAttibute, relatedBy: NSLayoutRelation, toItem: secondObject, attribute: .NSLayoutAttribute, multiplier: 1.0, constant: 0.0)`

### Activating and Deactivating Constraints
In iOS 8 and above, you activate or deactivate constraint like that: `constraint.isActive = true // or false`. This automatically calls `addConstraint` or `removeConstraint` on needed view! In versions prior to iOS 8, you need manually call `addConstraint` or `removeConstraint` on needed view.

If you need activate or deactivate multiple constraints, use `NSLayoutConstraint.activateConstraints()` or `NSLayoutConstraint.deactivateConstraints()` 




To activate constraint on (iOS 8 and above) you need: `leadingConstraint.isActive = true`
> In versions prior to iOS 8 you need to install constraint on the view like that: `view.addConstraint(leadingConstraint)`.</br>
Example:
Suppose you have v1 - view, v2 - v1 subview, v3 - v1 subview.
1. If you install constraint from v2 to v1, you must add constraint to v1
2. If you install constraint from v2 to v3, you must add constraint to v1

When you developing on iOS 8 and above, and creating Constraints in code, instead of adding or deleting constraint like this: `self.addConstraint(constraint)`, `self.removeConstraint(constraint)`, use constraint `active` property. This automatically calls `addConstraint` or `removeConstraint` on needed view!
If you need add or delete multiple constraints, use `NSLayoutConstraint.activateConstraints()` or `NSLayoutConstraint.deactivateConstraints()` 
>**Note:** 
* Deactivating constraint or changing it's `constant`property not trigger new layout pass automatically, so after deactivating constraints or changing `constant` property, you need call `setNeedsLayout` and `layoutIfNeeded`. 
*Maybe you don't need to call first method because maybe when you change constraint `active` or `constant` property, this method automatically get called, I don't know. For now safer approach for me to call both methods*
* Activating new constraint automatically trigger new layout pass and calls `updateLayout`so you don't need to write anything(like `setNeedsLayout` etc) after activating new constraint.

Try to use `constraint.identifier` instead of adding property for needed constraint to your class. You will write less code!
