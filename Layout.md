## Layout

###Phases of Display
When something changes in UI, we need to update it.
Auto Layout introduces two additional steps to the process before views can be displayed: updating constraints and laying out views. Each step is dependent on the one before; display depends on layout, and layout depends on updating constraints.

**1. Update Constrants**</br>
Engine updates constraints from Sub View to Super View
>**From The Good Arctice:**
The first step – updating constraints – can be considered a “measurement pass.” It happens bottom-up (from subview to superview) and prepares the information needed for the layout pass to actually set the views’ frame. You can trigger this pass by calling `-setNeedsUpdateConstraints` . Any changes you make to the system of constraints itself will automatically trigger this. However, it is useful to notify Auto Layout about changes in custom views that could affect the layout. Speaking of custom views, you can override
`-updateConstraints` to add the local constraints needed for your view in this phase.

**2. Layout**</br>
Layout updates in the opposite direction, from Super View to Sub View
>**From The Good Article:**
The second step – layout – happens top-down (from super view to subview). This layout pass actually applies the solution of the constraint system to the views by setting their frames (on OS X) or their center and bounds (on iOS). You can trigger this pass by calling `-setNeedsLayout` , which does not actually go ahead and apply the layout immediately, but takes note of your request for later. **This way you don’t have to worry about calling it too often, since all the layout requests will be coalesced into one layout pass.**

>To force the system to update the layout of a view tree immediately, you can call `-layoutIfNeeded` /`- layoutSubtreeIfNeeded `(on iOS and OS X respectively). This can be helpful if your next steps rely on the views’ frame being up to date. In your custom views you can override`-layoutSubviews` /`- layout` to gain full control over the layout pass. We will show use cases for this later on.

**3. Display**</br>
Display draws from SuperView to Sub View too
>Finally, the display pass renders the views to screen and is independent of whether you’re using Auto Layout or not. It operates top-down and can be triggered by calling setNeedsDisplay , which results in a deferred redraw coalescing all those calls. Overriding the familiar drawRect: is how you gain full control over this stage of the display process in your custom views.

----
>Since each step depends on the one before it, the display pass will trigger a layout pass if any layout changes are pending. Similarly, the layout pass will trigger updating the constraints if the constraint system has pending changes.

>It’s important to remember that these three steps are not a one-way street. Constraint-based layout is an iterative process. The layout pass can make changes to the constraints based on the previous layout solution, which again triggers updating the constraints following another layout pass. This can be leveraged to create advanced layouts of custom views, but you can also get stuck in an infinite loop if every call of your custom implementation of layoutSubviews results in another layout pass.

**So, In Conclusion:**
You can send requests to the system to update by calling:
`-setNeedsDisplay`
`-setNeedsLayout`
`-setNeedsUpdateConstraints`

To perform request immediately, you should call:
`-layoutIfNeeded`
