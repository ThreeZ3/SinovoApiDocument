# SinovoLib API Document
## Android Platform

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

#### Get bluethooth status
```
SinovoBle.getInstance().getBluetoothAdapter().isEnabled()
```

#### Enable bluethooth
```
SinovoBle.getInstance().getBluetoothAdapter().enable()
```

#### Disable bluethooth
```
SinovoBle.getInstance().getBluetoothAdapter().disable()
```

#### Ble scan by 10 second，but not connect
```
SinovoBle.getInstance().bleScanOnly()
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

#### Update lock user name
```
SinovoBle.getInstance().updateUserName(String userName, String userNID, String lockSNO, String lockMacAddress)
```
Example Return:
``
{"funCode":"03","errCode":"00","lockMac":"FE655103CE44","userNid":"01","username":"Username"}
``

#### Reset code
* Parmas:
  * codeType - DataType of this code
  * codeID - sid (Store ID of this code)
```
SinovoBle.getInstance().resetCode(String userNid, String codeType, String codeID, String newCode, String lockSNO, String lockMacAddress)
```
Example Return:
``
{"funCode":"0d","errCode":"00","lockMac":"FE655103CE44","userNid":"01","codeType":"01","sid":"01","code":"123456","opendata":"FE0A079F4805D6C6F06DF0296E398C0E41ADC501"}
``


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

#### Set properties of the lock
* Params:
  * data – value
  * dataType – \
    01 set lockname，1-10 characters\
    02 set time of the lock，format :YYMMDDHHMMSS\
    03 set auto-lock time ,0 means disable auto-lock\
    04 set volume, 00 mute , 01-05 volume\
    05 set auto-create user\
    07 set the lock into DFU OTA , update Firmware data – value\
    06 superuser's permission :\
    ** permission of share code: data is 01, 03, 05, 07, 09, 11, 13, 15\
    ** permission of manager user: data is 02，03，06，07，10，11，14, 15\
    ** permission of setting: data is 04，05，06，07，12，13，14，15\
    ** permission of checking log: data is 08，09，10，11，12，13，14, 15

```
SinovoBle.getInstance().setLockInfo(String dataType, String data, String lockSNO, String lockMacAddress)
```
dataType = 01, Example Return:
``
{"funCode":"11","errCode":"00","lockMac":"FE655103CE44","lockName":"SMART LOCK"}
``

#### Create a user, the default is a normal user
* Params:
  * userName – string
```
SinovoBle.getInstance().addUser(String userName, String lockSNO, String lockMacAddress)
```
Example Return:
``
{"funCode":"02","errCode":"00","lockMac":"FE655103CE44","userNid":"02","username":"User"}
``

#### Add a set of data for the user, password, card, fingerprint
* Params:
  * userNID – UserNid
  * data – The specific password content when adding a password, if it is adding a card/   fingerprint, leave it blank
  * dataType – type of data :\
    ** 02 Normal password\
    ** 03 Supervisor password\
    ** 06 Card\
    ** 07 Fingerprint\
    ** 08 Alarm Fingerprint
 ```
 SinovoBle.getInstance().addDataForUser(String userNID, String dataType, String data, String lockSNO, String lockMacAddress)
 ```
 dataType = 06, Example Return:
``
{"funCode":"05","errCode":"00","lockMac":"FE655103CE44","userNid":"02","dataType":"06","sid":"01","data":""}
``

#### Delete a certain item of data, delete a set of password, card, fingerprint, binding
* Params:
  * dataType – type of data
  * delID – sid (Store ID of this data)
```
 SinovoBle.getInstance().delData(String dataType, String delID, String lockSNO, String lockMacAddress)
```
Example Return:
``
{"funCode":"06","errCode":"00","lockMac":"FE655103CE44","dataType":"06","sid":"01"}
``

#### Clear data
* Params:
  * datakType – Type of data to be cleared :\
    ** 00 Clear users, do not delete administrators\
    ** 0c Restore the lock to factory settings
```
 SinovoBle.getInstance().cleanData(String datakType, String lockSNO, String lockMacAddress)
