# iOS Installation

## 1. Firebase Installation

### 1.1. Setup GoogleService-Info.plist

A `GoogleService-Info.plist` file contains all of the information required by the Firebase iOS SDK to connect to your Firebase project. To automatically generate the plist file, [follow the instructions](https://firebase.google.com/docs/ios/setup#add_firebase_to_your_app) on the Firebase console to "Add Firebase to your app".

Once downloaded, add the file to your iOS app using 'File > Add Files to "\[YOUR APP NAME]"...' in XCode.

### 1.2. Initialise Firebase

> The following instructions are also described on the Firebase console when going through the wizard.

To initilize the native SDK in your app, add the following to your `ios/[YOUR APP NAME]/AppDelegate.m` file:

A) At the top of the file:

```objectivec
#import <Firebase.h>
```

B) At the beginning of the `didFinishLaunchingWithOptions:(NSDictionary *)launchOptions` method add the following line:

```objectivec
[FIRApp configure];
```

?> It is recommended to add the line within the method **BEFORE** creating the **RCTRootView**. Otherwise the initialization can occur after already being required in your JavaScript code - leading to `app not initialised` exceptions.

### 1.3. Install Firebase Library

#### Option 1: Cocoapods (Recommended)

Firebase recommends using Cocoapods to install the Firebase SDK.

##### 1.3.0. If you don't already have Cocoapods set up
Follow the instructions to install Cocoapods and create your Podfile [here](https://firebase.google.com/docs/ios/setup#add_the_sdk).

> The Podfile needs to be initialised in the `ios` directory of your project. Make sure to update cocoapods libs first by running `pod update`

[collapse Troubleshooting]

1) When running `pod install` you may encounter an error saying that a `tvOSTests` target is declared twice. This appears to be a bug with `pod init` and the way that react native is set up.

**Resolution:**
- Open your Podfile
- Remove the duplicate `tvOSTests` target nested within the main project target
- Re-run `pod install`.

---

2) When running `pod install` you may encounter a number of warnings relating to `target overrides 'OTHER_LDFLAGS'`.

**Resolution:**
- Open Xcode
- Select your project
- For each target:
-- Select the target
-- Click Build settings
-- Search for `other linker flags`
-- Add `$(inherited)` as the top line if it doesn't already exist
- Re-run `pod install`

---

3) When running `pod install` you may encounter a warning that a default iOS platform has been assigned.  If you wish to specify a different minimum version:

**Resolution**
- Open your Podfile
- Uncomment the `# platform :ios, '9.0'` line by removing the `#` character
- Change the version as required

---

4) After installation you encounter an error like `RNFirebase core module was not found natively on iOS`.

**Resolution**
- It's most likely you did not run `pod update` to get the latest pod versions
- Run `pod update`
- You should see updated versions of your pods installed
- You may need to re-run `pod install`

[/collapse]

##### 1.3.1. Check the Podfile platform version
We recommend using a minimum platform version of at least 9.0 for your application to ensure that the correct version of the Firebase libraries are used.  To do this, you need to uncomment or make sure the following line is present at the top of your `Podfile`:

```ruby
platform :ios, '9.0'
```

##### 1.3.2. Add the required pods
Simply add the following to your `Podfile` either at the top level, or within the main project target:

```ruby
# Required by RNFirebase
pod 'Firebase/Core', '~> {{ ios.firebase.core }}'
```

Run `pod install`.

> You need to use the `ios/[YOUR APP NAME].xcworkspace` instead of the `ios/[YOUR APP NAME].xcproj` file from now on.

#### Option 2: Manual frameworks (Not Recommended)

1. Download the Firebase SDK zip. Check the react-native-firebase versioning table here: https://github.com/invertase/react-native-firebase/tree/v5.x.x#supported-versions---react-native--firebase then download the correct firebase-ios-sdk release from here: https://github.com/firebase/firebase-ios-sdk/releases/

2. Unzip the Firebase-x.x.x.zip file.

3. Note: Here instead of following the readme inside the Firebase folder you downloaded follow the below steps.

3.1. Create a Firebase folder in `{your-project}/ios/Firebase`.

3.2. Copy the folder Analytics and all other module folders you intend to use, and Firebase.h inside to root/ios/Firebase. The README can be useful at this step.

3.3. In xcode and in your build settings for your project (You need to have your project selected under targets and not just under Project). search for Header Search Paths and add this $(SRCROOT)/Firebase

3.4. Then Search for Framework Search Paths and add this $(SRCROOT)/Firebase.

3.5. Search for Other Linker Settings and add the -ObjC

3.6. In ios/[YOUR APP NAME]/AppDelegate.m add #import <Firebase.h> at the top.

3.7. Still in AppDelegate.m add [FIRApp configure]; in the beginning of the method didFinishLaunchingWithOptions:(NSDictionary *)launchOptions.

#### Note: If you run into issues with the steps outlined in the README, there are a number of workarounds:
- https://github.com/invertase/react-native-firebase/issues/1847#issuecomment-457952828

## 2. React Native Firebase Installation Recommended installation

### Option 1: Link React Native Firebase (Recommended)

Run:

```bash
react-native link react-native-firebase
```

### Option 2: Cocoapods (Not Recommended)

Whilst we do not recommend using `react-native-firebase` as a Pod, it is possible to install this way provided you are:

1) Not using Swift
2) Don't have the `use_frameworks!` flag enabled

This is a restriction of Cocoapods and how it interacts with Dynamic Frameworks such as Firebase.

> The upcoming v6 release supports `use_frameworks!` by default and modules are distrubted as static frameworks. [Learn more here](https://github.com/invertase/react-native-firebase/issues/2025). 

#### Why do we not recommend a pod installation?

Our preference would definitely be to make use of Cocoapods for both Firebase and React Native Firebase and indeed, this did use to be our recommended approach.

However, due to the fragmented React Native ecosystem and the fact that it defaults to a non-Cocoapods installation, it means that getting up and running with Cocoapods can be a bit of a nightmare.

If you look through our issue history, you'll see this caused us countless issues.  Since switching to using `react-native link` we have managed to minimise them dramatically.

## 3. Install modules

`React Native Firebase` only provides your application with access to [Core](version /core/reference) features. Check out the installation guides on the other modules for how to use other Firebase features.
