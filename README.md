# React Native Tips and Tricks
Lessons learned - Tips and Tricks for React Native

### [Listviews](#listviews)
* [Using `scrollTo` on a Listview](#using-scrollto-on-a-listview)

### [Screens](#screens)
* [Avoiding the Keyboard with Views](#avoiding-the-keyboard-with-views)

## Listviews
### Using `scrollTo` on a Listview
You can’t call scrollTo on a listview, BUT you can access the scrollview inside a listview and set the scrolling on that. 

In most cases this looks like so: `this.refs.someListView.getScrollResponder().scrollResponderScrollTo(0, 0, true)` for scroll to the top left with animation.

If you've implemented `RefreshControl` for pull to refresh, you can't animate to the top without triggering the Refresh (it looks horrible).   You'll have to go with `this.refs.someListView.getScrollResponder().scrollTo(0, 0)`

## Screens
### Avoiding the Keyboard with Views
Firstly, we need to listen to keyboard events, you'll need to import `DeviceEventEmitter`.  You'll also need to know the screen's full height, so we can subtract out the keyboard.  For this we'll import `Dimensions`.
`import React, {Dimensions, DeviceEventEmitter} from 'react-native'`
which gives us the the ability to subscribe to keyboard events like so:
```javascript

  componentWillMount () {
    DeviceEventEmitter.addListener('keyboardDidShow', this.keyboardDidShow.bind(this))
    DeviceEventEmitter.addListener('keyboardDidHide', this.keyboardDidHide.bind(this))
    this.mounted = true // This is to help stop warnings
  }
```
The `this.mounted` is because keyboard hiding when the screen is dismounted, causes warnings, this part is optional, because it would result in a no-op.   If you're doing this, you'll need to add the folowing:
```javascript
  componentWillUnmount () {
    this.mounted = false
  }
```
For the keyboard functionality, we'll be setting the container view, accordingly:
```javascript
  keyboardDidShow (e) {
    let newSize = Dimensions.get('window').height - e.endCoordinates.height
    this.setState({visibleHeight: newSize})
  }

  keyboardDidHide (e) {
    // Check for mounted to avoid warning
    this.mounted && this.setState({visibleHeight: Dimensions.get('window').height})
  }
```
We contain all the contents of the screen in a single view that changes when the keyboard shows/hides
```javascript
class SomeScreen extends React.Component {
  constructor (props) {
    super(props)
    // be sure to start off full screen height
    this.state = {
      visibleHeight: Dimensions.get('window').height
    }
  }

  // ...
  render () {
    return (
      <View style={{height: this.state.visibleHeight}}>
       ...
      </View>
    )
  }
}
```
