# React Native FBSDK
React Native FBSDK is a wrapper around the iOS Facebook SDK, allowing for Facebook integration in [React Native](https://facebook.github.io/react-native/) apps.  Access to native components, from login to sharing, is provided entirely through documented JavaScript modules so you never have to call a single native function directly.

Functionality is provided through three separate npm packages so you never have to include more than you need:
- `react-native-fbsdkcore`
- `react-native-fbsdkshare`
- `react-native-fbsdklogin`

## Usage
### react-native-fbsdkcore
#### Graph Requests
```js
var FBSDKCore = require('react-native-fbsdkcore');
var {
  FBSDKGraphRequest,
} = FBSDKCore;

// ...

// Create a graph request asking for friends with a callback to handle the response.
var fetchFriendsRequest = new FBSDKGraphRequest((error, result) => {
  if (error) {
    alert('Error making request.');
  } else {
    // Data from request is in result
  }
}, '/me/friends');
// Start the graph request.
fetchFriendsRequest.start();

// ...
```

#### App events
```js
var FBSDKCore = require('react-native-fbsdkcore');
var {
  FBSDKAppEvents,
} = FBSDKCore;

// ...

// Log a $15 purchase.
FBSDKAppEvents.logPurchase(15, 'USD', null, null)

// ...
```
### react-native-fbsdklogin
#### Login Button
```js
var FBSDKLogin = require('react-native-fbsdklogin');
var {
  FBSDKLoginButton,
} = FBSDKLogin;

var Login = React.createClass({
  render: function() {
    return (
      <View>
        <FBSDKLoginButton
          onLoginFinished={(error, result) => {
            if (error) {
              alert('Error logging in.');
            } else {
              if (result.isCanceled) {
                alert('Login cancelled.');
              } else {
                alert('Logged in.');
              }
            }
          }}
          onLogoutFinished={() => alert('Logged out.')}
          readPermissions={[]}
          publishPermissions={['publish_actions']}/>
      </View>
    );
  }
});
```

#### Login Manager
```js
var FBSDKLogin = require('react-native-fbsdklogin');
var {
  FBSDKLoginManager,
} = FBSDKLogin;

// ...

// Attempt a login using the native login dialog asking for default permissions.
FBSDKLoginManager.setLoginBehavior(GlobalStore.getItem('behavior', 'native'));
FBSDKLoginManager.logInWithReadPermissions([], (error, result) => {
  if (error) {
    alert('Error logging in.');
  } else {
    if (result.isCanceled) {
      alert('Login cancelled.');
    } else {
      alert('Logged in.');
    }
  }
});

// ...
```
### react-native-fbsdkshare
#### Share dialogs
All of the dialogs included are used in a similar way, with differing content types.
```js
var FBSDKShare = require('react-native-fbsdkshare');
var {
  FBSDKShareDialog,
  FBSDKShareLinkContent,
} = FBSDKShare;

// ...

// Build up a shareable link.
var linkContent = new FBSDKShareLinkContent('https://facebook.com', 'Wow, check out this great site!', 'Facebook.com', null);
// Share the link using the native share dialog.
FBSDKShareDialog.show(linkContent, (error, result) => {
  if (!error) {
    if (result.isCancelled) {
      alert('Share canceled.');
    } else {
      alert('Thanks for sharing!');
    }
  } else {
    alert('Error sharing.');
  }
});

// ..
```

#### Share API
Your app must have the publish_actions permission approved to share through the share API.
```js
var FBSDKShare = require('react-native-fbsdkshare');
var {
  FBSDKShareAPI,
  FBSDKSharePhoto,
  FBSDKSharePhotoContent,
} = FBSDKShare;

// ...

// Build up a shareable photo, where 'cat.png' is included in the project. A data URI encoding the image can also be passed.
var photo = new FBSDKSharePhoto('cat.png', true);
var photoContent = new FBSDKSharePhotoContent([photo]);
// Share using the share API.
FBSDKShareAPI.share(photoContent, "/me", "Check out this cat!", (error, result) => {
  if (error) {
    alert('Error sharing');
  } else {
    alert('Shared successfully');
  }
});

// ...
```

## Running the Sample App
- From the Sample folder, run `npm install`
- Download and install the [Facebook SDK for iOS](https://developers.facebook.com/docs/ios).
- Open NHSample.xcodeproj
- Drag `FBSDKCoreKit.framework`, `FBSDKLoginKit.framework`, and `FBSDKShareKit.framework` from the ~/Documents/FacebookSDK folder into the Frameworks group in the XCode project navigator.
- Build and run the app to try it out.

## Installation
- Download and install the [Facebook SDK for iOS](https://developers.facebook.com/docs/ios).
- Follow the [getting started guide](https://developers.facebook.com/docs/ios/getting-started/) to link your project with the Facebook SDK frameworks and set up the app delegate.
- Depending on what functionality you're looking to integrate, run any combination of the following:
  - `npm install react-native-fbsdkcore` for graph requests, app events, etc. `FBSDKCoreKit.framework` must be added to the project.
  - `npm install react-native-fbsdkshare` for share buttons, dialogs, etc. `FBSDKCoreKit.framework` and `FBSDKShareKit.framework` must be added to the project.
  - `npm install react-native-fbsdklogin` for login button and manager. `FBSDKCoreKit.framework` and `FBSDKLoginKit.framework` must be added to the project.
- Open the Xcode project for your app.
- Drag the folders prefixed with react-native-fbsdk from node_modules into the XCode project navigator.

### Troubleshooting
- If you get a build error stating that one of the Facebook SDK files was not found -- eg. `FBSDKCoreKit/FBSDKCoreKit.h` -- check two things\:
  - Ensure that the Facebook SDK frameworks have been added to the project.
  - Add the folder where the Facebook SDK was to the project's framework search path in Xcode. See Apple's [documentation on including frameworks](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Tasks/IncludingFrameworks.html).

## License
See the LICENSE file.

## Platform Policy
Developers looking to integrate with the Facebook Platform should familiarize themselves with the [Facebook Platform Policy](https://developers.facebook.com/policy/).
