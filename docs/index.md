## SocialRadar LocationKit

![LocationKit](img/locationkit.png)

SocialRadar's LocationKit provides advanced location technology to offer developers higher accuracy location, lower battery drain, automatic venue recognition and detailed location analytics.

LocationKit enables you to add enhanced location capabilities, and therefore personalized experiences, to your mobile applications without the need for location expertise.

<h3>How it works</h3>

LocationKit processes location signals through a private location manager instance, analyzing activity and validating data within the private SocialRadar cloud.

The LocationKit framework works independently of the host app, and other than the app providing match key data to the framework, no interaction between the app and framework is required to enable LocationKit to work properly.

<h3>Operating Requirements</h3>

-	Device capable of running iOS 7.x or above (Android coming soon!)
-	Device capable of properly returning location signals

<h3>Battery Consumption Safeguards</h3>

Battery consumption is extremely efficient – LocationKit averages 1.7% battery consumption per hour, depending on the type of device used. Additional details on battery consumption available in the docs.

<h3>Transparent operation</h3>

LocationKit never surfaces dialog boxes, errors or notifications directly to a consumer.

## Downloads

<h3>iOS</h3>

<h4>CocoaPods</h4>

Add the following to your Podfile then just run `pod install` from the root of your project

```
pod 'LocationKit', '~> 2.0.0'
```

<h4>Manual Download</h4>

We do not yet have LocationKit available for manual download. If you are unable to use the CocoaPod and need a `.framework`, please contact the LocationKit team at [locationkit@socialradar.com](mailto:locationkit@socialradar.com) and we will send you the file.

<h3>Android</h3>

Coming soon!
