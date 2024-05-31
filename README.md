Native OpenCV for Android with Android NDK
A tutorial for setting up OpenCV 4.6.0 (and other 4.x.y versions) for Android in Android Studio with Native Development Kit (NDK) support. Android NDK enables you to implement your OpenCV image processing pipeline in C++ and call that C++ code from Android Kotlin/Java code through JNI (Java Native Interface).

This sample Android application displays a live camera feed with an OpenCV adaptive threshold filter applied on each frame. The OpenCV adaptive threshold call is performed in C++.

Setup
Tool	Version
OpenCV	4.6.0
Android Studio	2021.2.1
Android Build Tool	33.0
Android NDK	25.0
Kotlin	1.6.10
Gradle	7.2.1
Mac OS	12.4
How to use this repository
Download and Install Android Studio

Install NDK and CMake

Clone this repository as an Android Studio project :

In Android Studio, click on File -> New -> Project from Version Control -> Git
Paste this repository Github URL, choose a project directory and click next.
Install OpenCV Android release :

Download OpenCV 4.6.0 Android release or download latest available Android release on OpenCV website.
Unzip downloaded file and put OpenCV-android-sdk directory on a path of your choice.
Link your Android Studio project to the OpenCV Android SDK you just downloaded :

Open gradle.properties file and edit following line with your own OpenCV Android SDK directory path :

opencvsdk=/Users/Example/Downloads/OpenCV-android-sdk
Sync Gradle and run the application on your Android Device!

Note: MainActivity is written in Kotlin but you can comment out the Kotlin file and uncomment the Java file to use Java.

Bootstrap a new Android project with Native OpenCV support
Here are the steps to follow to create a new Android Studio project with native OpenCV support :

Download and Install Android Studio

Install NDK and CMake

Create a new Native Android Studio project :

Select File -> New -> New Project... from the main menu.
Click Phone and Tablet tab, select Native C++ and click next.
Choose an Application Name, select your favorite language (Kotlin or Java), choose Minimum API level (28 here) and select next.
Choose Toolchain default as C++ standard and click Finish.
Install OpenCV Android release :

Download OpenCV 4.6.0 Android release or download latest available Android release on OpenCV website.
Unzip downloaded file and put OpenCV-android-sdk directory on a path of your choice.
Add OpenCV Android SDK as a module into your project :

Open setting.gradle file and append these two lines.

include ':opencv'
project(':opencv').projectDir = new File(opencvsdk + '/sdk')
Open gradle.properties file and append following line. Do not forget to use correct OpenCV Android SDK path for your machine.

opencvsdk=/Users/Example/Downloads/OpenCV-android-sdk
Open build.gradle file and add implementation project(path: ':opencv') to dependencies section :

dependencies {
    ...
    implementation project(path: ':opencv')
}
Click on File -> Sync Project with Gradle Files.

Add following config to app build.gradle file :

In android -> defaultConfig -> externalNativeBuild -> cmake section, put these three lines :

cppFlags "-frtti -fexceptions"
abiFilters 'x86', 'x86_64', 'armeabi-v7a', 'arm64-v8a'
arguments "-DOpenCV_DIR=" + opencvsdk + "/sdk/native"
Add following config to CMakeLists.txt file :

Before add_library instruction, add three following lines :

include_directories(${OpenCV_DIR}/jni/include)
add_library( lib_opencv SHARED IMPORTED )
set_target_properties(lib_opencv PROPERTIES IMPORTED_LOCATION ${OpenCV_DIR}/libs/${ANDROID_ABI}/libopencv_java4.so)
In target_link_libraries instruction arguments, add following line :

lib_opencv
Add following permissions to your AndroidManifest.xml file :

<uses-permission android:name="android.permission.CAMERA"/>
<uses-feature android:name="android.hardware.camera"/>
<uses-feature android:name="android.hardware.camera.autofocus"/>
<uses-feature android:name="android.hardware.camera.front"/>
<uses-feature android:name="android.hardware.camera.front.autofocus"/>
Create your MainActivity :

You can copy paste MainActivity Kotlin or Java file. Do not forget to adapt package name.
Create your activity_main.xml :

You can copy paste activity_main.xml file.
Add native code in native-lib.cpp :

You can copy paste native-lib.cpp file. Do not forget to adapt the method name : Java_com_example_nativeopencvtemplate_MainActivity_adaptiveThresholdFromJNI should be replaced with Java_<main-activity-package-name-with-underscores>_MainActivity_adaptiveThresholdFromJNI.
Sync Gradle and run the application on your Android Device!

alt text

Questions and Remarks
If you have any question or remark regarding this tutorial, feel free to open an issue.

Acknowledgments
This tutorial was inspired by this very good Github repository.

Keywords
Tutorial, Template, OpenCV 4, Android, Android Studio, Native, NDK, Native Development Kit, JNI, Java Native Interface, C++, Kotlin, Java "# Car-Detection-Mobile-App"
