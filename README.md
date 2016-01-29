# React Native Tips and Tricks
Lessons learned - Tips and Tricks for React Native

### [Listviews](#listviews)
* [Using `scrollTo` on a Listview](#using `scrollTo` on a Listview)

### [Screens](#screens)

* [Avoiding the Keyboard with Views](#avoiding-the-Keyboard-with-views)

## Listviews
### Using `scrollTo` on a Listview
You can’t call scrollTo on a listview, BUT you can access the scrollview inside a listview and set the scrolling on that. 

In most cases this looks like so: `this.refs.someListView.getScrollResponder(). scrollResponderScrollTo(0, 0, true)` for scroll to the top left with animation.

If you've implemented `RefreshControl` for pull to refresh, you can't animate to the top without triggering the Refresh (it looks horrible).   You'll have to go with `this.refs.someListView.getScrollResponder().scrollWithoutAnimation(0,0)`

## Screens
### Avoiding the Keyboard with Views
