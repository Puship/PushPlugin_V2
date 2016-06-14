# Push Notifications Plugin V2 for Android, iOS and Windows

---

## DESCRIPTION

This plugin is for use with [Puship.com](http://www.puship.com), it's quickly enable support for Push Notifications on phonegap applications.

**Important** - Push notifications are intended for real devices. The registration process will fail on the iOS simulator. Notifications can be made to work on the Android Emulator, however doing so requires installation of some helper libraries.



##<a name="license"></a> LICENSE

	The MIT License

	Copyright (c) 2012 Adobe Systems, inc.
	portions Copyright (c) 2012 Olivier Louvignes

	Permission is hereby granted, free of charge, to any person obtaining a copy
	of this software and associated documentation files (the "Software"), to deal
	in the Software without restriction, including without limitation the rights
	to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
	copies of the Software, and to permit persons to whom the Software is
	furnished to do so, subject to the following conditions:

	The above copyright notice and this permission notice shall be included in
	all copies or substantial portions of the Software.

	THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
	IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
	FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
	AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
	LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
	OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
	THE SOFTWARE.




##<a name="automatic_installation"></a>Automatic Installation


**Note:** For each service supported - APNS, GCM or MPNS - you may need to download the SDK and other support files.

### Cordova

The plugin can be installed via the Cordova command line interface:

1) Navigate to the root folder for your phonegap project. 2) Run the command.

```sh
cordova plugin add com.phonegap.plugins.Puship_V2
```

### Phonegap

The plugin can be installed using the Phonegap command line interface:

1) Navigate to the root folder for your phonegap project. 2) Run the command.

```sh
phonegap local plugin add com.phonegap.plugins.Puship_V2
```

### Plugman

The plugin is based on [plugman](https://github.com/apache/cordova-plugman) and can be installed using the Plugman command line interface:

```sh
plugman install --platform [PLATFORM] --project [TARGET-PATH] --plugin [PLUGIN-PATH]

where
	[PLATFORM] = ios, android or wp8
	[TARGET-PATH] = path to folder containing your phonegap project
	[PLUGIN-PATH] = path to folder containing this plugin
```

##<a name="automatic_installation"></a>How to use:

Go to puship.com and create your Free account. configure your application from dashboard an then add this code to your index.js:


```
receivedEvent: function(id) {
        var parentElement = document.getElementById(id);
        var listeningElement = parentElement.querySelector('.listening');
        var receivedElement = parentElement.querySelector('.received');
		
        listeningElement.setAttribute('style', 'display:none;');
        receivedElement.setAttribute('style', 'display:block;');

        console.log('Received Event: ' + id);
		
		
		var Puship = puship.init();
		Puship.PushipAppId="z23KUaPqVU7MbUq"; //YOU APPID
		Puship.EnableLog=true;
		
	
 		var push = PushNotification.init({
			android: {
				senderID: "921725624540" 
			},
			ios: {
				alert: "true",
				badge: true,
				sound: 'false'
			},
			windows: {}
		});
		
		push.on('registration', function(data) {
			//navigator.notification.alert("registration id:" + data.registrationId);
	
			Puship.Common.Register(data.registrationId,
			{
				successCallback: function (pushipresult){
					navigator.notification.alert("device registered with DeviceId:" + pushipresult.DeviceId);
				},
				failCallback: function (pushipresult){
					navigator.notification.alert("error during registration: "+ JSON.stringify(pushipresult));
				}
			});
		});
		
		push.on('notification', function(data) {
			
			navigator.notification.alert("PUSH: " + JSON.stringify(data));
			// data.title,
			// data.count,
			// data.sound,
			// data.image,
			// data.additionalData
		});
	}
```

Then run your app and start send push notifications!

...and yes, this is lovely all!

