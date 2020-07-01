# [DEPRECATED] Push Notifications Plugin V2 for Android, iOS and Windows

### _This plugin is deprecated, i.e. it is no longer maintained. Going forward additional features and bug fixes will be added to the new puship-plugin repository._
---

## DESCRIPTION

This plugin is for use with [Puship.com](https://www.puship.com), it's quickly enable support for Push Notifications on phonegap and cordova applications.

This plugin extends the plugin [phonegap-plugin-push](https://github.com/phonegap/phonegap-plugin-push),
So, you can use all his methods for manage the push notifications


**Important** - Push notifications are intended for real devices. The registration process will fail on the iOS simulator. Notifications can be made to work on the Android Emulator, however doing so requires installation of some helper libraries.



##<a name="automatic_installation"></a>Automatic Installation


**Note:** For each service supported - APNS, GCM or MPNS - you may need to download the SDK and other support files.

### Cordova

The plugin can be installed via the Cordova command line interface:

1) Navigate to the root folder for your cordova project. 2) Run the command.

```sh
cordova plugin add PushPlugin_V2 --variable SENDER_ID="XXXXXXX"
```

### Phonegap

The plugin can be installed using the Phonegap command line interface:

1) Navigate to the root folder for your phonegap project. 2) Run the command.

```sh
phonegap local plugin add PushPlugin_V2 --variable SENDER_ID="XXXXXXX"
```

Where the `XXXXXXX` in `SENDER_ID="XXXXXXX"` maps to the project number in the [Google Developer Console](https://www.google.ca/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwikqt3nyPjMAhXJ5iYKHR0qDcsQFggbMAA&url=https%3A%2F%2Fconsole.developers.google.com%2F&usg=AFQjCNF0eH059mv86nMIlRmfsf42kde-wA&sig2=BQ2BJpchw1CpGt87sk5p6w&bvm=bv.122852650,d.eWE). To find the project number login to the Google Developer Console, select your project and click the menu item in the screen shot below to display your project number.

![zzns8](https://cloud.githubusercontent.com/assets/353180/15588897/2fc14db2-235e-11e6-9326-f97fe0ec15ab.png)

If you are not creating an Android application you can put in anything for this value.

> Note: if you are using ionic you may need to specify the SENDER_ID variable in your package.json.

```
  "cordovaPlugins": [
    {
      "variables": {
        "SENDER_ID": "XXXXXXX"
      },
      "locator": "PushPlugin_V2"
    }
  ]
```

> Note: You need to specify the SENDER_ID variable in your config.xml if you plan on installing/restoring plugins using the prepare method.  The prepare method will skip installing the plugin otherwise.

```
<plugin name="PushPlugin_V2">
    <variable name="SENDER_ID" value="XXXXXXX" />
</plugin>
```

##<a name="automatic_installation"></a>How to use:

Go to [puship.com](https://www.puship.com/members) and create your Free account. Configure your application from dashboard an then add this code to your index.js:


```
receivedEvent: function(id) {
        var parentElement = document.getElementById(id);
        var listeningElement = parentElement.querySelector('.listening');
        var receivedElement = parentElement.querySelector('.received');
		
        listeningElement.setAttribute('style', 'display:none;');
        receivedElement.setAttribute('style', 'display:block;');

        console.log('Received Event: ' + id);
		
		
		var Puship = puship.init();
		Puship.PushipAppId="z23KUaPqVU7MbUq"; //Your AppId generated on Puship Dashboard
		Puship.EnableLog=true;
		
	
 		var push = PushNotification.init({
			android: {
				senderID: "XXXXXXX" 
			},
			ios: {
				alert: "true",
				badge: true,
				sound: 'false'
			},
			windows: {}
		});
		
		push.on('registration', function(data) {

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

