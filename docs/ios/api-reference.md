##LocationKit

***

###+ enableWithApiToken

``+ (LocationKit *)enableWithApiToken:(NSString *)token andDelegate:(id <LocationKitDelegate>)delegate``

Enables LocationKit using an API token retrieved from the [developer portal](https://developer.socialradar.com). Optionally, you can specify an object that implements the [LocationKitDelegate](#locationkitdelegate) protocol; otherwise specify `nil`.

**Arguments**

| Argument | Description |
| --- | --- |
| *token* | The API token retrieved from the developer portal. |
| *delegate* | The object that serves as the delegate for LocationKit. |

**Return value**

`LocationKit`

***

###+ sharedInstance

``+ (LocationKit *)sharedInstance``

Returns the initialized `LocationKit` object.

**Return value**

`LocationKit`

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

This protocol should be implemented on a delegate object.

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

###- didStartVisit

``- (void)locationKit:(LocationKit *)locationKit didStartVisit:(LKVisit *)visit``

Started visit?

**Arguments**

| Argument | Description |
| --- | --- |
| *visit* | The visit that started. |

**Return value**

`nil`

***
