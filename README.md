# PEKO

[![Build Status](https://travis-ci.org/deva666/Peko.svg?branch=master)](https://travis-ci.org/deva666/Peko) [![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-Peko-blue.svg?style=flat)](https://android-arsenal.com/details/1/6861) [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
---
### PErmissions in KOtlin

#### Android Permissions with Kotlin Coroutines
No more callbacks, builders, listeners or verbose code for requesting Android permissions.
Request permissions with one function call, thanks to Kotlin Coroutines.

***

### Installation

Add `jcenter` repository

```
compile 'com.markodevcic.peko:peko:0.1'
```

Example in Android Activity:
```kotlin
launch (UI) {
    val permissionResultDeferred = Peko.requestPermissionsAsync(this, Manifest.permission.BLUETOOTH, Manifest.permission.WRITE_EXTERNAL_STORAGE) 
    val permissionResult = permissionResultDeferred.await()
    
    if (permissionResult.grantedPermissions.contains(Manifest.permission.BLUETOOTH)) {
        //we have Bluetooth permission
    } else {
        //no Bluetooth permission
    }
}
```

If you want to show a permission rationale to the user, you can use the built in `AlertDialogPermissionRationale`. This will show an Alert Dialog with your message and title, explaining to user why this rationale is needed. It will be shown only once and only if user denies the permission for the first time.

```kotlin
val rationale = AlertDialogPermissionRationale(this@MainActivity) {
    this.setTitle("Need permissions")
    this.setMessage("Please give permissions to use this feature")	
}

launch (UI) {
    val permissionResult = Peko.requestPermissionsAsync(this, Manifest.permission.BLUETOOTH, rationale = rationale).await()
}
```

There is also a `SnackBarRationale` class that shows a SnackBar when permission rationale is required.

```kotlin
val snackBar = Snackbar.make(rootView, "Permissions needed to continue", Snackbar.LENGTH_LONG)
val snackBarRationale = SnackBarRationale(snackBar, "Request again")

launch(UI) {
    val permissionResult = Peko.requestPermissionsAsync(this@MainActivity, *permissions, rationale = snackBarRationale).await()
}
```

You can also show your own implementation of Permission Rationale to the user. Just implement the interface `PermissionRationale`. If `true` is returned from suspend function `shouldRequestAfterRationaleShownAsync`, Permission Request will be repeated, otherwise the permission request completes and returns the current permission result.


## License
```text
Copyright 2018 Marko Devcic

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```