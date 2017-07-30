# GLES30_ProgrammingGuide_NDK
Clang Version of NDK Android GLES Tutorial

## How to

1.Download or clone this project,inside the folder,create Android Studio Project

2.Add `JNI Folder` In AS,Drag `Android.mk` `Application.mk`(in template) into JNI Folder,also add your Clang files into JNI Folder

3.Edit the `Android.mk` Files

```
LOCAL_PATH          := $(call my-dir)
                                //Locate Common Folder
SRC_PATH            := ../../.. 
COMMON_PATH         := $(SRC_PATH)/Common
COMMON_INC_PATH     := $(COMMON_PATH)/Include
COMMON_SRC_PATH     := $(COMMON_PATH)/Source

include $(CLEAR_VARS)
//Module Name
LOCAL_MODULE    := Hello_Triangle
LOCAL_CFLAGS    += -DANDROID


LOCAL_SRC_FILES := $(COMMON_SRC_PATH)/esShader.c \
                   $(COMMON_SRC_PATH)/esShapes.c \
                   $(COMMON_SRC_PATH)/esTransform.c \
                   $(COMMON_SRC_PATH)/esUtil.c \
                   $(COMMON_SRC_PATH)/Android/esUtil_Android.c \
                           //Your Clang Class 
                   $(LOCAL_PATH)/Hello_Triangle.c
                   
                   
                   

LOCAL_C_INCLUDES    := $(SRC_PATH) \
                       $(COMMON_INC_PATH)
                   
LOCAL_LDLIBS    := -llog -landroid -lEGL -lGLESv3

LOCAL_STATIC_LIBRARIES := android_native_app_glue

include $(BUILD_SHARED_LIBRARY)

$(call import-module,android/native_app_glue)
```

4.Edit the `Application.mk`

5.Edit the `AndroidManifest.xml`

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.martinrgb.chapter2_hellotriangle">
        <application
            android:label="HelloTriangle"
            android:hasCode="false">
            <activity android:name="android.app.NativeActivity"
                android:label="HelloTriangle"
                android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
                android:launchMode="singleTask"
                android:configChanges="orientation|keyboardHidden">
            
                //The value should be as same as LOCAL_MODULE in android.mk
                <meta-data android:name="android.app.lib_name"
                    android:value="Hello_Triangle" />
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
        </application>
        <uses-feature android:glEsVersion="0x00030000"/>
        <uses-sdk android:minSdkVersion="18"/>
</manifest>
```

6.open terminal,direct the JNI Folder, `ndk-build`

7.in `build.gradle`
```
//This snippets will hide the c&mk files
sourceSets.main {
    jniLibs.srcDir 'libs'
    jni.srcDirs = []
}
```

8.CMD + R,run your project