```
datakType = 0c, Example Return:
``
{"funCode":"0c","errCode":"00","lockMac":"FE655103CE44","dataType":"0c"}
``

#### Under the add lock operation, unbind the lock
```
SinovoBle.getInstance().cancelAddLock()
```

#### Verify password
* Params:
  * password-password
```
SinovoBle.getInstance().verifyCode(String password, String lockSNO, String lockMacAddress)
```
Example Return:
``
{"funCode":"08","errCode":"00","lockMac":"FE:65:51:03:CE:44","codeType":"01","userNid":"01","sid":"01","code":"785073"}
``

#### Modify the attributes of the password, change the normal password and the super user password
* Params:
  * oldCodeType - the original type of the password,
    02 normal password,
    03 super user password
  * codeID - the id of the password
  * newCodeType - new password type,
    02 normal password,
    03 super user password; if this field is empty, it means to query the type of this password
```
SinovoBle.getInstance().updateCodeType(String oldCodeType, String codeID, String newCodeType, String lockSNO, String lockMacAddress)
```
Example Return:
``
{"funCode":"07","errCode":"00","lockMac":"FE:65:51:03:CE:44","dataType":"03","sid":"02"}
``

#### Synchronization lock user data, including user information
```
SinovoBle.getInstance().getAllUsers(String lockSNO, String lockMacAddress)
```
Example Return:
``
{"funCode":"13","errCode":"00","lockMac":"FE:65:51:03:CE:44","dataType":"01","userNid":"01","sid":"01","syncData":"785073"}
``

#### Sync log
* Params:
  * logID - the current logID, support the synchronization from the specified id, if the logID is ff, all logs will be synchronized
```
SinovoBle.getInstance().getLog(String logID, String lockSNO, String lockMacAddress)
```
Example Return:
``
{"funCode":"17","logType":"09","logID":"03","logDate":"23-04-12","logTime":"15:28","userType":"01","logCont":"01"}
``


#### Enable/disable dynamic password
* Params:
  * dynamicCode - corresponding dynamic code
  * enable - 00 means disable, 01 means start
```
SinovoBle.getInstance().doDynamicCode(String dynamicCode, String enable, String lockSNO, String lockMacAddress)
```
Example Return:
``
{"funCode":"20","errCode":"00","lockMac":"FE:65:51:03:CE:44","codeStatus":"01","sharedCode":"22877542856"}
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

#### Get the file path of the DFU upgrade package
```
SinovoBle.getInstance().getAppFilePath()
```

#### DFU upgrade, lock firmware upgrade
* Params:
  * dfuPath - the file path of the DFU upgrade package
```
SinovoBle.getInstance().upgradeFW(Context context, String mac  ,String dfuPath)
```

#### Cancel DFU upgrade
```
SinovoBle.getInstance().cancelUpgrade(Context context);
```

### GWSmartConfig
#### Gateway configuration
* Params:
  * context - context
  * GWSmartConfigCallback - Gateway configuration callback
  * WifiSSID - Wifi SSID(Wifi Name)
  * WifiPass - Wifi Password
  ```
  GWSmartConfig.getInstance().startConifg(Context context, GWSmartConfigCallback GWSmartConfigCallback, String WifiSSID, String WifiPass)
  ```
   It can also be configured via Bluetooth
   ```
   SinovoBle.getInstance().configureGW(GWSmartConfigCallback GWSmartConfigCallback, String wifiSSID, String wifiPass)
   ```

#### Cancel Gateway configuration  
```
GWSmartConfig.getInstance().exitConfig()
```
```
SinovoBle.getInstance().finishConfigureGW()
```

### MqttLib
#### Initialize:
* Params:
  * context – context
  * proKey – produceKey
  * deName – DeviceName
  * deSecret – DeviceSecret
  * region – DeviceRegion
  * iotMqttCallback - iotMqttCallback
```
MqttLib.getInstance().init(Context context, String proKey, String deName, String deSecret, String region, iotMqttCallback callback)
```

#### Logout mqtt
```
MqttLib.getInstance().logoutMQTT()
```

#### Create a user, the default is a normal user
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * userName – string
```
MqttLib.getInstance().addUser(String gatewayid, String type, String uuid, String mac, String sno, String userName)
```
Example Return:
``
 {msg: {"funCode":"02","errCode":"00","lockMac":"FE:65:51:03:CE:44","userNid":"03","username":"New User03","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``

#### Reset code
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * userNid - the id of the user
  * codeType - the type of password
  * codeID - the Store ID of the password (sid)
  * newCode - new password
```
MqttLib.getInstance().resetCode(String gatewayid, String type, String uuid, String mac, String sno, String userNid, String codeType, String codeID, String newCode)
```
Example Return:
``
{msg: {"funCode":"0d","errCode":"00","lockMac":"FE:65:51:03:CE:44","userNid":"01","codeType":"01","sid":"01","code":"785078","opendata":"FE0A07F8C3AC23C090C1F49858722F5BC6D5385D","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``

#### Update lock user name
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * userNid - the id of the user
  * userName - string
```
MqttLib.getInstance().updateUserName(String gatewayid, String type, String uuid, String mac, String sno, String userName, String userNID)
```
Example Return:
``
{msg: {"funCode":"03","errCode":"00","lockMac":"FE:65:51:03:CE:44","userNid":"03","username":"YG","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``


#### Modify the attributes of the password, change the normal password and the super user password
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * oldCodeType - the original type of the password, 02 normal password, 03 super user password
  * codeID - the id of the password
  * newCodeType - new password type, 02 normal password, 03 super user password; if this field is empty, it means to query the type of this password
```
MqttLib.getInstance().updateCodeType(String gatewayid, String type, String uuid, String mac, String sno,String oldCodeType,String codeID, String newCodeType)
```
Example Return:
``
{msg: {"funCode":"07","errCode":"00","lockMac":"FE:65:51:03:CE:44","dataType":"03","sid":"02","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``

#### Add a set of data for the user, password, card, fingerprint
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * userNID – UserNid
  * password – The specific password content when adding a password, if it is adding a card/fingerprint, leave it blank
  * dataType – type of data :\
    ** 02 Normal password\
    ** 03 Supervisor password\
    ** 06 Card\
    ** 07 Fingerprint\
    ** 08 Alarm Fingerprint
 ```
 MqttLib.getInstance().addDataForUser(String gatewayid, String type, String uuid, String mac, String sno, String userNID, String dataType, String password)
 ```
 Example Return:
``
{msg: {"funCode":"05","errCode":"00","lockMac":"FE:65:51:03:CE:44","userNid":"02","dataType":"06","sid":"01","data":"","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``


#### Delete a certain item of data, delete a set of password, card, fingerprint, binding
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * dataType – type of data
  * delID – sid (Store ID of this data)
```
MqttLib.getInstance().delData(String gatewayid, String type, String uuid, String mac, String sno, String dataType, String delID)
```
Example Return:
``
{msg: {"funCode":"06","errCode":"00","lockMac":"FE:65:51:03:CE:44","dataType":"06","sid":"01","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``

#### Verify password
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * password-password
```
MqttLib.getInstance().verifyCode(String gatewayid, String type, String uuid, String mac, String sno, String password)
```
Example Return:
``
{msg: {"funCode":"08","errCode":"00","lockMac":"FE:65:51:03:CE:44","codeType":"01","userNid":"01","sid":"01","code":"785078","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``



#### Set properties of the lock
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * data – value
  * dataType – \
    01 set lockname,1-10 characters\
    02 set time of the lock，format :YYMMDDHHMMSS\
    03 set auto-lock time ,0 means disable auto-lock\
    04 set volume, 00 mute , 01-05 volume\
    05 set auto-create user\
    07 set the lock into DFU OTA , update Firmware data – value\
    06 superuser's permission :\
        ** permission of share code: data is 01, 03, 05, 07, 09, 11, 13, 15\
        ** permission of manager user: data is 02，03，06，07，10，11，14, 15\
        ** permission of setting: data is 04，05，06，07，12，13，14，15\
        ** permission of checking log: data is 08，09，10，11，12，13，14, 15
```
MqttLib.getInstance().setLock(String gatewayid, String type, String uuid, String mac, String sno, String dataType, String data)
```    
Example Return:
``
{msg: {"funCode":"09","errCode":"00","lockMac":"FE:65:51:03:CE:44","enable":"01","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``



#### Get information of the lock
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
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
    12 get lock info（power、status、auto-lock、volume、auto-create、superUser、    firmware）
```
MqttLib.getInstance().getLockInfo(String gatewayid, String type, String uuid, String mac, String sno, String dataType)
```
Example Return:
``
{msg: {"funCode":"2c","errCode":"00","lockMac":"FE:65:51:03:CE:44","power":20,"lockStatus":"00","autoLockTime":5,"volume":"05","autocreate":"01","superUserLevel":"01","fwVersion1":"01","fwVersion2":"02","fwVerTime":"221107","fwType":"05","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``




#### Lock opening and closing operation
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * unlockType – 00 Lock、01 Unlock
  * code - password
```
MqttLib.getInstance().toUnlock(String gatewayid, String type, String uuid, String mac, String sno, String unlockType, String code)
```
Example Return:
``
{msg: {"funCode":"0a","errCode":"00","lockMac":"FE:65:51:03:CE:44","opType":"01","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``





#### Clear data
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * datakType – Type of data to be cleared :\
    ** 00 Clear users, do not delete administrators\
    ** 0c Restore the lock to factory settings
```
 MqttLib.getInstance().cleanData(String gatewayid, String type, String uuid, String mac, String sno, String datakType)
