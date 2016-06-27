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

Throughout the document, invitation sender will be called as "Referrer" and receiver will be called as "Friend".


<H4>STEP 1 - Adding dependency</H4>

This applies for Gradle based projects. Add the following dependency to your <b>build.gradle</b> file: 

```java
dependencies {

    compile 'com.appvirality:AppviralityUI:1.1.23+'

}
```


<H4>STEP 2 - Set up your AppVirality Keys</H4>

Once you've registered with AppVirality.com and add a new app, you will be given an App key.

![Alt text](https://github.com/appvirality/appvirality-sdk-android/blob/master/images/App-key-obtaining.jpg?raw=true)


<H4>STEP 3 - Configure the <b>AndroidManifest.xml</b> file of your project as follows</H4> 

1) Add the following <i>meta-data</i> elements to the <i>application</i> element

* Replace "02e1r5e99b94f56t69f42a32a00d2e7ff" with your AppVirality App key

```java
<application android:label="@string/app_name" ...>
    ...
    <meta-data
        android:name="com.appvirality.sdk.AppViralityApiKey"
        android:value="02e1r5e99b94f56t69f42a32a00d2e7ff" />
    ...
</application>
```

* Set <i>value</i> as <i>true</i> or <i>false</i>, depending on whether you want to run the app in <b>Test Mode</b> or <b>Live Mode</b> respectively. Set the value as <i>false</i> before publishing the app to the play store.

```java
<application android:label="@string/app_name" ...>
    ...
    <meta-data
        android:name="com.appvirality.sdk.TestMode"
        android:value="true" />
    ...
</application>
```

* Set <i>value</i> as <i>false</i> or <i>true</i>, depending on whether you want to use <b>Default Init Flow</b> or <b>Custom Init Flow</b> respectively

```java
<application android:label="@string/app_name" ...>
    ...
    <meta-data
        android:name="com.appvirality.sdk.CustomInit"
        android:value="false" />
    ...
</application>
```

2) Add the following permissions within the <code>&lt;manifest...&gt;</code>

```java
<manifest..>
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />  
    <!-- Optional permissions. WRITE_EXTERNAL_STORAGE is used to improve the performance by storing campaign images. -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

</manifest>
```

3) Add Install Referrer Receiver

AppVirality Uses Google Install Referrer for attribution as a fallback, in addition to device finger printing.

Add the following code block if you don't already have an <b>INSTALL_REFERRER</b> receiver in your manifest file <i>Application</i> Tag

```java
<receiver
    android:name="com.appvirality.AppViralityInstallReferrerReceiver"
    android:exported="true">
    <intent-filter>
        <action android:name="com.android.vending.INSTALL_REFERRER" />
    </intent-filter>
</receiver>
```
(or)

If you already have an <b>INSTALL_REFERRER</b> receiver, use following code block in the onReceive method of your broadcast receiver

```java
if (extras != null && extras.containsKey("referrer")) {
    	String referrer = intent.getStringExtra("referrer");
	AppVirality.setReferrerKey(context, referrer);
}
```
(or)

<b>NOTE:</b> If you have multiple <b>INSTALL_REFERRER</b> receivers in your App, please go through the documentation [here](https://github.com/farazAV/AppVirality-Android-SDK-2.0/wiki/Using-Multiple-Install-Referrer-Receivers).


<H4>STEP 4 - Initializing the AppVirality SDK</H4>

1) Create the <b>AppVirality</b> class singleton in the <b>onCreate</b> method of your app's launcher activity. It is very important to do this in the launcher activity so that the SDK will queue up all the API calls for retrying, which might have got failed in the past. Use the following code for the same

<code>
AppVirality appVirality = AppVirality.getInstance(SplashActivity.this);
</code>

2) Initializing the SDK

* Create a <b>UserDetails</b> class object and set the various user details to recognize the user same as your backend system. Also, it is required to personalize the referral messages and welcome screen, which will be shown to new users upon app installation. (Friends shall be able to see the referrer's name and profile picture). We will also pass these user details through web-hooks to notify you on successful referral or conversion(install,signup or transaction,etc.)

```java
UserDetails userDetails = new UserDetails();
userDetails.setReferralCode("ReferralCode");
userDetails.setAppUserId("UserId");
userDetails.setPushRegId("PushRegistrationId");
userDetails.setUserEmail("Email");
userDetails.setExtraInfo("ExtraInfo");
userDetails.setUserName("Name);
userDetails.setProfileImage("UserImage");
userDetails.setMobileNo("MobileNo");
userDetails.setCity("City");
userDetails.setState("State");
userDetails.setCountry("Country");
userDetails.setExistingUser(false);
```

- <b>setReferralCode</b> —  User's Referral Code.
- <b>setAppUserId</b> —  ID of the user in your App(helps to identify users on dashboard as you do in your app).
- <b>setPushRegId</b> —  Unique id assigned to the device by your Push Notification Service. Providing this helps AppVirality in sending Push Notifications to Users.
- <b>setUserEmail</b> —  User's email address.
- <b>setExtraInfo</b> —  Comma separated extra info. For example, various email addresses separated by comma.
- <b>setUserName</b> — First Name of the user, required to personalize the referral messages.
- <b>setProfileImage</b> —  User profile picture URL, required to personalize the referral messages.
- <b>setMobileNo</b> —  User's mobile number.
- <b>setCity</b> —  User's city.
- <b>setState</b> —  User's state.
- <b>setCountry</b> —  User's country.
- <b>setExistingUser</b> — Set this as True, if you identify the user as existing user(this is useful if you don't want to reward existing users).

* Invoke <i>init(<b>UserDetails</b> userDetails, <b>AppViralitySessionInitListener</b> callback)</i> method of the <b>AppVirality</b> class, passing the <i>UserDetail</i> object created in the previous step, to start the AppVirality's initialization calls 
