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

a) <b>setReferralCode</b> —  User's Referral Code  
b) <b>setAppUserId</b> —  ID of the user in your App(helps to identify users on dashboard as you do in your app)  
c) <b>setPushRegId</b> —  Unique id assigned to the device by your Push Notification Service. Providing this helps AppVirality in sending Push Notifications to Users  
d) <b>setUserEmail</b> —  User's email address  
e) <b>setExtraInfo</b> —  Comma separated extra info. For example, various email addresses separated by comma  
f) <b>setUserName</b> — First Name of the user, required to personalize the referral messages  
g) <b>setProfileImage</b> —  User profile picture URL, required to personalize the referral messages  
h) <b>setMobileNo</b> —  User's mobile number  
i) <b>setCity</b> —  User's city  
j) <b>setState</b> —  User's state  
k) <b>setCountry</b> —  User's country  
l) <b>setExistingUser</b> — Set this as True, if you identify the user as existing user(this is useful if you don't want to reward existing users)

* Invoke <b>init(UserDetails userDetails, AppViralitySessionInitListener callback)</b> method of the <b>AppVirality</b> class to start the AppVirality's initialization API calls, passing the <i>UserDetail</i> object created in the previous step and an <i>AppViralitySessionInitListener</i> instance. Use this method preferably in your splash activity or main activity, so that your campaigns will be ready in the background by the time your app gets loaded. This ensures smooth user experience. Use the following code to initialize the sdk

```java
appVirality.init(userDetails, new AppVirality.AppViralitySessionInitListener() {
        @Override
        public void onInitFinished(boolean isInitialized, JSONObject responseData, String errorMessage) {
                Log.i("AppVirality: ", "Is Initialized " + isInitialized);
                if (responseData != null)
                        Log.i("AppVirality: ", "userDetails " + responseData.toString());
	}
});
```  

<H4>STEP 5 - Launching Growth Hack</H4>

In-App referral growth hack can be launched in 3 different ways. You can use any/all of these 3 options to launch the growth hack.

#### Option 1 - Launch from custom button i.e from "Invite Friends" or "Refer & Earn" button on your App menu

###### Show Custom Button(i.e. "Refer & Earn" button) only on Campaign availability

You can use the following method if you want to show some label or message bar, only if there is any campaign available for the user.<i>CampaignDetailsReadyListener</i> will get called irrespective of campaign availability but if campaign is not available the <i>onCampaignReady</i> method shall receive empty campaign list. This is mainly useful when you want to have some control over the "Invite" or "Share" button visibility.

Use below code block to get the campaign details configured on AppVirality dashboard.

```java
appVirality.getCampaigns(Constants.GrowthHackType.All, new AppVirality.CampaignDetailsReadyListener() {
        @Override
        public void onCampaignDetailsReady(ArrayList<CampaignDetail> campaignDetails, boolean refreshImages, String errorMsg) {
        	// Get Word of Mouth campaign details from list of campaign details
                CampaignDetail womCampaignDetail = getCampaignDetail(Constants.GrowthHackType.Word_of_Mouth, campaignDetails);
                if (refreshImages)
                	refreshImages(womCampaignDetail);
                if (campaignDetails.size() > 0 && womCampaignDetail != null) {
                	// Campaigns available, display Refer & Earn button or launch growth hack screen
                } else {
                        // Campaigns not available, hide Refer & Earn button or display some message to the user
                }
        }
});
```

<b>NOTE:</b> You must check for <i>refreshImages</i> value and download the images for the campaign if its true, whenever you use the <i>CampaignDetailsReadyListener</i> callback because this value will be provided only once whenever campaign data will change. So in order to have latest campaign images you must check <i>refreshImages</i> value each time you use this callback.

Tip: Let the App users know about referral program by showing mini notification or some banner to achieve great results.

#### Option 2 - Launch from Popup

You can launch the growth hack from popup dialog. You can configure the popup dialog message and style from AppVirality dashboard and you need not update your app every time you make the modifications.

You can control the visibility of this mini notification from dashboard.(i.e. By setting launch conditions like after how many app launches you want to show the notification or after how many days of first install you want to show the notification).

Use the below code to create a popup for launching growth hack

```java
appVirality.getCampaigns(Constants.GrowthHackType.All, new AppVirality.CampaignDetailsReadyListener() {
        @Override
        public void onCampaignDetailsReady(ArrayList<CampaignDetail> campaignDetails, boolean refreshImages, String errorMsg) {
        	if (refreshImages)
                    utils.refreshImages(utils.getCampaignDetail(Constants.GrowthHackType.Word_of_Mouth, campaignDetails));
                CampaignDetail womCampaignDetail = utils.getCampaignDetail(Constants.GrowthHackType.Word_of_Mouth, campaignDetails);
                if (womCampaignDetail != null) {
                    // Checking Popup visibility conditions as set by you on the AppVirality dashboard
                    if (appVirality.showCustomPopUp(womCampaignDetail))
                        customPopUp.showLaunchPopUp(campaignDetails, womCampaignDetail, isMiniNotification);
                }
        }
});
```

#### Option 3 - Launch from Mini Notification

You can launch the GrowthHack from Mini notification. You can configure the Mini notification style and message from your AppVirality dashboard. You can control the visibility of this mini notification from dashboard, same as for a Popup.

Use the below code to create a mini notification for launching growth hack

```java
appVirality.getCampaigns(Constants.GrowthHackType.All, new AppVirality.CampaignDetailsReadyListener() {
        @Override
        public void onCampaignDetailsReady(ArrayList<CampaignDetail> campaignDetails, boolean refreshImages, String errorMsg) {
        	if (refreshImages)
                    utils.refreshImages(utils.getCampaignDetail(Constants.GrowthHackType.Word_of_Mouth, campaignDetails));
                CampaignDetail womCampaignDetail = utils.getCampaignDetail(Constants.GrowthHackType.Word_of_Mouth, campaignDetails);
                if (womCampaignDetail != null) {
                    // Checking Mini Notification visibility conditions as set by you on the AppVirality dashboard
                    if (appVirality.showCustomPopUp(womCampaignDetail))
                        customPopUp.showLaunchPopUp(campaignDetails, womCampaignDetail, isMiniNotification);
                }
        }
});
```

<H4>STEP 6 - Register Events</H4>

Registering Events are very important to reward your participants (Referrer/Friend) in case of a successful event. Also to calculate the LTV of participant (Referrer/Friend)

Tip: Identify top influencer's and make most of their network.

<H5>Install Event: </H5>

Please add the following code to send a <b>Install</b> conversion event when a user installs the app.

```java
saveConversionEvent("Install", null, null, campaignId, Constants.GrowthHackType.Word_of_Mouth, new AppVirality.ConversionEventListener() {
        @Override
        public void onResponse(boolean isSuccess, String message, String errorMsg) {
        	Toast.makeText(getApplicationContext(), message, Toast.LENGTH_LONG).show();
        }
});
```