```
Example Return:
``
{msg: {"funCode":"0c","errCode":"00","lockMac":"FE:65:51:03:CE:44","dataType":"00","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``


#### Enable/disable dynamic password
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * dynamicCode - corresponding dynamic code
  * enable - 00 means disable, 01 means start
```
MqttLib.getInstance().doDynamicCode(String gatewayid, String type, String uuid, String mac, String sno, String dynamicCode, String enable)
```
Example Return:
``
{msg: {"funCode":"20","errCode":"00","lockMac":"FE:65:51:03:CE:44","codeStatus":"00","sharedCode":"28767063126","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``


#### Synchronization lock user data, including user information
```
MqttLib.getInstance().getAllUsers(String gatewayid, String type, String uuid, String mac, String sno)
```
Example Return:
``
{msg: {"funCode":"13","errCode":"00","lockMac":"FE:65:51:03:CE:44","dataType":"01","userNid":"01","sid":"01","syncData":"785078","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``


#### Sync log
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
  * logID - the current logID, support the synchronization from the specified id, if the logID is ff, all logs will be synchronized
```
MqttLib.getInstance().getLog(String gatewayid, String type, String uuid, String mac, String sno, String logID)
```
Example Return:
``
{msg: {"funCode":"17","logType":"0b","logID":"08","logDate":"23-04-12","logTime":"15:45","userType":"06","logCont":"0602","appId":"534158825@qq.com","type":"bledata","uuid":"F6F7","gateway_id":"2462ABD177C8","mac":"FE655103CE44"}, topic: /a3Q9ncgAkZp/534158825@qq.com/user/get}
``



#### Push data to MQTT server
* Params:
  * jsonData - your data
```
MqttLib.getInstance().pushDataToMqtt(final String jsonData)
```
example: 
```
final String jsonData = "{"type": "updateDevice", "device_id": "FE655103CE44", "value": "value", "sender": "sender", "target_user": "123@qq.com"}"

MqttLib.getInstance().pushDataToMqtt(jsonData)
```

#### Notification lock disconnects Bluetooth connection
* Params:
  * gatewayid - gateway id
  * type - the type of data sent
  * uuid - UUID of the lock
  * mac - lock mac address
  * sno - lockSno
```
MqttLib.getInstance().toDisconnBle(String gatewayid, String type, String uuid, String mac, String sno)
```

#### Stop mqtt from sending commands
```
MqttLib.getInstance().stopMqttSendCmd()
```



### HttpLib
#### Initialize
* Params:
  * callBack - HttpLib callback
```
 HttpLib httpLib = HttpLib.getInstance(HttpLibCallback callBack);
```


#### Register new user
* Params:
  * account – User's account
  * password – user's password
```
HttpLib.getInstance(callBack).userRegister(String account, String password)
```
Example Return:
``
{"code":0,"msg":"request ok","auth":"12345667@qq.com","user_id":735}
``


#### login
* Params:
  * account – User's account
  * password – user's password
```
HttpLib.getInstance(callBack).userLogin(String account, String password)
```
Example Return:
``
{"code":0,"msg":"request ok","access_token":"df6188f4d92c5d606ba59b3bd20884283ff932da","expires_in":3600,"refresh_token":"2b81e604e6b13656133b4a5d9e60bce566037b13","user_id":360,"nickname":"13","age":"89","gender":"","location":"SZ","shake_unlock":"01","vibration":"01","avatar_url":"https:\/\/gws.qiksmart.com\/upload\/avatar\/16795548437733.jpg","app_productKey":"a3Q9ncgAkZp","app_deviceName":"534158825@qq.com","app_deviceSecret":"806a5b1682a5a940af90219838eef2d3","app_region":"us-west-1","app_subscribe_topic":"\/a3Q9ncgAkZp\/534158825@qq.com\/user\/get","app_publish_topic":"\/a3Q9ncgAkZp\/534158825@qq.com\/user\/update","user_imei":"87A6A4D9F1D3"}
``


#### Get the verification code before changing the password
* Params:
  * account – User's account
```
HttpLib.getInstance(callBack).getVerfyCode(String account)
```
Example Return:
``
{"code":0,"msg":"request ok"}
``

#### change password
* Params:
  * account – User's account
  * verifycode – the verification code
  * newPass – new password
```
HttpLib.getInstance(callBack).modifyPass(String account, String verifycode, String newPass)
```
Example Return:
``
{code: 0, msg: request ok, auth: 534158825@qq.com}
``

#### add gateway
* Params:
  * gatewayID – the gateway's ID
  * gatewayName – the gateway's name
```
HttpLib.getInstance(callBack).addGateway(String gatewayID, String gatewayName)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### delete gateway
* Params:
  * gatewayID – the gateway's ID
```
HttpLib.getInstance(callBack).delGateway(String gatewayID)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### change gateway's name
* Params:
  * gatewayID – the gateway's ID
  * gatewayName - string
```
HttpLib.getInstance(callBack).modifyGWName(String gatewayID, String gatewayName)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``


#### get the gateway's list
```
HttpLib.getInstance(callBack).getGatewayList()
```
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### Add lock to http server
```
for example:
{
"device_autoAuth":"01",
"device_qrcode":"222111090001",
"lock_password_type":"01",
"device_id":"FE655103CE44",
"device_mute":"05",
"nid":"01",
"device_type":"FM810",
"device_firmware":"1.2.211206",
"device_spPriv":"0f",
"lock_password":"280500",
"lock_password_valid":"Permanent",
"device_from":"Manually add",
"device_name":"FM810",
"device_autolock":"11",
"open_data":"FE0A07F6E16F59666EB7BB5B47C1B3398C3E5A67",
"device_basetime":"211218183753",
"open_back":"FE0A021A09373BA1D2A36D77DC5AEC247D60FCB8",
"device_sno":"fdd504",
"admin_user":"654321@qq.com"
}

HttpLib.getInstance(callBack).addLock(final JSONObject json)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### update information of the lock
```
for example:
{
"device_autoAuth":"01",
"device_qrcode":"222111090001",
"lock_password_type":"01",
"device_id":"FE655103CE44",
"device_mute":"05",
"nid":"01",
"device_type":"FM810",
"device_firmware":"1.2.211206",
"device_spPriv":"0f",
"lock_password":"280500",
"lock_password_valid":"Permanent",
"device_from":"Manually add",
"device_name":"FM810",
"device_autolock":"11",
"open_data":"FE0A07F6E16F59666EB7BB5B47C1B3398C3E5A67",
"device_basetime":"211218183753",
"open_back":"FE0A021A09373BA1D2A36D77DC5AEC247D60FCB8",
"device_sno":"fdd504",
"admin_user":"654321@qq.com"
}

