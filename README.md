Adinch App Install User Guide.
=====================


Introduction
-------------
This guide will help you to include the Adinch App Install SDK into your application.

SDK Integration
---------------

The following steps should be performed in Eclipse to include the Adinch App Install into your Android Application :

1.  Include the Adinch App Install SDK library.
--------------
In your application, create a folder ‘libs’ and drag the jar file adinch_appinstall_sdk.jar into the
libs folder.
    
2.  Add settings to your application manifest :
-------------------------------------------
    Open AndroidManifest.xml and add the following lines within the application tag.
    <receiver android:name="adinch.app.install.AdinchReceiver" android:exported="true">
          <intent-filter>
                      <action android:name="com.android.vending.INSTALL_REFERRER" />
          </intent-filter>
    </receiver>
    <meta-data android:name="APP_NAME" android:value="YOUR.APPLICATION.NAME" />
    <meta-data android:name="ADINCH_CLIENT_ID" android:value="ClientID" />

* Replace YOUR.APPLICATION.NAME with your application package name along the lines of com.yourcompany.yourapplication
* Replace ClientID with the client ID code given to you by Adinch App Install.

Add the following lines to AndroidManifest.xml after the </application> tag to allow permissions
for the Adinch App Install SDK
<!-- Container TAG requires Internet permission -->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
3. If you have any other referral Tracking :
-------------------------------------------

You should only have a single receiver for the INSTALL_REFERRER intent. If you are using any other referral tracking providers such as Admob or GoogleAnalytics, Adinch App Install provides a mechanism to forward the intent from the market on to other receivers. Simply add a line like the following one (The example given below is for Admob) within the receiver tags :

    <!—Forward the referral to Admob -->
    <meta-data android:name="forward.Admob"
    android:value="com.admob.android.ads.analytics.InstallReceiver" /> 
    
The name of the tag (in this case forward.Admob) can be anything so long as it is unique. So the code within the receiver tags would then look like :
    
    <receiver android:name="adinch.app.install.AdinchReceiver" android:exported="true">
          <intent-filter>
                      <action android:name="com.android.vending.INSTALL_REFERRER" />
          </intent-filter>
          <meta-data android:name="forward.Admob"
          android:value="com.admob.android.ads.analytics.InstallReceiver" /> 
    </receiver>
    
You can add as many receivers as you like and the Adinch App Install SDK will forward the referral broadcast on to each of them.

4. Call the Adinch App Install SDK.
----------------------------------

a) At the top of your main file include the Tag library :

    import adinch.app.install.AdinchConnect;
    
b) Inside your applications OnCreate function add the following lines :
// Connect with the Adinch App Install server.  Call this when the application
first starts.

    AdinchConnect.getConnectInstance(this);
