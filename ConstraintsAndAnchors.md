## Constraints and Anchors

When you developing on iOS 8 and above, and creating Constraints in code, instead of adding or deleting constraint like this: `self.addConstraint(constraint)`, `self.removeConstraint(constraint)`, use constraint `active` property. This automatically calls `addConstraint` or `removeConstraint` on needed view!
If you need add or delete multiple constraints, use `NSLayoutConstraint.activateConstraints()` or `NSLayoutConstraint.deactivateConstraints()` 
>**Note:** 
* Deactivating constraint or changing it's `constant`property not trigger new layout pass automatically, so after deactivating constraints or changing `constant` property, you need call `setNeedsLayout` and `layoutIfNeeded`. 
*Maybe you don't need to call first method because maybe when you change constraint `active` or `constant` property, this method automatically get called, I don't know. For now safer approach for me to call both methods*
* Activating new constraint automatically trigger new layout pass and calls `updateLayout`so you don't need to write anything(like `setNeedsLayout` etc) after activating new constraint.

Try to use `constraint.identifier` instead of adding property for needed constraint to your class. You will write less code!