HttpLib.getInstance(callBack).updateLock(final JSONObject json)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### Get the list of lock for the user
```
HttpLib.getInstance(callBack).getLockList()
```
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### delete lock from http server
* Params:
  * lockID - lockMac
```
HttpLib.getInstance(callBack).delLock(String lockID)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### Update the information of the login user
* Params:
  * shake_unlock - 01 enable shake unlock , 00 disable shake unlock
  * vibration - 01 enable lock/unlock vibration effect , 00 disable lock/unlock vibration effect
```
HttpLib.getInstance(callBack).updateLoginUserInfo(String nickname, String age, String gender, String address, String shake_unlock, String vibration)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### Update avatar for the login user
* Params:
  * bitmap – avatar
```
HttpLib.getInstance(callBack).updateUserAvatar(final Bitmap bitmap)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### Share the lock to other users
````
for example:
{
  "lock_password":"3616 6894 533",
  "lock_password_valid":"2021-12-28 13:52#2021-12-31 13:52",
  "device_from":"Shared by Bad Lock",
  "lock_password_type":"04",
  "device_id":"FE655103CE44",
  "nid":"DynamicUser04",
  "target_user":"369@qq.com",
  "device_sno":"fdd504"
  }

HttpLib.getInstance(callBack).shareLock(final JSONObject json)
````
Example Return:
``
{code: 0, msg: request ok, ...}
``


#### Add share data to http server
```
for example:
{
  "share_time":"28 Dec 2021 13:52:48",
  "share_to":"369@qq.com",
  "start_time":"2021-12-28 13:52",
  "code":"3616 6894 533",
  "device_id":"FE655103CE44",
  "share_type":1,
  "end_time":"2021-12-31 13:52",
  "share_by":"654321@qq.com",
  "device_basetime":"211218183753",
  "status":"01",
  "code_type":"One-Time"
  }

HttpLib.getInstance(callBack).addShareData(final JSONObject json)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``


#### Update shareData on http server
```
for example:
status - 00 disable password , 01 enable password
{
  "code":"0070 6804 1430 22",
  "device_id":"FE655103CE44",
  "status":"00"
  }

HttpLib.getInstance(callBack).updateShareData(final JSONObject json)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``




#### Get list of sharedData from http server
```
HttpLib.getInstance(callBack).getShareDataList(final JSONObject json)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### Delete shareData from http server
``` 
for example:
{
  "list":[{
    "code":"5959 6916 203",
    "device_id":"FE655103CE44"
    }]
  }

HttpLib.getInstance(callBack).delShareData(final JSONObject json)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### Delete the sub-device in the gateway
```
for example:
{"device_id":"FE655103CE44"}

HttpLib.getInstance(callBack).delGwSubLock(final JSONObject json)
``` 
Example Return:
``
{code: 0, msg: request ok, ...}
``


#### Reset lock
```
for example:
{"device_id":"FE655103CE44"}

HttpLib.getInstance(callBack).resetLock(final JSONObject json)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``


#### Get user avatar
* Params:
  * url – url of the user avatar
```
HttpLib.getInstance(callBack).getUserAvatar(String url)
``` 
Example Return:
``
{code: 0, msg: request ok, ...}
``

#### Get lock image
* Params:
  * url – url of the lock image
  * lockType - type of the lock
```
HttpLib.getInstance(callBack).getLockImage(String url, String lockType)
``` 
Example Return:
``
{code: 0, msg: request ok, ...}
``



#### Get lock type information
```
HttpLib.getInstance(callBack).getLockType()
``` 
Example Return:
``
{code: 0, msg: request ok, ...}
``



#### Get upgrade package information
* Params:
  * isRelease - true will get  from release channel , false will get  from debug channel
```
HttpLib.getInstance(callBack).getDFUInfo(boolean isRelease)
```
Example Return:
``
{code: 0, msg: request ok, ...}
``


#### Download the upgrade package of the current model lock
* Params:
  * filePath - the file path of the upgrade package
  * lockType - the type of the lock (ex:"FM810")
  * fmversion - firmware version of the upgrade package
  * savePath - save path
  * dfuMD5 - MD5 value of the upgrade package
  * isRelease - true will get  from release channel , false will get  from debug channel\
[These params can be obtained from getDFUInfo(boolean isRelease)]
```
HttpLib.getInstance(callBack).downloadDFUfile(String filePath, String lockType, String fmversion ,String savePath, String dfuMD5, boolean isRelease)
```


## Issues

