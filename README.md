# AppVirality-Android-SDK-2.0
Referrals &amp; Loyalty Program

<H3>Introduction:</H3>
AppVirality is a Plug and Play Growth Hacking Toolkit for Mobile Apps. 

It helps to identify and implement the right growth hacks, within seconds. No Coding Required. We are providing easy to integrate SDK's for Android, iOS and Windows(coming soon) platforms.

Appvirality Android SDK supports from Android (API level 8) and higher.

Version History 
---------------

Current Version : 1.2.1

[Version Info](https://github.com/farazAV/AppVirality-Android-SDK-2.0/wiki/Android-SDK-Version-History)

Integrating Appvirality into your App
-------------------------------------

<H3>Introduction:</H3>
AppVirality is a Plug and Play Growth Hacking Toolkit for Mobile Apps.

It helps app developers to identify and implement the right growth hack, within seconds & without any coding. We provide easy to integrate SDK's for various platforms that include Android & [iOS](https://github.com/appvirality/AppVirality-iOS-SDK).

AppVirality Android SDK supports from Android (API level 8) and higher.

Throughout the document, invitation sender will be called as "Referrer" and receiver will be called as "Friend".

<H4>STEP 1 - Getting Started:</H4>

This applies for Gradle based projects. Add the following dependency to your <b>build.gradle</b> file: 

```java
dependencies {

    compile 'com.appvirality:AppviralityUI:1.1.23+'

}
```

<H4>STEP 2 - Set up your AppVirality Keys</H4>

1) Once you've registered with AppVirality.com and add a new app, you will be given an App key.

![Alt text](https://github.com/appvirality/appvirality-sdk-android/blob/master/images/App-key-obtaining.jpg?raw=true)

2) Open the <b>AndroidManifest.xml</b> file of your project, add a <i>meta-data</i> element to the <i>application</i> element, with <i>name</i> as <b>com.appvirality.sdk.AppViralityApiKey</b> and <i>value</i> as your AppVirality App key:

```java
<application android:label="@string/app_name" ...>
    ...
    <meta-data
            android:name="com.appvirality.sdk.AppViralityApiKey"
            android:value="02e1r5e99b94f56t69f42a32a00d2e7ff" />
    ...
</application>
```

NOTE: Don't forget to replace the key "02e1r5e99b94f56t69f42a32a00d2e7ff" with your App Key.








