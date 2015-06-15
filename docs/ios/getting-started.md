**1. Create an account**

The first thing you'll need to do is to [sign up for an account](http://developer.socialradar.com). This will generate your Developer Portal, and an API token.

***

**2. Install the LocationKit SDK**

The easiest way to start using LocationKit in your project is by using [CocoaPods](https://cocoapods.org); add LocationKit to your `Podfile`:
```ruby
pod 'LocationKit', '~> 2.0.0'
```
Then just run `pod install` from your project root.


***

**3. Configure your project**

Enable *Location Updates* in the allowed *Background Modes*.

***

**4. Configure permissions**

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

**5. Initialize and start LocationKit**

Include the LocationKit header in your app to get all the classes:

```objective_c
#import <LocationKit/LocationKit.h>
```

Enable LocationKit with the API token that you obtained earlier:

```objective_c
[LocationKit startWithApiToken:@"your_api_token" andDelegate:nil];
```

LocationKit will now start collecting location data and is ready for you to request points or places.

***

**Next steps**

* Read the full [Usage Guide](using-locationkit.md)
* View the [iOS API Reference](api-reference.md)
* Learn about the LocationKit [object model](object-model.md)
* Look at our [sample app](sample-app.md)