Please file any issues, bugs or feature request as an issue on our [GitHub](https://github.com/konewu/SinovoLib) page. Commercial support is available if you need help with integration with your app or services. You can contact us at [ken.wu@zygcom.com](mailto:ken.wu@zygcom.com).


## Author

This plugin for App is developed by [Sinovotec](http://www.sinovotec.com). You can contact us at <ken.wu@zygcom.com>

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

#### Ble scan by 10 second，but not connect
* data callback: ``onLockFound :(BleLock *)bleDevice``
```
[[SinovoBle sharedBLE] bleScanOnly]
```

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

#### Update lock user name
* Params:
  * username - new username
  * userNID - the id of the user

* data callback: ``onUpdateUser :(NSMutableDictionary *)dict``
```
[[SinovoBle sharedBLE] updateUserName :(NSString *)username :(NSString *)userNID :(NSString *)lockSNO :(NSString *)lockMacAddress]
```

#### Reset code
* Parmas:
  * codeType - DataType of this code
  * codeID - sid (Store ID of this code)
  * userNID - the id of the user

* data callback: ``onUpdateUser :(NSMutableDictionary *)dict``
```
[[SinovoBle sharedBLE] resetCode :(NSString *)userNID :(NSString *)codeType :(NSString *)codeID :(NSString *)newCode :(NSString *)lockSNO :(NSString *)lockMacAddress]
```


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

#### Set properties of the lock
* Params:
  * data – value
  * dataType – \
     01 set lockname，1-10 characters\
    02 set time of the lock，format :YYMMDDHHMMSS\
    03 set auto-lock time ,0 means disable auto-lock\
    04 set volume, 00 mute , 01-05 volume\
    05 set auto-create user\
    07 set the lock into DFU OTA , update Firmware data – value\
    06 superuser's permission :\
    ** permission of share code: data is 01, 03, 05, 07, 09, 11, 13, 15\
    ** permission of manager user: data is 02，03，06，07，10，11，14, 15\
    ** permission of setting: data is 04，05，06，07，12，13，14，15\
    ** permission of checking log: data is 08，09，10，11，12，13，14, 15

* data callback: ``onLockInfo :(NSMutableDictionary *)dict``    
```
[[SinovoBle sharedBLE] setLockInfo :(int)dataType :(NSString *)data :(NSString *)lockSNO :(NSString *)lockMacAddress]
```

#### Create a user, the default is a normal user
* data callback: ``onCreateUser :(NSMutableDictionary *)dict``  
```
[[SinovoBle sharedBLE] addUser :(NSString *)username :(NSString *)lockSNO :(NSString *)lockMacAddress]
```

#### Add a set of data for the user, password, card, fingerprint
* Params:
  * userNID – UserNid
  * data – The specific password content when adding a password, if it is adding a card/fingerprint, leave it blank
  * dataType – type of data :\
    ** 02 Normal password\
    ** 03 Supervisor password\
    ** 06 Card\
    ** 07 Fingerprint\
    ** 08 Alarm Fingerprint

* data callback: ``onAddData :(NSMutableDictionary *)dict``      
 ```
[[SinovoBle sharedBLE] addDataForUser :(NSString *)userNID :(NSString *)dataType :(NSString *)data :(NSString *)lockSNO :(NSString *)lockMacAddress]
 ```

#### Delete a certain item of data, delete a set of password, card, fingerprint, binding
* Params:
  * dataType – type of data
  * delID – sid (Store ID of this data)

* data callback: ``onDelData :(NSMutableDictionary *)dict``      
```
[[SinovoBle sharedBLE] delData :(NSString *)dataType :(NSString *)delID :(NSString *)lockSNO :(NSString *)lockMacAddress]
```

#### Clear data
* Params:
  * dataType – Type of data to be cleared :\
    ** 00 Clear users, do not delete administrators\
    ** 0c Restore the lock to factory settings

* data callback: ``onCleanData :(NSMutableDictionary *)dict``      
```
[[SinovoBle sharedBLE] cleanData :(NSString *)dataType :(NSString *)lockSNO :(NSString *)lockMacAddress]
```

#### Under the add lock operation, unbind the lock
```
[[SinovoBle sharedBLE] cancelConnectLock]
```

#### Verify password
* Params:
  * code - password

* data callback: ``onVerifyCode :(NSMutableDictionary *)dict``    
```
[[SinovoBle sharedBLE] verifyCode :(NSString *)code :(NSString *)lockSNO :(NSString *)lockMacAddress]
```

#### Modify the attributes of the password, change the normal password and the super user password
* Params:
  * oldCodeType - the original type of the password, 02 normal password, 03 super user password
  * codeID - the id of the password
  * newCodeType - new password type, 02 normal password, 03 super user password; if this field is empty, it means to query the type of this password

* data callback: ``onUpdateUser :(NSMutableDictionary *)dict``   
```
[[SinovoBle sharedBLE] updateCodeType :(NSString *)oldCodeType :(NSString *)codeID :(NSString *)newCodeType :(NSString *)lockSNO :(NSString *)lockMacAddress]
```

#### Synchronization lock user data, including user information
* data callback: ``onRequestData :(NSMutableDictionary *)dict``   
```
[[SinovoBle sharedBLE] getAllUsers :(NSString *)lockSNO :(NSString *)lockMacAddress]
```

#### Sync log
* Params:
  * logID - the current logID, support the synchronization from the specified id, if the logID is ff, all logs will be synchronized

* data callback: ``onRequestLog :(NSMutableDictionary *)dict``     
```
[[SinovoBle sharedBLE] getLog :(NSString *)logID :(NSString *)lockSNO :(NSString *)lockMacAddress]
```


#### Enable/disable dynamic password
* Params:
  * code - corresponding dynamic code
  * enable - 00 means disable, 01 means start

* data callback: ``onDynamicCodeStatus :(NSMutableDictionary *)dict``
```
[[SinovoBle sharedBLE] doDynamicCode :(NSString *)code :(NSString *)enable :(NSString *)lockSNO :(NSString *)lockMacAddress]
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


### GWSmartConfig
To use ``startSmartconfig()`` and ``configureGW()`` on iOS >= 12, the ``Access WiFi information capability`` in XCode must be enabled. Otherwise, both methods will return null.

#### Gateway configuration
* Params:
  * wifiSSID - Wifi SSID(Wifi Name)
  * wifiBSSID - Wifi BSSID
  * password - Wifi Password
```
  [[GwConfigure sharedGwConfigure] startSmartconfig:(NSString *)wifiSSID :(NSString *)wifiBSSID :(NSString *)password];
```
such as :
````
- (void) startSmartconfig : (NSString *) wifiSSID : (NSString *) wifiBSSID : (NSString *) password{
 
    dispatch_queue_t seriaQueue = dispatch_queue_create("parallel", DISPATCH_QUEUE_CONCURRENT);
    dispatch_async(seriaQueue, ^{
        myDelegate.esptouchTask = [[ESPTouchTask alloc]initWithApSsid: wifiSSID andApBssid:wifiBSSID andApPwd:password];
        [myDelegate.esptouchTask setEsptouchDelegate:self._esptouchDelegate];
        [myDelegate.esptouchTask setPackageBroadcast:  YES];

        NSLog(@"wifissid:%@ ,bssid:%@", wifiSSID, wifiBSSID);

        NSArray *results = [myDelegate.esptouchTask executeForResults:1];

        NSLog(@"Gateway configure result : %@", results);
        
        

        ESPTouchResult *first = [results objectAtIndex:0];
        NSLog(@"Gateway configure result-first：%@", first);
        if (first.isCancelled) {
            NSLog(@"Cancel Gateway configuration");
            return;
        }

        if (first.isSuc) {
            NSString *gwID = [first.bssid uppercaseString];
            NSString *results = [NSString stringWithFormat:@"%@%@%@%@", @"Gateway configure success \n\n",resultf, @"\n\nGateway‘s BSSID: ",gwID];
        }

        // Gateway configure failed
        if (!first.isSuc && !first.isCancelled) {
            NSString *results = [NSString stringWithFormat:@"%@", @"Gateway configure timeout "];

        }
    });
}
````


#### Cancel Gateway configuration  
```
[[ESPTouchTask alloc] interrupt]
```

#### It can also be configured via Bluetooth
* Params:
  * wifiSSID - Wifi SSID(Wifi Name)
  * wifiPass - Wifi Password
  
* data callback: ``onConfigureGW :(NSMutableDictionary *)dict``     
 ````
 [[SinovoBle sharedBLE] configureGW :(NSString *)wifiSSID :(NSString *)wifiPass]
 ````

#### Cancel Gateway configuration  
```
[[SinovoBle sharedBLE] finishConfigureGW]
```

#### SinovoBle others callback
* ``onBleStatusUnknown`` - Bluetooth status unknown
* ``onBluetoothOn`` - Bluetooth enabled
* ``onBluetoothOff`` - Bluetooth disabled
* ``onScanOnlyFinish`` - Bluetooth scan completed in ten seconds
* ``onConnectLockViaQRCodeTimeOut`` - Connection timed out
* ``onConnectFailure`` - Connection failed
* ``onScanNothing20s`` - The specified lock cannot be searched within 20 seconds
* ``onBleDisconnect :(CBPeripheral *)peripheral`` - Connection disconnected
  


### MqttInstance
#### Initialize: 
* Params:
  * productKey – produceKey
  * deviceName – DeviceName
  * deviceSecret – DeviceSecret
  * region – DeviceRegion
````
MqttInstance *mqttInstance = [MqttInstance sharedMqtt];

[mqttInstance MqttInit :(NSString *)productKey :(NSString *)deviceName :(NSString *)deviceSecret :(NSString *)region]
````

#### Initialize MqttCallBack:
```
-(void) InitMqttCallback{
    [MqttInstance sharedMqtt].delegate = self;
}

[[MqttCallBack sharedMqttCallBack] InitMqttCallback];
```


#### Unsubscribe topic
````
[[MqttCallBack sharedMqttCallBack] unSubscriptTopic]
````


#### Logout mqtt
```
[[MqttInstance sharedMqtt] logoutMQTT]
```

#### Create a user, the default is a normal user
* Params:
  * gatewayid - gateway id , eg: 3C61052AD7FC
  * mac - lock mac address
  * sno - lockSno 
  * userName – user name
```
[[MqttInstance sharedMqtt] addUser :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(NSString *)userName]
```

#### Reset code
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * userNid - the id of the user
  * codeType - the type of password
  * codeID - the Store ID of the password (sid)
  * newCode - new password
```
[[MqttInstance sharedMqtt] resetCode :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(NSString *)userNid :(NSString *)codeType :(NSString *)codeID :(NSString *)newCode]
```

#### Update lock user name
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * userNid - the id of the user
  * userName - new user name
```
[[MqttInstance sharedMqtt] updateUserName :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(NSString *)userName :(NSString *)userNid]
```

#### Modify the attributes of the password, change the normal password and the super user password
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * oldCodeType - the original type of the password, 02 normal password, 03 super user password
  * codeID - the id of the password
  * newCodeType - new password type, 02 normal password, 03 super user password; if this field is empty, it means to query the type of this password
```
[[MqttInstance sharedMqtt] updateCodeType :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(NSString *)oldCodeType :(NSString *)codeID :(NSString *)newCodeType]
```

#### Add a set of data for the user, password, card, fingerprint
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * userNid – UserNid
  * password – The specific password content when adding a password, if it is adding a card/fingerprint, leave it blank
  * dataType – type of data :\
    ** 02 Normal password\
    ** 03 Supervisor password\
    ** 06 Card\
    ** 07 Fingerprint\
    ** 08 Alarm Fingerprint
 ```
[[MqttInstance sharedMqtt] addDataForUser :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(NSString *)userNid :(NSString *)dataType :(NSString *)password]
 ```


#### Delete a certain item of data, delete a set of password, card, fingerprint, binding
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * dataType – type of data
  * delID – sid (Store ID of this data)
```
[[MqttInstance sharedMqtt] delData :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(NSString *)dataType :(NSString *)delID]
```

#### Verify password
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * password - password
```
[[MqttInstance sharedMqtt] verifyCode :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(NSString *)password]
```

#### Set properties of the lock
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * data – value
  * datatype – \
    01 set lockname,1-10 characters\
    02 set time of the lock，format :YYMMDDHHMMSS\
    03 set auto-lock time ,0 means disable auto-lock\
    04 set volume, 00 mute , 01-05 volume\
    05 set auto-create user\
    07 set the lock into DFU OTA , update Firmware data – value\
    06 superuser's permission :\
        ** permission of share code: data is 01, 03, 05, 07, 09, 11, 13, 15\
        ** permission of manager user: data is 02，03，06，07，10，11，14, 15\
        ** permission of setting: data is 04，05，06，07，12，13，14，15\
        ** permission of checking log: data is 08，09，10，11，12，13，14, 15
```
[[MqttInstance sharedMqtt] setLockInfo :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(int)datatype :(NSString *)data]
```    

#### Get information of the lock
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * datatype – \
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
    12 get lock info（power、status、auto-lock、volume、auto-create、superUser、    firmware）
```
[[MqttInstance sharedMqtt] getLockInfo :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(int)datatype]
```

#### Lock opening and closing operation
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * unlockType – 00 Lock、01 Unlock
  * code - password
```
[[MqttInstance sharedMqtt] toUnlock :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(NSString *)unlockType :(NSString *)code]
```

#### Clear data
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * dataType – Type of data to be cleared :\
    ** 00 Clear users, do not delete administrators\
    ** 0c Restore the lock to factory settings
```
[[MqttInstance sharedMqtt] cleanData :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(NSString *)dataType]
```

#### Enable/disable dynamic password
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * dynamicCode - corresponding dynamic code\
  * enable - 00 means disable, 01 means start
```
[[MqttInstance sharedMqtt] doDynamicCode :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(NSString *)dynamicCode :(NSString *)enable]
```

#### Synchronization lock user data, including user information
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
```
[[MqttInstance sharedMqtt] getAllUsers :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno]
```

#### Sync log
* Params:
  * gatewayid - gateway id
  * mac - lock mac address
  * sno - lockSno
  * logID - the current logID, support the synchronization from the specified id, if the logID is ff, all logs will be synchronized
```
[[MqttInstance sharedMqtt] getLog :(NSString *)gatewayid :(NSString *)mac :(NSString *)sno :(NSString *)logID]
```

#### Push data to MQTT server or other user
* Params:
  * dict - your data
```
[[MqttInstance sharedMqtt] pushDataToOthers :(NSMutableDictionary *)dict]
```
example: 
```
NSMutableDictionary *dict = {@"type": @"updateDevice", @"device_id": @"FE655103CE44", @"value": @"value", @"sender": @"sender", @"target_user": @"123@qq.com"};

[[MqttInstance sharedMqtt] pushDataToOthers :dict];
```

#### Disconnect the Bluetooth connection between the gateway and the lock
* Params:
  * gatewayid : the gateway's id , eg: 3C61052AD7FC
  * macList   : NSMutableArray,  An array formed by the mac addresses of multiple locks
```
[[MqttInstance sharedMqtt] disconnectLock :(NSString *)gatewayid :(NSMutableArray *)macList]
```

#### Stop mqtt from sending commands
```
[[MqttInstance sharedMqtt] stopMqttSendCmd]
```


#### MqttInstance callback
* ``onMqttInitSuccess`` - Mqtt init success
* ``onMqttInitFailed`` - Mqtt init failed
* ``onSubcribeSuccess`` - Subcribe success
* ``onSubcribeFailed`` - Subcribe failed
* ``onPublishSuccess`` - Publish success
* ``onPublishFailed`` - Publish failed
* ``onPublishTimeOut`` - Publish timeOut
* ``onLogoutSuccess`` - Logout mqtt success
* ``onLogoutFailed`` -  Logout mqtt failed
* ``onConnected`` - Mqtt connected
* ``onConnectionLost`` - Mqtt disconnect
* ``onMsgArrived :(NSString *)topic :(id) msg`` - Received data





### HttpLib

#### Register new user
* Params:
  * account – User's account
  * password – user's password

* data callback: ``onUserRegister  :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] userRegister :(NSString *)account :(NSString *)password]
```

#### login
* Params:
  * account – User's account
  * password – user's password

* data callback: ``onUserLoign  :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] userLogin :(NSString *)account :(NSString *)password]
```

#### Get the verification code before changing the password
* Params:
  * account – User's account

* data callback: ``onGetVerifyCode  :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] getVerfyCode :(NSString *)account]
```

