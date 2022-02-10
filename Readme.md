# BLE Permissions changes in Android 12.

- This is as per my understanding i will update one tested 
- Please consider this one final versions. 
- I have used ActivityResult Api to provide run time permissions.

The BLUETOOTH_ADVERTISE, BLUETOOTH_CONNECT, and BLUETOOTH_SCAN permissions are runtime permissions. Therefore, you must explicitly request user approval in your app before you can look for Bluetooth devices, make a device discoverable to other devices, or communicate with already-paired Bluetooth devices. When your app requests at least one of these permissions, the system prompts the user to allow your app to access Nearby devices, as shown in figure 1.

The following code snippet demonstrates how to declare Bluetooth-related permissions in your app if it targets Android 12 or higher:

```aidl
        <manifest>
        <!-- Request legacy Bluetooth permissions on older devices. -->
        <uses-permission android:name="android.permission.BLUETOOTH"
                         android:maxSdkVersion="30" />
        <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"
                         android:maxSdkVersion="30" />
    
        <!-- Needed only if your app looks for Bluetooth devices.
             If your app doesn't use Bluetooth scan results to derive physical
             location information, you can strongly assert that your app
             doesn't derive physical location. -->
        <uses-permission android:name="android.permission.BLUETOOTH_SCAN" />
    
        <!-- Needed only if your app makes the device discoverable to Bluetooth
             devices. -->
        <uses-permission android:name="android.permission.BLUETOOTH_ADVERTISE" />
    
        <!-- Needed only if your app communicates with already-paired Bluetooth
             devices. -->
        <uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
    
        <!-- Needed only if your app uses Bluetooth scan results to derive physical location. -->
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        ...
    </manifest>
```




### Target Android 11 or lower
- If your app targets Android 11 (API level 30) or lower, declare the following permissions in your app's manifest file:
  - BLUETOOTH is necessary to perform any Bluetooth classic or BLE communication, such as requesting a connection, accepting a connection, and transferring data.
  - ACCESS_FINE_LOCATION is necessary because, on Android 11 and lower, a Bluetooth scan could potentially be used to gather information about the location of the user.
  - If your app targets Android 9 (API level 28) or lower, you can declare the ACCESS_COARSE_LOCATION permission instead of the ACCESS_FINE_LOCATION permission.


```aidl
     private var listOfPermissions= arrayOf(android.Manifest.permission.BLUETOOTH_ADVERTISE,android.Manifest.permission.BLUETOOTH_SCAN,android.Manifest.permission.BLUETOOTH_CONNECT,android.Manifest.permission.ACCESS_FINE_LOCATION,android.Manifest.permission.ACCESS_COARSE_LOCATION).toMutableList()
    
        private var deniedList= arrayListOf<String>().toMutableList()
        var isPermissionDenied=false
    
        private val requestPermission =
            registerForActivityResult(ActivityResultContracts.RequestMultiplePermissions()) { isGranted ->
                // Do something if permission granted
                isGranted.forEach {
                    if (it.value) {
                        Log.i("DEBUG", "${it.key} permission granted")
                    } else {
                        isPermissionDenied=true
                        deniedList.add(it.key)
                        Log.i("DEBUG", "${it.key} permission denied")
                    }
                }
    
                Log.i("DEBUG", "listOf Permissions: ${listOfPermissions.size}")
    
            }
            
  // just need to add one line to initialize , Runtime permissions
  
        requestPermission.launch(listOfPermissions.toTypedArray())
          

```

    Android Device / Permission.
    
    24..30
    android.permission.BLUETOOTH
    android.permission.BLUETOOTH_ADMIN
    android.permission.ACCESS_FINE_LOCATION
    
    
    31
    android.permission.BLUETOOTH_SCAN
    android.permission.BLUETOOTH_ADVERTISE
    android.permission.BLUETOOTH_CONNECT
    android.permission.ACCESS_FINE_LOCATION


- <B>NOTE My Findings , I will update this one is final or not once confirm</B>
-  found something like ,

- All three permissions comes under dangerous zone , and still need CONNECT or any permission to allow at run time and all three get enabled. (Granted ) or disallow any one will be disallow all three.

- we need to provide all thee for BLE as Peripheral and BLE as client.



    
     

 


