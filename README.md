# OpenCV Mixed-signal Processing

opencvmixedsignalprocessing-melvincabatuan created by Classroom for GitHub

This project illustrates OpenCV Mixed-signal Processing, i.e Native and Java, in Android Studio.

## Prerequisite:

1. Download and extract OpenCV 3.0 for Android from http://opencv.org/.
2. Download and setup Android NDK build tools: http://developer.android.com/ndk/guides/setup.html
3. Create a new project in Android Studio with "No Activity".

![alt tag](https://github.com/DeLaSalleUniversity-Manila/opencvcamerapreviewsample-melvincabatuan/blob/master/OpenCV_001.png)

![alt tag](https://github.com/DeLaSalleUniversity-Manila/opencvcamerapreviewsample-melvincabatuan/blob/master/OpenCV_002.png)

![alt tag](https://github.com/DeLaSalleUniversity-Manila/opencvcamerapreviewsample-melvincabatuan/blob/master/OpenCV_003.png)

## Procedures:

1. Create a folder called "libraries" inside your Android Studio project.

2. Copy the entire content of 'OpenCV-android-sdk/sdk/java' into "libraries" directory created in Procedure 2. 

3. Rename 'AndroidStudioProjects/OpenCV3-MixedProcessing/libraries/java' into 'AndroidStudioProjects/OpenCV3-MixedProcessing/libraries/opencv' 

4. Create a 'build.gradle' file inside 'AndroidStudioProjects/OpenCV3-MixedProcessing/libraries/opencv'
Ex. 

```gradle
apply plugin: 'android-library'

buildscript {
  repositories {
    mavenCentral()
  }
  dependencies {
    classpath 'com.android.tools.build:gradle:1.3.0'
  }
}


android {
    compileSdkVersion 22
    buildToolsVersion "22.0.1"

    defaultConfig {
    	minSdkVersion 15
    	targetSdkVersion 22
    	versionCode 3000
    	versionName "3.0.0"
    }
	
	sourceSets {
    	main {
      		manifest.srcFile 'AndroidManifest.xml'
      		java.srcDirs = ['src']
      		resources.srcDirs = ['src']
      		res.srcDirs = ['res']
      		aidl.srcDirs = ['src']
    	}
	}
}
```
Note: This file is subject to your own 'compileSdkVersion','buildToolsVersion','targetSdkVersion', etc.

* Go to 'File/Project Structure' and inside 'Modules' click 'app', then from the Tab pick 'Dependencies', click '+' to add new dependency, pick 'Module Dependency', and add 'library:opencv' dependency to your project, then click OK.

![alt tag](https://github.com/DeLaSalleUniversity-Manila/opencvcamerapreviewsample-melvincabatuan/blob/master/OpenCV_004.png)

## Adding the Sample Project

1. Go to 'OpenCV-android-sdk/samples', pick a sample project, e.x. 'tutorial-2-mixedprocessing'.  

2. Delete the 'res' folder inside your own project app/src/main, then place the res folder from the samples inside your app/src/main folder.

3. Delete the 'java' folder from app/src/main, then copy the 'src' folder from the samples in there. Rename the copied 'src' folder into 'java'.

4. Replace the manifest inside your own project using the samples manifest.

E.x.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="org.opencv.samples.tutorial2"
    android:versionCode="301"
    android:versionName="3.01">

    <application
        android:label="@string/app_name"
        android:icon="@drawable/icon"
        android:theme="@android:style/Theme.NoTitleBar.Fullscreen" >

        <activity android:name="Tutorial2Activity"
            android:label="@string/app_name"
            android:screenOrientation="landscape"
            android:configChanges="keyboardHidden|orientation">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

    <supports-screens android:resizeable="true"
        android:smallScreens="true"
        android:normalScreens="true"
        android:largeScreens="true"
        android:anyDensity="true" />

    <uses-sdk android:minSdkVersion="8" />

    <uses-permission android:name="android.permission.CAMERA"/>

    <uses-feature android:name="android.hardware.camera" android:required="false"/>
    <uses-feature android:name="android.hardware.camera.autofocus" android:required="false"/>
    <uses-feature android:name="android.hardware.camera.front" android:required="false"/>
    <uses-feature android:name="android.hardware.camera.front.autofocus" android:required="false"/>

</manifest>
```

## Possible Errors:

* OpenCV.mk not found Error 

```shell
~/AndroidStudioProjects/OpenCV3-MixedProcessing/app/jni$ ndk-build
/home/cobalt/AndroidStudioProjects/OpenCV3-MixedProcessing/app/jni/Android.mk:5: ../../sdk/native/jni/OpenCV.mk: No such file or directory
make: *** No rule to make target `../../sdk/native/jni/OpenCV.mk'.  Stop.
```

* Solution:

Modify 'Android.mk' include as follows:

```make
include /path/to/your/OpenCV-android-sdk/sdk/native/jni/OpenCV.mk
```

Ex.
```make
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)

include /home/cobalt/Android/OpenCV-android-sdk/sdk/native/jni/OpenCV.mk

LOCAL_MODULE    := mixed_sample
LOCAL_SRC_FILES := jni_part.cpp
LOCAL_LDLIBS +=  -llog -ldl

include $(BUILD_SHARED_LIBRARY)
```
## Compile JNI:

```shell
~/AndroidStudioProjects/OpenCV3-MixedProcessing/app/jni$ ndk-build
Android NDK: WARNING:/home/cobalt/AndroidStudioProjects/OpenCV3-MixedProcessing/app/jni/Android.mk:mixed_sample: non-system libraries in linker flags: -lopencv_java3    
Android NDK:     This is likely to result in incorrect builds. Try using LOCAL_STATIC_LIBRARIES    
Android NDK:     or LOCAL_SHARED_LIBRARIES instead to list the library dependencies of the    
Android NDK:     current module    
[armeabi-v7a] Compile++ thumb: mixed_sample <= jni_part.cpp
[armeabi-v7a] SharedLibrary  : libmixed_sample.so
/home/cobalt/Android/adt-bundle-linux-x86-20131030/android-ndk-r10d/toolchains/arm-linux-androideabi-4.8/prebuilt/linux-x86/bin/../lib/gcc/arm-linux-androideabi/4.8/../../../../arm-linux-androideabi/bin/ld: warning: hidden symbol '__aeabi_atexit' in /home/cobalt/Android/adt-bundle-linux-x86-20131030/android-ndk-r10d/sources/cxx-stl/gnu-libstdc++/4.8/libs/armeabi-v7a/thumb/libgnustl_static.a(atexit_arm.o) is referenced by DSO /home/cobalt/Android/OpenCV-android-sdk/sdk/native/jni/../libs/armeabi-v7a/libopencv_java3.so
[armeabi-v7a] Install        : libmixed_sample.so => libs/armeabi-v7a/libmixed_sample.so
```

##Modify app:build.gradle sourceSets as follows:

```gradle
apply plugin: 'com.android.application'

android {
    compileSdkVersion 22
    buildToolsVersion "22.0.1"

    defaultConfig {
        applicationId "org.opencv.samples.tutorial1.opencv3_camerapreview"
        minSdkVersion 15
        targetSdkVersion 22
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }

    sourceSets {
        main {
            jni.srcDirs = []
            jniLibs.srcDirs=['libs']
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:22.2.1'
    compile project(':libraries:opencv')
}
```


## Accept

To accept the assignment, click the following URL:

https://classroom.github.com/assignment-invitations/e42ab762abc51ee1d98206feb6bff10b

## Sample Solution:

https://github.com/DeLaSalleUniversity-Manila/opencvmixedsignalprocessing-melvincabatuan

## Submission Procedure with Git: 

```shell
$ cd /path/to/your/android/app/
$ git init
$ git add –all
$ git commit -m "your message, e.x. Assignment 1 submission"
$ git remote add origin <Assignment link copied from assignment github, e.x. https://github.com/DeLaSalleUniversity-Manila/secondactivityassignment-melvincabatuan.git>
$ git push -u origin master
<then Enter Username and Password>
```


## Screenshot:

### RGB Preview and Menu

![alt tag](https://github.com/DeLaSalleUniversity-Manila/opencvmixedsignalprocessing-melvincabatuan/blob/master/device-2015-11-02-113102.png)

### Grayscale

![alt tag](https://github.com/DeLaSalleUniversity-Manila/opencvmixedsignalprocessing-melvincabatuan/blob/master/device-2015-11-02-113128.png)

### Canny-Edge Detection

![alt tag](https://github.com/DeLaSalleUniversity-Manila/opencvmixedsignalprocessing-melvincabatuan/blob/master/device-2015-11-02-113151.png)

### Keypoints

![alt tag](https://github.com/DeLaSalleUniversity-Manila/opencvmixedsignalprocessing-melvincabatuan/blob/master/device-2015-11-02-113227.png)

"*Nothing is wholly obvious without becoming enigmatic. Reality itself is too obvious to be true.*" - Jean Baudrillard
