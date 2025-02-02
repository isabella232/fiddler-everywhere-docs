---
title: Capturing and Inspecting Android Traffic
description: "Capture and inspect traffic from Android devices while using the Fiddler Everywhere web-debugging HTTP-proxy tool."
type: how-to
slug: capture-mobile-android-traffic
publish: true
position: 50
previous_url: /knowledge-base/configure-android, /get-started/mobile-traffic/configure-android, /get-started/traffic/configure-android
---

# Capturing and Inspecting Android Traffic

This article describes how to use Fiddler Everywhere to capture and inspect traffic that comes from Android devices and emulators.

To capture and inspect traffic on Android devices, perform the following steps:

1. [Provide the prerequisites](#prerequisites).
1. [Configure Fiddler Everywhere](#configuring-fiddler-everywhere).
1. [Configure the Android device](#configuring-the-android-device).
1. [Inspect the browser traffic](#inspecting-the-browser-traffic).
1. [Inspect the Android application traffic](#inspecting-the-android-application-traffic).

## Prerequisites

- Install [Fiddler Everywhere v. 1.0.0 and above](https://www.telerik.com/download/fiddler-everywhere).
- Connect an Android device to the same network or use an Android emulator on the Fiddler Everywhere host machine.
Ensure that the machine on which Fiddler Everywhere and the Android device run is discoverable on the same network.

## Configuring Fiddler Everywhere

1. Enable the remote connections in Fiddler Everywhere through **Settings** > **Connections** > **Allow remote computers to connect**.

1. Check the IP address of the host machine where Fiddler Everywhere is running. For demonstration purposes, let's assume that the local IP of the Fiddler Everywhere host machine is **192.168.148.39**. You can use [the connection status on the lower right-hand side]({slug connections-section}) to obtain the Fiddler Everywhere host IP address. Alternatively, you can use **ipconfig** on Windows or **ifconfig** on Linux.


## Configuring the Android Device

The following steps apply to real Android devices with access to the Internet through the same network as the Fiddler Everywhere host machine.

1. Set the Fiddler Everywhere proxy on the Android device or emulator.

    1. Open the connected Wi-Fi and tap **Settings**.

    1. Select **Edit** and expand **Advanced Settings**. For previous Android versions, you might have to touch and hold the name of the connected network, then tap **Modify**, and expand **Advanced Settings**.

    1. On **Proxy**, select **Manual proxy**.

     - Enter the IP address of the host computer on which Fiddler Everywhere runs&mdash;for example, **192.168.148.39**.
     - Enter the proxy port. Use the port configured in the Fiddler Everywhere application (configurable through **Settings > Connections > Fiddler listens on port...** ). The default port is **8866**.


1. Install the root certificate of Fiddler Everywhere on the Android device.

    1. Open a mobile browser on the Android device and type the http://ipv4.fiddler:8866 echo service address of Fiddler Everywhere.

    1. Tap the option to download the certificate. Then save and install as follows:

        - Save and install the certificate (**Android 10 and below**):

            - In the prompt window, enter a descriptive certificate name and tap **Save**.
            - Install the downloaded Fiddler certificate in the device certificate storage. The settings location depends on the Android version but is usually located under **Settings** > **Security** > **Encryption and Credentials** > **Install a certificate** > **CA Certificate**.

        - Save and install the certificate (**Android 11 and above**):

            - In the prompt windows, enter a descriptive certificate name and select **VPN and apps** as credentials use.

    1. Verify that the Fiddler Everywhere certificate is installed as a user certificate in the **Android OS** > **Settings** > **Security** > **Encryption & Credentials** > **User and Credentials** section.

With the above setup, you are ready to capture traffic from your Android mobile browser. Test your configuration as follows:

1. In Fiddler Everywhere, ensure that **Settings** > **Connections** > **Allow remote computers to connect** is checked and that **Live Traffic** capturing mode is turned ON.
1. On your emulator, open **Google Chrome** (or any other mobile browser that respect the proxy settings) and type [https://example.com](https://example.com)
1. Observe the secure traffic being captured back in Fiddler Everywhere.


## Configuring the Android Emulator

The Android Virtual Devices (a.k.a. AVDs or Android emulators) can use Fiddler Everywhere as a proxy by directly configuring the Android operating system (like an actual device). The crucial difference is that the Fiddler Everywhere proxy address will be the loopback address of the emulator. Check the emulator documentation for the IP address used as a loopback address. In most cases, the loopback alias of the Android emulator is of the **10.0.2.2**. Note that some third-party emulators are using different alias for the loopback address.

1. Start the emulator, open the simulated Wi-Fi, tap **Settings**, and expand **Advanced Settings**.

1. Select **Edit** and expand **Advanced Settings**. For some older Android versions, you might have to touch and hold the name of the connected network, then tap **Modify**, and expand **Advanced Settings**.

1. Open **Proxy**, and then select **Manual proxy**.

    - Enter the emulator loopback address as a proxy address. For the state Android emulators, the address is **10.0.2.2**.
    - Enter the proxy port. Use the port configured in the Fiddler Everywhere application (configurable through **Settings** > **Connections** > **Fiddler listens on port...** ). The default port is **8866**.

1. Install the root certificate of Fiddler Everywhere on the Android device.

    1. Open a mobile browser on the Android device and type the http://ipv4.fiddler:8866 echo service address of Fiddler Everywhere.

    1. Tap the option to download the certificate. Then save and install as follows:

        - (For Android 10 and earlier) Save and install the certificate:

            1. In the prompt window, enter a descriptive certificate name and tap **Save**.
            1. Install the downloaded Fiddler certificate in the device certificate storage. The settings location depends on the Android version but is usually located under **Settings** > **Security** > **Encryption and Credentials** > **Install a certificate** > **CA Certificate**.

        - (For Android 11 and above) Save and install the certificate: 
          
            1. In the prompt windows, enter a descriptive certificate name. 
            1. In the **Credentials use** field, select **VPN and apps**.

    1. Verify that the Fiddler Everywhere certificate is installed as a user certificate in the **Android OS** > **Settings** > **Security** > **Encryption & Credentials** > **User and Credentials** section.


With the above setup, you are ready to capture traffic from your Android mobile browser. Test your configuration as follows:

1. In Fiddler Everywhere, ensure that **Settings** > **Connections** > **Allow remote computers to connect** is checked and that the **Live Traffic** capturing mode is turned ON.
1. On your emulator, open **Google Chrome** (or any other mobile browser that respect the proxy settings) and type [https://example.com](https://example.com).
1. Back in Fiddler Everywhere, observe the secure traffic being captured.


## Inspecting the Browser Traffic

Now you can immediately monitor HTTP/HTTPS traffic from mobile browsers. For example, open a Chrome browser on your Android device, type an address of your choice, and observe the captured traffic in the **Live Traffic** section of Fiddler Everywhere. 

To differentiate the traffic that comes from the mobile device from the one that is being captured from the Fiddler Everywhere host machine, you can apply a **Client IP** column filter (for example, while using the mobile device IP) or a **Process** column filter (while using the device process name).

>important When you've finished debugging, remove the Wi-Fi proxy from your Android device to regain connectivity.

## Inspecting the Android Application Traffic

You can monitor traffic from applications in active development, which means that you have access to the codebase of that application.

1. (For Android API 24 and later) Add the following code to the `Android/src/main/res/xml/network_security_config.xml` file:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <network-security-config>
        <base-config>
            <trust-anchors>
            <!-- Trust preinstalled CAs -->
            <certificates src="system" />
            <!-- HERE: Additionaly trus user added CAs -->
            <certificates src="user"/>
        </trust-anchors>
        </base-config>
    </network-security-config>
    ```

1. In the `AndroidManifest.xml_` file, reference the `network-security-config` from the previous step through a parameter in the `application` tag:

    ```XML
    android:networkSecurityConfig="@xml/network_security_config"
    ```

    For example:

    ```XML
        <application
            android:name="com.tns.NativeScriptApplication"
            android:allowBackup="true"
            android:icon="@drawable/icon"
            android:networkSecurityConfig="@xml/network_security_config">
            ...
    ```

1. Rebuild the application. Now you can start monitoring its HTTP/HTTPS traffic.

## Additional Resources

* [Directing Localhost Requests from Mobile Application through the Fiddler Proxy]({%slug fiddler-localhost-android%})
* [Debugging Mobile Applications with Fiddler Everywhere (Webinar)](https://www.telerik.com/webinars/fiddler/how-to-debug-ios-and-android-mobile-apps-with-fiddler)
