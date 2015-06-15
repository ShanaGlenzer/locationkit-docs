##1. Create an account

The first thing you'll need to do is to [sign up for an account](http://developer.socialradar.com/#/signup). This will generate your Developer Portal account, and generate your API token.

Within the Developer Portal, you will be able to find your API token, and find insights into the location-based usage of your app with LocationKit.

***

##2. Install the SDK

**CocoaPods**

The easiest way to start using LocationKit in your project is by using [CocoaPods](https://cocoapods.org). Just add the following to the Podfile in your project, run `pod install`, and skip to [Configure your project](#configure-your-project):

        pod 'LocationKit', '~> 2.0.0'

**Download LocationKit.framework**

Alternatively, you can [download the client library], unzip the file, and drag and drop the `LocationKit.framework` file into your project under Frameworks.

***

##3. Configure your project

1. Link to the following libraries to your project:

    | Libraries |
    | - |
    | CoreLocation.framework |
    | MapKit.framework |


**AdSupport (optional)**

Optionally, you can link the `AdSupport.framework` library and, if present, we can use the Apple advertising identifier to identify the device. This is useful particularly if you have other apps or other, more general analytics platforms in which you'd like to cross reference location data your app will gather from LocationKit. Without the AdSupport framework it may not be possible to link data with some of those other sources if you run a report from our Developer Portal.

In the past, Apple has [rejected some apps which use the AdSupport framework](http://techcrunch.com/2014/02/03/apples-latest-crackdown-apps-pulling-the-advertising-identifier-but-not-showing-ads-are-being-rejected-from-app-store/) (particularly the IDFA, the identifier for advertisers) without serving advertisements hence why this framework is optional.

Under the hood, we look to see if this framework is present and use the correct identifier in all cases.

1. Enable **Location Updates** in the allowed **Background Modes**.

![Background Modes](../img/background_modes.png)

***

##4. Configure permissions

Add the following InfoPlist.strings file configuration (adjust the language as required by your app):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>NSLocationUsageDescription</key>
        <string>Change this line to inform the consumer about how location is being used.</string>
        <key>NSLocationAlwaysUsageDescription</key>
        <string>Change this line to inform the consumer about how location is being used in the background</string>
    </dict>
</plist>
```
***

##5. Initialize and start LocationKit

Include the LocationKit header in your app to get all the classes:

```objective_c
#import <LocationKit/LocationKit.h>
```

Enable LocationKit with the API token that you obtained earlier:

```objective_c
[LocationKit startWithApiToken:@"your_api_token" andDelegate:nil];
```

LocationKit will now start collecting location data.

Optionally you can provide a `LocationKitDelegate` which will receive a constant stream of updates as they come in. Here we have supplied a `nil`. More on that below.

***

## Single Location Requests

### > Location Point

To quickly obtain the user’s most recent location point, execute the following:

```objective_c
[LocationKit getCurrentLocationWithHandler:^(CLLocation *location, NSError *error) {
    if (error == nil) {
        NSLog(@"%.6f, %.6f, %@", location.coordinate.latitude, location.coordinate.longitude, location.timestamp);
    } else {
        NSLog(@"Error: %@", error);
    }
}];
```

Single point requests do not increase battery consumption rates. Notice this point is simply a `CLLocation` so if your code currently uses the Apple Location Manager, this will return an item in the same format. Here are the [Apple docs on CLLocation](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocation_Class/index.html)

You will quickly receive the user's most recent location point to the provided handler.

### > Place

To request the user's current place:

```objective_c
[LocationKit getCurrentPlaceWithHandler:^(LKPlace *place, NSError *error) {
    if (error == nil) {
        NSLog(@"The user is in %@", place.address.locality);
    } else {
        NSLog(@"Venues error %@", error);
    }
}];
```

This will return an LKPlace object. This place includes data about the user's current location.

An LKPlace object consists of the address for the user's current location along with the venue they are in, if they are at a business. *(see the [Object Model](object-model.md) docs for more details)*

If they are in a place with no address or venue, both will be `nil` so plan your code accordingly. *(Note, currently we only have reliable support and coverage for addresses and venues in the United States.)*

***

## Streaming Updates

In order to receive streaming updates, you must provide an object which conforms to the [LocationKitDelegate](api-reference/#locationkitdelegate) protocol when launching LocationKit. LocationKit will then call the methods on your delegate whenever it detects the device has moved to a new location.

### > Location Updates

As we detect the device moving to a new location and once we receive enough location data from the device to accurately determine the location of the device, we will call `didUpdateLocation` on your delegate.

So your delegate could have a method that looks like:

```objective_c
- (void)locationKit:(LocationKit *)locationKit didUpdateLocation:(CLLocation *)location {
    NSLog(@"The user has moved and their location is now (%.6f, %.6f)",
          location.coordinate.latitude,
          location.coordinate.longitude);
}
```

Note, this behaves a bit differently from the Apple Location Manager's `didUpdateLocations`. At a high level, the differences are:

 Apple | LocationKit
 -------|------------
 `didUpdateLocations` receives a constant stream of raw updates as it gets them from the GPS chip | `didUpdateLocation` takes the raw data and filters it to provide a single point with a higher degree of accuracy
 Receives many data points as they come in raw from the GPS chip | Receives fewer, cleaned up and confirmed data points
 Continues to be called constantly, even when the device has not moved | Is called only when a new location is detected by the device. This provides a significant battery optimization as code in this delegate method is executed only when the device is in a new location rather than being constantly executed.
 Receives an array of points, sometimes with a varying degree of accuracy | Receives a single clean point

LocationKit's `didUpdateLocation` will continue to receive location updates while the app is in the background.

If you have any further questions or confusion about this (or anything) please post about it on our [Community](https://community.socialradar.com) and someone from our staff will get back to you ASAP to clarify.

### > Visit start

When we detect a visit has started we will call the `didStartVisit` method on your LocationKitDelegate.

We detect a visit start when we have gathered enough location data to identify:

1. The location of the device
1. That the user is visiting a place

It will receive an LKVisit object which contains information on the time of arrival and the place to which this visit refers. For example:

```objective_c
- (void)locationKit:(LocationKit *)locationKit didStartVisit:(LKVisit *)visit {
    NSLog(@"LocationKit detected that a visit started at %@ to %@", visit.arrivalDate, visit.place.venue.name);
}
```
Note, an LKVisit also has a property called `departureDate` which of course would be `nil` for in `didStartVisit` (because without a crystal ball we cannot determine when the user will leave at the time they arrive!).

### > Visit end

When we detect that a visit to a place has ended, we will call the `didEndVisit` method on your delegate.

We are constantly tweaking the criteria to determine that a visit has ended, so this may change slightly, but generally we determine that a visit has ended when:

1. We detect that another visit has started or
1. We detect that the device has moved far enough away from the place that they are no longer there.

This delegate method will receive an LKVisit object which contains information about the time of arrival, departure, and the place to which that visit refers. For example:

```objective_c
- (void)locationKit:(LocationKit *)locationKit didEndVisit:(LKVisit *)visit {
    NSLog(@"LocationKit detected that a visit occurred from %@ to %@ at %@",
          visit.arrivalDate,
          visit.departureDate,
          visit.place.venue.name);
}
```

***

## Control mechanisms

###  > Pausing LocationKit

To pause LocationKit and stop receiving locations and visits, run the following:

```objective_c
[LocationKit pause];
```

Pausing LocationKit will prevent you from receiving any further updates to locations, places, visits, and so on so do so sparingly. It will prevent LocationKit from continuing to run in the background and significantly diminish the usefulness of our location-based developer insights. We strongly recommend not pausing LocationKit.

###  > Resuming LocationKit

In order to once again receive updates and get locations or places after you have paused it, you must resume LocationKit as follows:

```objective_c
NSError *error = [LocationKit resume];
```

If there was some issue starting LocationKit, the error will be non-`nil`
