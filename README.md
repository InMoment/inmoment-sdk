# InMoment SDK (iOS) (Beta)
InMoment is a simple framework giving client developers the ability to
launch and interact with InMoment web surveys.
**NOTE: This software is still in beta and is subject to change.**

## Screenshots

![presentsurvey_fl](https://cloud.githubusercontent.com/assets/15389109/18068072/e5e2c70e-6dfc-11e6-85fa-c7e60301b89b.gif)
![completionpoint_fl](https://cloud.githubusercontent.com/assets/15389109/18071937/d83ad6c2-6e16-11e6-8a3d-c49935905410.gif)
![lastpage_fl](https://cloud.githubusercontent.com/assets/15389109/18071939/dac98abe-6e16-11e6-909b-b976b6a84864.gif)

## System Requirements

- **Xcode 8.1** and **Swift 3.0.1**. Apps that include this framework may not build properly if newer or older versions of Xcode or Swift are used. If you need to target Swift 2.2, please use one of our **0.4.x** releases.
- Apps must require **iOS 8.0** or later.

## Usage

```swift
import InMoment
```

```swift
class ViewController: UIViewController, InMomentSurveyDelegate {

    override func viewDidAppear(animated: Bool) {
        super.viewDidAppear(animated)
        InMoment.presentSurvey(gateway: "MySurveyGateway", delegate: self)
    }

    func didRecieveErrorWhileLoadingSurvey(survey: InMomentSurvey, error: Error) {
        //Insert your own code here. This method is required.
        survey.dismiss()
    }

    func didArriveAtLastPageOfSurvey(survey: InMomentSurvey) {
        //Insert your own code here. This method is required.
        survey.dismiss()
    }

    func didPassCompletionPointInSurvey(survey: InMomentSurvey) {
        //Insert your own code here. This method is optional.
    }

}
```

### Presenting the survey

```swift
//class InMoment
public static func presentSurvey(gateway: String, 
                              parameters: [String:String] = [:],
                                delegate: InMomentSurveyDelegate)
```

Calling this method presents a survey modally using the given `presentationStyle`.
- `gateway` (required): A valid InMoment web survey gateway. Ask your CSM or operations specialist for details.
- `parameters` (optional): A dictionary of strings corresponding to any survey URL parameters.
- `delegate` (required): A reference to an implementation of `InMomentSurveyDelegate`.

### Recording survey completion

```swift
//protocol InMomentSurveyDelegate
optional func didPassCompletionPointInSurvey(survey: InMomentSurvey)
```

This method is called when the user arrives at the page immediately following the survey completion point. Once this happens, the survey response will be available in InMoment reports and in Hub 2.0™, (even if the user doesn't continue until the very last page of the survey). Use this method to perform actions such as recording that the user has finished taking the survey or giving the user a reward.

### Customizing survey appearance

The following methods can be implemented to customize the appearance of the survey view controller:

```swift
//protocol InMomentSurveyDelegate
optional func preferredWebSurveyModalPresentationStyle() -> UIModalPresentationStyle
optional func preferredWebSurveyNavigationBarStyle() -> UIBarStyle
optional func preferredWebSurveyNavigationBarColor() -> UIColor?
optional func preferredWebSurveyNavigationBarTextColor() -> UIColor?
optional func preferredWebSurveyProgressBarColor() -> UIColor?
optional func preferredWebSurveyLoadingViewStyle() -> LoadingViewStyle
```

## Privacy

If a survey has a video feedback prompt, this framework will attempt to access the device's camera, microphone, and/or photo library. Since the framework does not know if your survey has a video feedback prompt at compile-time, all developers must add the following entries to their Info.plist. All entries have string values:

- `NSCameraUsageDescription` (Privacy - Camera Usage Description)
- `NSMicrophoneUsageDescription` (Privacy - Microphone Usage Description)
- `NSPhotoLibraryUsageDescription` (Privacy - Photo Library Usage Description)

Failure to add these entries to your Info.plist will result in your app being rejected during App Store submission.

## Installation

### CocoaPods (Recommended)

1. Add the following to your ```Podfile```:

    ```ruby
    platform :ios, '8.0'
    use_frameworks!
    pod 'InMoment', '~> 0.7.2'
    ```

2. Run `pod install` or `pod update`.
3. Don't forget to add the appropriate entries to your Info.plist (see "Privacy").

### Carthage

1. Add the following to your ```Cartfile```:

    ```ruby
    github 'InMoment/inmoment-sdk-ios' ~> 0.7.2
    ```

2. Run `carthage bootstrap` or `carthage update`.
3. On your app target's `General` settings tab, drag `InMoment.framework` from the `Carthage/Build` folder into the `Embedded binaries` section.
4. On your app target's `Build Phases` settings tab, click the `+` icon and choose `New Run Script Phase`. Create a Run Script in which your specify your shell (ex: `bin/sh`), and add the following contents to the script area below the shell:

    ```bash
    /usr/local/bin/carthage copy-frameworks
    ```

    and add the following line under `Input Files`:

    ```bash
    $(SRCROOT)/Carthage/Build/iOS/InMoment.framework
    ```

    This script works around an App Store submission bug triggered by universal binaries and ensures that necessary bitcode-related files and dSYMs are copied when archiving.

5. Don't forget to add the appropriate entries to your Info.plist (see "Privacy").

### Manual Installation

See our [manual installation guide](/Manual Installation.md).
