##1. Create an account

The first thing you'll need to do is to [sign up for an account](http://developer.socialradar.com). This will generate your Developer Portal, and an API key.

Within the Developer Portal, you will be able to find your API token, and find insights into the usage of your app with LocationKit.

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
    | AdSupport.framework |
    | CoreLocation.framework |
    | MapKit.framework |

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
[LocationKit enableWithApiToken:@"your_api_token" andDelegate:nil];
```

LocationKit will now start collecting location data.

***

##6. Single Location Point Request

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

Single point requests do not increase battery consumption rates.

You will quickly receive the user's most recent location point to the provided handler.

##7. Streaming Update Frequency

Selecting the right update frequency when initiating streaming location tracking ensures the right algorithm is used to refine the GPS data.

If you’re unsure which frequency to use, choose **SRFrequencyLevelMedium** – this will provide accurate GPS data useful for most tracking requirements.

The **SRFrequencyLevel** defines the frequency with which you wish to update. Each type is optimized for processing speed, battery life and accuracy. The update frequencies offered are:

* **SRFrequencyLevelLowest** – suitable for continuous very low power GPS signaling (3.3%/hour on average). New location signals will arrive every 1-2 seconds when moving, less frequently if not moving.
* **SRFrequencyLevelLow** — suitable for general fitness, sports, and walking <15mph.
* **SRFrequencyLevelMedium** – tracking for activity speeds <25mph.
* **SRFrequencyLevelHigh** – tracking for speeds <40mph on land and water.
* **SRFrequencyLevelHighest** – automotive tracking with ‘snap-to-road’.

The location object will update as new location data becomes available. This data will include both highly refined and lightly refined data points suitable for mapping an activity.

In order to minimize battery usage, it is recommended that you enable the continuous stream only for the period your app needs that type of GPS update and disable it when that is no longer needed.

For example, for an app that tracks runs, start continuous updates when the user starts their run and stop when the user indicates their run is finished.

##8. Start Streaming Location Data

To start streaming location data, use the following (substitute the *SRFrequencyLevelMedium* with the activity tracking option you require):

```objective_c
[LocationKit startLocationUpdatesWithFrequencyLevel:SRFrequencyLevelMedium updateHandler:^(CLLocation *location, NSError *error) {
    if (error == nil) {
        NSLog(@"Activity location %@", location);
    } else {
        NSLog(@"Activity error %@", error);
    }
}];
```

Streaming location data is available when a continuous real-time feed of location data is required.

##9. Stop Streaming Location Data

To stop streaming location data, use the following:

```objective_c
[LocationKit stopLocationUpdates];
```

Stopping activity tracking will not stop LocationKit -- it only stops the location tracking function that you have chosen. Note, only one location tracking function will operate at any time.

##10. Single Venue List Request

To retrieve a list of venues at the current location, use the following:

```objective_c
[LocationKit getCurrentVenuesWithHandler:^(NSArray *venues, NSError *error) {
  if (error == nil) {
      if(venues.count > 0) {
          SRVenue *venue = venue[0];
          NSLog(@"First Venue name %@", venue.name);
      } else {
          NSLog(@"No Venues found");
      }
  } else {
      NSLog(@"Venues error %@", error);
  }
}];
```

`getCurrentVenuesWithHandler` will retrieve an array of SRVenue objects at the current location.

An SRVenue object contains the name, category and subcategory for a venue, in addition to its location and address information.  The address is represented as an NSDictionary.

##11. Start Receiving Venue Updates

To start receiving venue updates, use the following:

```objective_c
[LocationKit startVenueUpdatesWithHandler:^(NSArray *venues, NSError *error) {
  if (error == nil) {
      if(venues.count > 0) {
          SRVenue *venue = venue[0];
          NSLog(@"First Venue name %@", venue.name);
      } else {
          NSLog(@"No Venues found");
      }
  } else {
      NSLog(@"Venues error %@", error);
  }
}];
```

Venue updates will be sent when LocationKit detects that a user has entered a new venue, so there is no frequency setting as with location updates.

##12. Stop Receiving Venue Updates

To stop receiving venue updates, use the following:

```objective_c
[LocationKit stopVenueUpdates];
```

Stopping venue updates will not stop LocationKit -- it only stops receiving venue updates.

##13. Venue Object

Venue objects as returned with `getCurrentVenuesWithHandler` or `startVenueUpdatesWithHandler` have the following properties:

Property | Type | Description
--------- | --------- | ---------
locationCoordinate | CLLocationCoordinate2D | Coordinates of the venue
addressDictionary | NSDictionary | Address of the venue
name | NSString | Name of the venue
category | NSString | Category of the venue
subcategory | NSString | Subcategory of the venue

Below are the keys in `addressDictionary`:

Key | Type | Description
--------- | --------- | ---------
Street | kABPersonAddressStreetKey | Street address of the venue
City | kABPersonAddressCityKey | City of the venue address
State | kABPersonStateKey | State of the venue address
ZIP | kABPersonZIPKey | ZIP Code of the venue address
Country | kABPersonCountryKey | Country of the venue address
