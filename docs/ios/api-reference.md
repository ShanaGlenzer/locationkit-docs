##LocationKit

***

###+ enableWithApiToken

``+ (LocationKit *)enableWithApiToken:(NSString *)token andDelegate:(id <LocationKitDelegate>)delegate``

Enables LocationKit using an API token retrieved from the [developer portal](http://developer.socialradar.com). Optionally, you can specify an object that implements the [LocationKitDelegate](#locationkitdelegate) protocol; otherwise specify `nil`.

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

Update location? No idea. Please halp.

**Arguments**

| Argument | Description |
| --- | --- |
| *location* | The updated location that needs to be handled in the app. |

**Return value**

`nil`

***

###- didEndVisit

``- (void)locationKit:(LocationKit *)locationKit didEndVisit:(LKVisit *)visit``

Ended visit?

**Arguments**

| Argument | Description |
| --- | --- |
| *visit* | The visit that ended. |

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