#### change password
* Params:
  * account – User's account
  * verifycode – the verification code
  * newPass – new password

* data callback: ``onModifyPass  :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] modifyPass :(NSString *)account :(NSString *)verifycode :(NSString *)newPass]
```

#### add gateway
* Params:
  * gatewayID – the gateway's ID
  * gatewayName – the gateway's name

* data callback: ``onAddGateway :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] addGateway :(NSString *)gatewayID :(NSString *)gatewayName]
```

#### delete gateway
* Params:
  * gatewayID – the gateway's ID

* data callback: ``onDelGateway :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] delGateway :(NSString *)gatewayID]
```

#### change gateway's name
* Params:
  * gatewayID – the gateway's ID
  * gatewayName - string

* data callback: ``onModifyGwName :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] modifyGWName :(NSString *)gatewayID :(NSString *)gatewayName]
```


#### get the gateway's list
* data callback: ``onGetGwList  :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] getGatewayList]
```

#### Add lock to http server
* data callback: ``onAddLock :(NSDictionary *)pNSDictionary``
```
for example:
{
"device_autoAuth":"01",
"device_qrcode":"222111090001",
"lock_password_type":"01",
"device_id":"FE655103CE44",
"device_mute":"05",
"nid":"01",
"device_type":"FM810",
"device_firmware":"1.2.211206",
"device_spPriv":"0f",
"lock_password":"280500",
"lock_password_valid":"Permanent",
"device_from":"Manually add",
"device_name":"FM810",
"device_autolock":"11",
"open_data":"FE0A07F6E16F59666EB7BB5B47C1B3398C3E5A67",
"device_basetime":"211218183753",
"open_back":"FE0A021A09373BA1D2A36D77DC5AEC247D60FCB8",
"device_sno":"fdd504",
"admin_user":"654321@qq.com"
}

