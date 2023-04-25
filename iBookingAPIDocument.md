- [iBooking API Document](#ibooking-api-document)
  - [HTTP](#http)
      - [Header](#header)
      - [Hotel registration](#hotel-registration)
        - [POST：`` http://fota.qiksmart.com/hotel_lock/addManager.php``](#post-httpfotaqiksmartcomhotel_lockaddmanagerphp)
      - [Hotel Login](#hotel-login)
        - [POST：`` http://fota.qiksmart.com/hotel_lock/checkpass.php``](#post-httpfotaqiksmartcomhotel_lockcheckpassphp)
      - [Guest registration](#guest-registration)
        - [POST：``http://fota.qiksmart.com/hotel_lock/adduser.php``](#posthttpfotaqiksmartcomhotel_lockadduserphp)
      - [Guest Login](#guest-login)
        - [POST：``http://fota.qiksmart.com/hotel_lock/checkuserpass.php``](#posthttpfotaqiksmartcomhotel_lockcheckuserpassphp)
      - [Get Guest info](#get-guest-info)
        - [POST：``http://fota.qiksmart.com/hotel_lock/showusers.php``](#posthttpfotaqiksmartcomhotel_lockshowusersphp)
      - [Update Guest info](#update-guest-info)
        - [POST：``http://fota.qiksmart.com/hotel_lock/updateuser.php``](#posthttpfotaqiksmartcomhotel_lockupdateuserphp)
      - [Add hotel](#add-hotel)
        - [POST：`` http://fota.qiksmart.com/hotel_lock/addHotel.php``](#post-httpfotaqiksmartcomhotel_lockaddhotelphp)
      - [Get hotel info](#get-hotel-info)
        - [POST：``http://fota.qiksmart.com/hotel_lock/showhotels.php``](#posthttpfotaqiksmartcomhotel_lockshowhotelsphp)
      - [Binding room and lock](#binding-room-and-lock)
        - [POST：``http://fota.qiksmart.com/hotel_lock/addroom.php``](#posthttpfotaqiksmartcomhotel_lockaddroomphp)
      - [Get room info](#get-room-info)
        - [POST：``http://fota.qiksmart.com/hotel_lock/showrooms.php``](#posthttpfotaqiksmartcomhotel_lockshowroomsphp)
      - [Get Lock info](#get-lock-info)
        - [POST：``http://fota.qiksmart.com/hotel_lock/showlocks.php``](#posthttpfotaqiksmartcomhotel_lockshowlocksphp)
      - [Update room info](#update-room-info)
        - [POST：``http://fota.qiksmart.com/hotel_lock/updateroom.php``](#posthttpfotaqiksmartcomhotel_lockupdateroomphp)
      - [Delete hotel](#delete-hotel)
        - [POST：``http://fota.qiksmart.com/hotel_lock/delhotel.php ``](#posthttpfotaqiksmartcomhotel_lockdelhotelphp-)
      - [Delete room](#delete-room)
        - [POST：``http://fota.qiksmart.com/hotel_lock/delroom.php ``](#posthttpfotaqiksmartcomhotel_lockdelroomphp-)
      - [Delete SmartLock](#delete-smartlock)
        - [POST：``http://fota.qiksmart.com/hotel_lock/dellock.php``](#posthttpfotaqiksmartcomhotel_lockdellockphp)
      - [Reset Guest Password](#reset-guest-password)
        - [POST：``http://fota.qiksmart.com/hotel_lock/updateuserpass.php``](#posthttpfotaqiksmartcomhotel_lockupdateuserpassphp)
      - [Reset Hotel Password](#reset-hotel-password)
        - [POST：``http://fota.qiksmart.com/hotel_lock/updateManagerpass.php``](#posthttpfotaqiksmartcomhotel_lockupdatemanagerpassphp)
      - [Add  Cleaner](#add--cleaner)
        - [POST：`` http://fota.qiksmart.com/hotel_lock/adduser.php``](#post-httpfotaqiksmartcomhotel_lockadduserphp)
      - [Cleaner Login](#cleaner-login)
        - [POST：``http://fota.qiksmart.com/hotel_lock/checkuserpass.php``](#posthttpfotaqiksmartcomhotel_lockcheckuserpassphp-1)
  - [SinovoLib API Document - Android Platform](#sinovolib-api-document---android-platform)
    - [Installing](#installing)
      - [Add the following in your ``main/AndroidManifest.xml``:](#add-the-following-in-your-mainandroidmanifestxml)
    - [SinovoBle](#sinovoble)
      - [Initialize Ble:](#initialize-ble)
      - [Ble connect to lock via qrcode](#ble-connect-to-lock-via-qrcode)
      - [Lock opening and closing operation](#lock-opening-and-closing-operation)
      - [Automatic connection lock in non-binding mode](#automatic-connection-lock-in-non-binding-mode)
      - [Get information of the lock](#get-information-of-the-lock)
      - [Generate interval dynamic code ,Calculate periodic code](#generate-interval-dynamic-code-calculate-periodic-code)
      - [Calculate one-time code or Timed code](#calculate-one-time-code-or-timed-code)
      - [Stop Scan BLE](#stop-scan-ble)
      - [Disconnect the BLE connection and clear the cache](#disconnect-the-ble-connection-and-clear-the-cache)
- [iosSinovoLib.framework API Document](#iossinovolibframework-api-document)
  - [IOS Platform](#ios-platform)
    - [Installing](#installing-1)
      - [Add the following in your ``info.plist``:](#add-the-following-in-your-infoplist)
    - [SinovoBle](#sinovoble-1)
      - [Initialize Ble:](#initialize-ble-1)
      - [Initialize SinovoBle data callback:](#initialize-sinovoble-data-callback)
      - [Ble connect to lock via qrcode](#ble-connect-to-lock-via-qrcode-1)
      - [Lock opening and closing operation](#lock-opening-and-closing-operation-1)
      - [Automatic connection lock in non-binding mode](#automatic-connection-lock-in-non-binding-mode-1)
      - [Get information of the lock](#get-information-of-the-lock-1)
      - [Generate interval dynamic code ,Calculate periodic code](#generate-interval-dynamic-code-calculate-periodic-code-1)
      - [Calculate one-time code or Timed code](#calculate-one-time-code-or-timed-code-1)
      - [Stop Scan BLE](#stop-scan-ble-1)
      - [Disconnect the BLE connection and clear the cache](#disconnect-the-ble-connection-and-clear-the-cache-1)
  - [Issues](#issues)
  - [Author](#author)




# iBooking API Document


##  HTTP
#### Header
```
header("Content-type:application/x-www-form-urlencoded;charset=utf-8"); 
```


#### Hotel registration
* Params:
  * username – String
  * password – String

* Return: success
##### POST：`` http://fota.qiksmart.com/hotel_lock/addManager.php``
  

####  Hotel Login 
* Params:
  * username – String
  * password – String

* Return: success 
* Error Return: password error
##### POST：`` http://fota.qiksmart.com/hotel_lock/checkpass.php``




#### Guest registration
* Params:
  * username – String
  * password – String
  * type - String

* Return: success 
##### POST：``http://fota.qiksmart.com/hotel_lock/adduser.php``



#### Guest Login
* Params:
  * username – String
  * password – String

* Return: ``{"status":"success","user":"guest@qq.com","type":"1","lock_key":null}`` 
* Error Return: password error
##### POST：``http://fota.qiksmart.com/hotel_lock/checkuserpass.php``


#### Get Guest info
* Params:
  * username – String

* Return: `` {"dataType":"userlist","username":"321@qq.com","type":"1","lock_key":"[{\"hotelName\":\"yang\",\"roomNo\":\"269\",\"type\":1}]"}`` 
##### POST：``http://fota.qiksmart.com/hotel_lock/showusers.php``


#### Update Guest info
* Params:
  * username – String
  * lock_key - String

* Return: success 
##### POST：``http://fota.qiksmart.com/hotel_lock/updateuser.php``



#### Add hotel 
* Params:
  * hotelname – String
  * address – String
  * phone – String

* Return: success 
##### POST：`` http://fota.qiksmart.com/hotel_lock/addHotel.php``


#### Get hotel info 
* Params:
  * hotelname – String or null
* hotelname is not null, get specifies the info of the hotel; If the hotel name is not provided, obtain info about all hotels 

* Return: ``{"dataType":"hotel_list","list_size":3,"hotels":[{"hotelname":"yang","hotel_addr":"aaaa","hotel_phone":"18090309691"}]}``

##### POST：``http://fota.qiksmart.com/hotel_lock/showhotels.php``
	

 #### Binding room and lock
* Params:
  * hotelname – String
  * roomNO - String
  * roomType - String
  * deviceID - String
  * lockMAC - String
  * lockSNO - String
  * lockQRcode - String
  * basetime - String
  * status - String

* Return: success
##### POST：``http://fota.qiksmart.com/hotel_lock/addroom.php``

	

 #### Get room info
* Params:
  * hotelname - String 
  * roomNO - String or not provided will get all rooms

* Return: ``{"dataType":"room_list","list_size":1,"rooms":[{"hotelname":"yang","roomNO":"269","roomType":"2","status":"1","bookTime":"","deviceID":"FE655103CE44","lockMAC":"FE655103CE44","lockSNO":"eccf3f","lockQRcode":"222111090001","basetime":"230423100528"}]}`` 
##### POST：``http://fota.qiksmart.com/hotel_lock/showrooms.php``



 #### Get Lock info
* Params:
  * deviceID - String or not provided will get all locks
  * bookTime - String
  * status - String

* Return: ``{"dataType":"lock_list","list_size":1,"locks":[{"deviceID":"FE655103CE44","lockMAC":"FE655103CE44","lockSNO":"eccf3f","lockQRcode":"222111090001","basetime":"230423100528"}]}`` 
##### POST：``http://fota.qiksmart.com/hotel_lock/showlocks.php``
   


#### Update room info
* Params:
  * roomNO - String
  * bookTime - String
  * status - String

* Return: success 
##### POST：``http://fota.qiksmart.com/hotel_lock/updateroom.php``

   

#### Delete hotel
* Params:
  * hotelname – String
  * roomNO - String

* Return: success 
##### POST：``http://fota.qiksmart.com/hotel_lock/delhotel.php ``



#### Delete room
* Params:
  * hotelname – String
  * roomNO - String

* Return: success 
##### POST：``http://fota.qiksmart.com/hotel_lock/delroom.php ``




#### Delete SmartLock
* Params:
  * deviceID – String

* Return: success 
##### POST：``http://fota.qiksmart.com/hotel_lock/dellock.php``
  




#### Reset Guest Password
* Params:
  * username – String
  * password – String

* Return: success 
##### POST：``http://fota.qiksmart.com/hotel_lock/updateuserpass.php``



#### Reset Hotel Password
* Params:
  * username – String
  * password – String

* Return: success 
##### POST：``http://fota.qiksmart.com/hotel_lock/updateManagerpass.php``



#### Add  Cleaner
* Params:
  * username – String
  * password – String
  * type - String

* Return: success 
##### POST：`` http://fota.qiksmart.com/hotel_lock/adduser.php``


#### Cleaner Login
* Params:
  * username – String
  * password – String

* Return: ``{"status":"success","user":"guest@qq.com","type":"1","lock_key":null}`` 
* Error Return: password error
##### POST：``http://fota.qiksmart.com/hotel_lock/checkuserpass.php``








<br>
<br>
<br>
<br>
<br>
<br>

##  SinovoLib API Document - Android Platform

### Installing
Add sinovoLib to your ``build.grade(:app)`` file:
 ``` 
 dependencies{
    implementation 'com.github.konewu:sinovoLib:latest version'
}
```
Sync Project with Gradle


#### Add the following in your ``main/AndroidManifest.xml``:
These permissions are required for sinovoLib to work
```
// .........

    <!--BLUETOOTH Permission-->
    <uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
 
    <!--LOCATION Permission-->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

    <!--MQTT need these permissions-->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.WAKE_LOCK"/>

// .........

</manifest>
```

### SinovoBle
#### Initialize Ble: 
* Params:
  * context – context
  * iScanCallBack – ble scan callback
  * iConnectCallback – ble connect callback
```
SinovoBle.getInstance().init(Context context, IScanCallBack iScanCallBack, IConnectCallback iConnectCallback)
```


#### Ble connect to lock via qrcode
* Params:
  * qrcode – qrcode of the lock
  * userIMEI – lock will bind to the user
```
SinovoBle.getInstance().connectLockViaQRCode(String qrcode, String userIMEI)
```
Example Return:
``
{"funCode":"01","errCode":"00","lockMac":"FE655103CE44","lockSno":"d7a76e","lockQRCode":"222111090001","autoCreateUser":"01","userNid":"02","codeType":"02","sid":"02","code":"036465"}
``



#### Lock opening and closing operation
* Params:
  * unlockType – 00 Lock、01 Unlock
```
SinovoBle.getInstance().toUnlock(String unlockType, String code, String lockSNO, String lockMacAddress)
```
Example Return:
``
{"funCode":"0a","errCode":"00","lockMac":"FE655103CE44","opType":"01"}
``


#### Automatic connection lock in non-binding mode
* Params:
  * autoConnectList – Automatically connected lock list
```
SinovoBle.getInstance().connectLockViaMacSno(final ArrayList<BleConnectLock> autoConnectList)
```


#### Get information of the lock
* Params:
  * dataType – \
    01 get admin's info\
    02 get power of the lock\
    03 get status of the lock ,is it unlocked?\
    04 get hardware information\
    05 get the lockname\
    06 get time of the lock\
    07 get auto-lock time of the lock\
    08 get the volume setting\
    09 get the auto-create setting\
    10 get the superUser's priority\
    11 get the basetime of the lock\
    12 get lock info（power、status、auto-lock、volume、auto-create、superUser、firmware）
```
SinovoBle.getInstance().getLockInfo(String dataType ,String lockSNO, String lockMacAddress)
```
dataType = 12, Example Return:
``
{"funCode":"2c","errCode":"00","lockMac":"FE655103CE44","power":9,"lockStatus":"00","autoLockTime":5,"volume":"05","autocreate":"00","superUserLevel":"01","fwVersion1":"01","fwVersion2":"02","fwVerTime":"221107","fwType":"05"}
``


#### Generate interval dynamic code ,Calculate periodic code
* Params:
  * lockmac – lock mac address, such as 00A051F4D44A
  * starttime – start time, such as 12:23
  * endtime – end time, such as 12:23

* Returns: code
```
SinovoBle.getInstance().calcDyCode(String lockmac, String starttime, String endtime)
```



#### Calculate one-time code or Timed code
* Params:
  * lockmac – lock mac address, such as 00A051F4D44A
  * basetime – basetime,such as 2021-03-23 19:14
  * starttime – start time, such as 2020-07-08 17:18
  * valid – Effective time period, such as 3 Days, 1 Hours, 1 Months,[one-time code: 3 Days]
  * type – 0: one-time  1: Timed code

* Returns: code
```
SinovoBle.getInstance().calcDyCode(String lockmac, String basetime,  String starttime, String valid, int type)
```


#### Stop Scan BLE
```
SinovoBle.getInstance().stopScanBLE()
```

#### Disconnect the BLE connection and clear the cache
```
SinovoBle.getInstance().disconnBle()
```

<br>
<br>
<br>
<br>
<br>
<br>

# iosSinovoLib.framework API Document
## IOS Platform
### Installing
* Add the ``iosSinovoLib.framework`` in TARGET
* Add the following in your Podfile:
```
target 'Runner' do
  
  pod 'iOSDFULibrary'
  pod 'IotLinkKit', '1.2.1'

  use_frameworks!
  use_modular_headers!

end
```

Before using the ``iOSDFULibrary``, you need to create a bridging header file (``Header.h``):
````
//
//  Use this file to import your target's public headers that you would like to expose to Swift.
//
@import iOSDFULibrary;
````


#### Add the following in your ``info.plist``:
These permissions are required for iosSinovoLib to work
```
// .........

<dict>
// .........
    
      // Bluetooth
      <key>NSBluetoothAlwaysUsageDescription</key>
	  <string>The application needs to access bluetooth</string>
	  <key>NSBluetoothPeripheralUsageDescription</key>
	  <string>The application needs to access bluetooth</string>
    
    
      // Location
      <key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
	  <string>Always and when in use!</string>
	  <key>NSLocationAlwaysUsageDescription</key>
	  <string>Can I have location always?</string>
	  <key>NSLocationUsageDescription</key>
	  <string>Older devices need location.</string>
	  <key>NSLocationWhenInUseUsageDescription</key>
	  <string>Need location when in use</string>



       <key>UIBackgroundModes</key>
	   <array>
	    	<string>bluetooth-central</string>
	    	<string>bluetooth-peripheral</string>
	    	<string>location</string>
	   </array>


  </dict>
</plist>
```


### SinovoBle
#### Initialize Ble: 
````
SinovoBle *sinovoBle = [SinovoBle sharedBLE];
````



#### Initialize SinovoBle data callback:
````
- (void) initBle {
    [SinovoBle sharedBLE].delegate = self;
    [[SinovoBle sharedBLE] centralInit];
}

[[SinovoBleCallBack sharedBleCallBack] initBle];
````



#### Ble connect to lock via qrcode
* Params:
  * qrcode – qrcode of the lock
  * userIMEI – lock will bind to the user

* data callback: ``onConnectLockViaQRCode :(NSMutableDictionary *)dict``
```
[[SinovoBle sharedBLE] connectLockViaQRCode :(NSString *)qrcode :(NSString *)userIMEI]
```


#### Lock opening and closing operation
* Params:
  * unlockType – 00 Lock、01 Unlock
  * code - unlock/lock password

* data callback: ``onUnlock :(NSMutableDictionary *)dict``
* data callback: Failed to unlock five times  ``onLockFrozen :(NSMutableDictionary *)dict``
```
[[SinovoBle sharedBLE] toUnlock :(NSString *)dataType :(NSString *)code :(NSString *)lockSNO :(NSString *)lockMacAddress]
```



#### Automatic connection lock in non-binding mode
* Params:
  * autoConnectList – Automatically connected lock list

* data callback: ``onConnectLockViaMacSno :(NSMutableDictionary *)dict``
```
[[SinovoBle sharedBLE] connectLockViaMacSno :(NSMutableArray *)autoConnectLockList]
```
* Usage:
````
NSMutableArray *bleLockArr = [[NSMutableArray alloc] initWithCapacity:0];
BleLock  *bleLock = [[BleLock alloc] init];
bleLock.lockmac = "FE655103CE44";
bleLock.SNO = "02f621";
bleLock.qrCode = "123456789012";
[bleLockArr addObject: bleLock];

[[SinovoBle sharedBLE] connectLockViaMacSno : bleLockArr];
````  



#### Get information of the lock
* Params:
  * dataType –\
    01 get admin's info\
    02 get power of the lock\
    03 get status of the lock ,is it unlocked?\
    04 get hardware information\
    05 get the lockname\
    06 get time of the lock\
    07 get auto-lock time of the lock\
    08 get the volume setting\
    09 get the auto-create setting\
    10 get the superUser's priority\
    11 get the basetime of the lock\
    12 get lock info（power、status、auto-lock、volume、auto-create、superUser、firmware）

* data callback: ``onLockInfo :(NSMutableDictionary *)dict``
```
[[SinovoBle sharedBLE] getLockInfo :(int)dataType :(NSString *)lockSNO :(NSString *)lockMacAddress]
```



#### Generate interval dynamic code ,Calculate periodic code
* Params:
  * mac_addr – lock mac address, such as 00A051F4D44A
  * baseTime - base time of lock,such as 2021-03-23 19:14
  * startTime – start time, such as 12:23
  * codeType - 03 (``NSString``)
  * validTime – end time, such as 12:23
  
* Returns: code
```
GetDymanicCode *getDymanicCode =  [[GetDymanicCode alloc] init];

NSString *result = [getDymanicCode getDymanicCode :(NSString *)mac_addr :(NSString*)baseTime :(NSString *)startTime :(NSString *)codeType :(NSString *)validTime];
```



#### Calculate one-time code or Timed code
* Params:
  * mac_addr – lock mac address, such as 00A051F4D44A
  * baseTime – base time of lock,such as 2021-03-23 19:14
  * startTime – start time, such as 2020-07-08 17:18
  * codeType – 02(``NSString``): one-time , 04(``NSString``): timed code
  * validTime – Effective time period, such as 3 Days, 1 Hours, 1 Months , [ one-time code: 3d(``NSString``) ]

* Returns: code
```
GetDymanicCode *getDymanicCode =  [[GetDymanicCode alloc] init];

NSString *result = [getDymanicCode getDymanicCode :(NSString *)mac_addr :(NSString*)baseTime :(NSString *)startTime :(NSString *)codeType :(NSString *)validTime];
```



#### Stop Scan BLE
```
[[SinovoBle sharedBLE] cancelConnectLock]
```


#### Disconnect the BLE connection and clear the cache
```
[[SinovoBle sharedBLE] toDisconnBle]
```



## Issues

Please file any issues, bugs or feature request as an issue on our [GitHub](https://github.com/konewu/iosSinovoLibrary) page. Commercial support is available if you need help with integration with your app or services. You can contact us at [ken.wu@zygcom.com](mailto:ken.wu@zygcom.com).


## Author

This plugin for App is developed by [Sinovotec](http://www.sinovotec.com). You can contact us at <ken.wu@zygcom.com>