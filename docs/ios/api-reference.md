##LocationKit

***

###- sharedInstance

``+ (LocationKit *)sharedInstance``

Enables LocationKit using an API token retrieved from the [developer portal](https://developer.socialradar.com).

Optionally, you can specify an object that implements the [LocationKitDelegate](#locationkitdelegate) protocol; otherwise specify `nil`.

**Arguments**

| Argument | Description |
| --- | --- |
| *token* | The API token retrieved from the developer portal. |
| *delegate* | The object that serves as the delegate for LocationKit (may be `nil`). |

**Return value**

`LocationKit`

***

###- startWithApiToken

``- (LocationKit *)startWithApiToken:(NSString *)token andDelegate:(id <LocationKitDelegate>)delegate``

Enables LocationKit using an API token retrieved from the [developer portal](https://developer.socialradar.com).

Optionally, you can specify an object that implements the [LocationKitDelegate](#locationkitdelegate) protocol; otherwise specify `nil`.

**Arguments**

| Argument | Description |
| --- | --- |
| *token* | The API token retrieved from the developer portal. |
| *delegate* | The object that serves as the delegate for LocationKit (may be `nil`). |

**Return value**

`LocationKit`

***

###- pause

``- (void)pause``

Pause LocationKit and step receiving locations and visits.

*Note: Pausing LocationKit will prevent you from receiving any further updates to locations, places, visits, and so on so do so sparingly. It will prevent LocationKit from continuing to run in the background and significantly diminish the usefulness of our location-based developer insights. We strongly recommend not pausing LocationKit.*

**Arguments**

*none*

**Return value**

`nil`

***

###- resume

``- (NSError *)resume``

In order to once again receive updates and get locations or places after you have paused it, you must resume LocationKit with this method.

If there was some error starting LocationKit, the error will be returned. Otherwise, it will be `nil`

**Arguments**

*none*

**Return value**

`NSError`

***

###- getCurrentPlaceWithHandler

``- (void)getCurrentPlaceWithHandler:(void (^)(LKPlace *place, NSError *error))handler``

Retrieves the (place)[#lkplace] at the current location and handles the result in a block.

**Arguments**

| Argument | Description |
| --- | --- |
| *handler* | This block will be called with the results of the place retrieval. |

**Return value**

`nil`

***

###- getCurrentLocationWithHandler

``- (void)getCurrentLocationWithHandler:(void (^)(CLLocation *location, NSError *error))handler``

Retrieves the current location of the device, and handles the result in a block.

**Arguments**

| Argument | Description |
| --- | --- |
| *handler* | This block will be called with the results of the location retrieval. |

**Return value**

`nil`

***

##LocationKitDelegate

This protocol should be implemented on a delegate object to receive streaming updates from LocationKit.

***

###- didUpdateLocation

``- (void)locationKit:(LocationKit *)locationKit didUpdateLocation:(CLLocation *)location``

Any time we detect a new location, this method will be called on the delegate object.

*Unlike the Apple location manager's `didUpdateLocation` method, this will not provide a constant stream of location updates if the device is not moved. LocationKit will provide you with a more relevant set of location updates instead.*

**Arguments**

| Argument | Description |
| --- | --- |
| *location* | The updated location that needs to be handled in the app. |

**Return value**

`nil`

***

###- didStartVisit

``- (void)locationKit:(LocationKit *)locationKit didStartVisit:(LKVisit *)visit``

This delegate method is called anytime LocationKit detects that a visit is starting. The criteria for a visit starting is something we are always optimizing, but it generally happens within a couple minutes of the device arriving at a new location.

Once we gather enough location information and check with our database to retrieve information about this place, we call this delegate method.

**Arguments**

| Argument | Description |
| --- | --- |
| *visit* | The visit that started. |

**Return value**

`nil`

***

###- didEndVisit

``- (void)locationKit:(LocationKit *)locationKit didEndVisit:(LKVisit *)visit``

This method is called on the delegate when it detects that a visit to a place has ended.

**Arguments**

| Argument | Description |
| --- | --- |
| *visit* | The [visit](#lkvisit) that ended. |

**Return value**

`nil`

***