[[HttpLib sharedHttpLib] addLock :(NSMutableDictionary *)jsonData]
```

#### update information of the lock
* data callback: ``onUpdateLock :(NSDictionary *)pNSDictionary``
```
for example:
{
"device_autoAuth":"01",
"device_qrcode":"222111090001",
"lock_password_type":"01",
"device_id":"FE655103CE44",
"device_mute":"05",
"nid":"01",
"device_type":"FM810",
"device_firmware":"1.2.211206",
"device_spPriv":"0f",
"lock_password":"280500",
"lock_password_valid":"Permanent",
"device_from":"Manually add",
"device_name":"FM810",
"device_autolock":"11",
"open_data":"FE0A07F6E16F59666EB7BB5B47C1B3398C3E5A67",
"device_basetime":"211218183753",
"open_back":"FE0A021A09373BA1D2A36D77DC5AEC247D60FCB8",
"device_sno":"fdd504",
"admin_user":"654321@qq.com"
}

[[HttpLib sharedHttpLib] updateLock :(NSMutableDictionary *)jsonData]
```

#### Get the list of lock for the user
* data callback: ``onGetLockList:(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] getLockList]
```

#### delete lock from http server
* Params:
  * lockId - lockMac

* data callback: ``onDelLock :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] delLock :(NSString *)lockId]
```

#### Update the information of the login user
* Params:
  * shake_unlock - 01 enable shake unlock , 00 disable shake unlock
  * vibration - 01 enable lock/unlock vibration effect , 00 disable lock/unlock vibration effect

* data callback: ``onUpdateLoginUserInfo :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] updateLoginUserInfo :(NSString *)nickname :(NSString *)age :(NSString *)address :(NSString *)shake_unlock :(NSString *)vibration]
```

#### Update avatar for the login user
* Params:
  * imgBase64 – avatar data

* data callback: ``onUpdateUserAvatar :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] updateUserAvatar :(NSString *)imgBase64]
```

#### Share the lock to other users
* data callback: ``onShareLock :(NSDictionary *)pNSDictionary``
````
for example:
{
  "lock_password":"3616 6894 533",
  "lock_password_valid":"2021-12-28 13:52#2021-12-31 13:52",
  "device_from":"Shared by Bad Lock",
  "lock_password_type":"04",
  "device_id":"FE655103CE44",
  "nid":"DynamicUser04",
  "target_user":"369@qq.com",
  "device_sno":"fdd504"
  }

[[HttpLib sharedHttpLib] shareLock :(NSMutableDictionary *)jsondata]
````


#### Add share data to http server
* data callback: ``onAddShareData :(NSDictionary *)pNSDictionary``
```
for example:
{
  "share_time":"28 Dec 2021 13:52:48",
  "share_to":"369@qq.com",
  "start_time":"2021-12-28 13:52",
  "code":"3616 6894 533",
  "device_id":"FE655103CE44",
  "share_type":1,
  "end_time":"2021-12-31 13:52",
  "share_by":"654321@qq.com",
  "device_basetime":"211218183753",
  "status":"01",
  "code_type":"One-Time"
  }

[[HttpLib sharedHttpLib] addShareData :(NSMutableDictionary *)jsondata]
```


#### Update shareData on http server
* data callback: ``onUpdateShareData :(NSDictionary *)pNSDictionary``
```
for example:
status - 00 disable password , 01 enable password
{
  "code":"0070 6804 1430 22",
  "device_id":"FE655103CE44",
  "status":"00"
  }

