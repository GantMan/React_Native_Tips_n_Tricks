# React Native Tips and Tricks
Lessons learned - Tips and Tricks for React Native

### Using scrollTo on a Listview
You can’t call scrollTo on a listview, BUT you can access the scrollview inside a listview and set the scrolling on that.  `this.refs.someListView.getScrollResponder(). scrollResponderScrollTo(0, 0, true)` will scroll to the top left with animation.
