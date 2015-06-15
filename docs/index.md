## SocialRadar LocationKit

![LocationKit](img/locationkit.png)

LocationKit is designed to provide accurate location data to apps which require precise or continuous location services, passive venue check-in and detailed user insights based on the places your users go every day.

### How it works

LocationKit processes location signals through a private location manager instance, analyzing activity and validating data within the private SocialRadar cloud. Anonymized consumer insights may be shared with app publishers and marketing firms.

The LocationKit framework works independently of the host app, and other than the app providing match key data to the framework, no interaction between the app and framework is required to enable LocationKit to work properly.

### Operating Requirements

-	Device capable of running iOS 7.x or above (Android coming soon!)
-	Device capable of properly returning location signals

### Battery Consumption Safeguards

Battery consumption is extremely efficient – LocationKit averages 1.7% battery consumption per hour, depending on the type of device used. We will quantify this in greater detail later!

### Transparent operation

LocationKit never surfaces dialog boxes, errors or notifications directly to a consumer.

## Downloads

### iOS

#### CocoaPods

Add the following to your Podfile then just run `pod install` from the root of your project

```
pod 'LocationKit', '>= 2.0.0'
```

#### Manual Download

We do not yet have LocationKit available for manual download. If you are unable to use the CocoaPod and need a `.framework`, please contact the LocationKit team at [locationkit@socialradar.com](mailto:locationkit@socialradar.com) and we will send you the file.

### Android

Coming soon!