[[HttpLib sharedHttpLib] updateShareData :(NSMutableDictionary *)jsondata]
```

#### Get list of sharedData from http server
* data callback: ``onGetShareData :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] getShareDataList :(NSMutableDictionary *)jsondata]
```

#### Delete shareData from http server
* data callback: ``onDelShareData :(NSDictionary *)pNSDictionary``
``` 
for example:
{
  "list":[{
    "code":"5959 6916 203",
    "device_id":"FE655103CE44"
    }]
  }

[[HttpLib sharedHttpLib] delShareData :(NSString *)jsondata]
```

#### Delete the sub-device in the gateway
* Params:
  * lockID – lock mac address

* data callback: ``onDelGwSubLock :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] delGWsubLock :(NSString *)lockID]
``` 


#### Reset lock
* Params:
  * lockID – lock mac address

* data callback: ``onResetLock :(NSDictionary *)pNSDictionary``
```
[[HttpLib sharedHttpLib] resetLock :(NSString *)lockID]
``` 


#### Get user avatar
* Params:
  * url – url of the user avatar

* data callback: ``onGetUserAvatar :(id)object``
```
[[HttpLib sharedHttpLib] getUserAvatar :(NSString *)url]
``` 

#### Get lock image
* Params:
  * url – url of the lock image
  * lockType - type of the lock

* data callback: ``onGetLockImage :(id)object``
```
[[HttpLib sharedHttpLib] getLockImage :(NSString *)url :(NSString *)lockType]
``` 


#### Get lock type information
* data callback: ``onGetLockType :(id)object``
```
[[HttpLib sharedHttpLib] getLockType]
``` 


#### Get upgrade package information
* Params:
  * isRelease - YES will get  from release channel , NO will get  from debug channel

* data callback: ``onGetDFUInfo :(id)object``
```
[[HttpLib sharedHttpLib]  getDFUInfo :(BOOL)isRelease]
```


#### Download the upgrade package of the current model lock
* Params:
  * url - the url of the upgrade package
  * isRelease - YES will get  from release channel , NO will get  from debug channel\
[These params can be obtained from getDFUInfo(boolean isRelease)]

* data callback: ``onDownloadDfuFile :(NSString *)filepath``
```
[[HttpLib sharedHttpLib] downloadDfuFile :(BOOL)isRelease :(NSString *)url]
```



### DFU upgrade, lock firmware upgrade
for example:
* Start to DFU upgrade :
```
myDelegate.isDFUMode = YES; // enter dfu upgrade mode

[[SinovoBle sharedBLE] startToDFU]
```

* Then：
  * dfuFilePath: dfuFilePath is obtained from ``onDownloadDfuFile :(NSString *)filepath`` 
  

```
- (void)onLockFound:(nonnull BleLock *)bleDevice {
    NSLog(@"result: qrcode：%@， name：%@, uuid:%@", bleDevice.qrCode, bleDevice.deviceName, bleDevice.uuid);
    
    // .......... do something
  
    // if in dfu upgrade mode
    if (myDelegate.isDFUMode) {
        NSFileManager *fileManager = [[NSFileManager alloc] init];
        if ([fileManager fileExistsAtPath:myDelegate.dfuFilePath]) {
            if ([bleDevice.deviceName isEqualToString:@"lockDFU"]) {
                [self uploadFileToBlueDevice :[NSURL fileURLWithPath:myDelegate.dfuFilePath] :bleDevice.mPeripheral];
                return;
            }
        }else {

            NSLog(@"DFU file does not exist：%@",myDelegate.dfuFilePath);
    
        }
    }
    
}
```


```

/**
   *****   Execute the upgrade file send to firmware operation *****
*/

- (void)uploadFileToBlueDevice:(NSURL *)filePath :(CBPeripheral *)peripheral{

    NSLog(@"Start uploading the dfu upgrade file to the lock %@", filePath);
    DFUFirmware *selectedFirmware  = [[DFUFirmware alloc] initWithUrlToZipFile:filePath];
    DFUServiceInitiator *initiator = [[DFUServiceInitiator alloc] initWithCentralManager: [SinovoBle sharedBLE].mCentral target:peripheral];
    [initiator withFirmware :selectedFirmware];

    initiator.logger            = self; // - to get log info
    initiator.delegate          = self; // - to be informed about current state and errors
    initiator.progressDelegate  = self; // - to show progress bar
    
    myDelegate.DFUcontroller = [initiator startWithTarget:peripheral];
}
```  


```
#pragma mark - LoggerDelegate
- (void)logWith:(enum LogLevel)level message:(NSString *)message{
    NSLog(@"logWith---------level = %ld,-------message,%@",(long)level,message);
}
```


```
#pragma mark - DFUServiceDelegate
```
  
```
- (void)dfuStateDidChangeTo:(enum DFUState)state{
    NSLog(@"dfuStateDidChangeTo-----------state = %ld",(long)state);
    if (state == 0) {
        NSLog(@"Connecting lock");
    }
    if (state == 1) {
        NSLog(@"Start to uploading");
    }
    if (state == 2) {
        NSLog(@"onDFUEnablingDfuMode");
    }
    if (state == 3) {
        NSLog(@"onDfuUploading");
    }
    if (state == 4) {
        NSLog(@"onDFUFirmwareValidating");
    }
    if (state == 5) {
        NSLog(@"onDFUDeviceDisconnecting");
    }
    if (state == 6) {
        NSLog(@"onDfuCompleted");

        // dfu upgrade finish
        [[SinovoBle sharedBLE] finishiDFU];
        myDelegate.isDFUMode = NO;
        myDelegate.dfuFilePath = @"";
    }
    if(state == 7){
      NSLog(@"onDfuAborted");
    }

}
```  


```
  // Upgrade failed

- (void)dfuError:(enum DFUError)error didOccurWithMessage:(NSString *)message {
    NSLog(@"dfuError-----------error = %ld,-------------message = %@",(long)error,message);
    
    // dfu upgrade exit
    [[SinovoBle sharedBLE] finishiDFU];
    myDelegate.isDFUMode = NO;
    myDelegate.dfuFilePath = @"";

}
```


#### Cancel DFU upgrade
```
// for example:

myDelegate.DFUcontroller.abort;
```


## Issues

Please file any issues, bugs or feature request as an issue on our [GitHub](https://github.com/konewu/iosSinovoLibrary) page. Commercial support is available if you need help with integration with your app or services. You can contact us at [ken.wu@zygcom.com](mailto:ken.wu@zygcom.com).


## Author

This plugin for App is developed by [Sinovotec](http://www.sinovotec.com). You can contact us at <ken.wu@zygcom.com>