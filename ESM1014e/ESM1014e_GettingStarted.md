﻿﻿﻿﻿﻿---
platform: FreeRTOS
device: ESM1014e Power Probe
language: c
---

Connect ESM1014e Power Probe to your Azure IoT Central Application
===

---
# Table of Contents

-   [Introduction](#Introduction)
-   [Prerequisites](#Prerequisites)
-   [Create Azure IoT Central application](#Create_AICA)
-   [Device Connection Details](#DeviceConnectionDetails)
-   [Prepare the Device](#preparethedevice)
-   [Integration with IoT Central](#IntegrationwithIoTCentral)
-   [Additional Links](#AdditionalLinks)


<a name="Introduction"></a>

# Introduction 

**About this document**

This document describes how to connect ESM1014e to Azure IoT Central application using the IoT plug and Play model.
Plug and Play simplifies IoT by allowing solution developers to integrate devices without writing any device code. 
Using Plug and Play, device manufacturers will provide a model of their device to cloud developers 
to be integrated quickly into IoT Central or any solution built on the Azure IoT platform. 
IoT Plug and Play will be open to the community by way of a definition language and SDKs.

ESM1014e is an electric power monitor with Ethernet connectivity. 
It offers the measurement capabilities required to monitor the power consumption of the interested main/branch circuit. 
(e.g. Lighting, Air conditioning or Power outlet circuits……etc.). 
ESM1014e can report energy in high data rate, so it can also used to monitor energy ripple.

<a name="Prerequisites"></a>
# Prerequisites

You should have the following items ready before beginning the process: 

-   [Azure Account](https://portal.azure.com)
-   [Azure IoT Central App](https://docs.microsoft.com/en-us/azure/iot-central/core/overview-iot-central)
-   [dps-keygen](https://github.com/Azure/dps-keygen)
-   Provide Network connectivity (LAN) supported by the device

**Note:** Azure IoT plug and play code is preinstalled in this device. Follow instrunctions in [Prepare the device](#preparethedevice) 
to setup device registration ID, ID scope, and the symmetric key.

<a name="Create_AICA"></a>
# Create Azure IoT Central application
You should have your [IoT Central App](https://apps.azureiotcentral.com/) that the plug and play device can connect to.


<a name="DeviceConnectionDetails"></a>
# Device Connection Details
Login to your Azure IoT Central App with a privileged account. Select the Administration tab , and then select "Device connection". Note down the "ID Scope".
Select "View Keys" somewhere below "ID scope", note down the "Primary key"(master key) as well.

<a name="preparethedevice"></a>
# Prepare the Device.

**Hardware Environmental setup**

As figure below:
-   Connect RJ-45 ethernet.
-   Connect Current Transformer to Probe's CT.L1+/CT.L1-. Please pay attention to the direction of current flow. It should be marked on the current transformer.
-   Connect Probe's Volt.Line1/Volt.Neutral to the L/N of the power cable respectively. The power cable supplies the load we plan to monitor.
-   You can reboot Power Probe by pressing button "B".
![PPBe basic connecion](ESM1014e/png/ESM1014e_basic.png)

**Software Environmental Setup**
1. Get your Probe's connection details
-   You should already have your "Scope ID"(ID scope) and "Primary key"(master key) in [Device Connection Details](#DeviceConnectionDetails).
    Say them *"Scope_SSS"* and *"Master_PPP"* for example.
-   Determine your "Device ID"(registration ID).
    You can assign any string which is unique in your ID scope.
    Example here it is named *"ESM1014e_00:11:22:33:44:55"* where 00:11:22:33:44:55 is its ethernet MAC.
-   Use [dps-keygen](https://github.com/Azure/dps-keygen) to get this Probe device's corresponding "Symmetric key"

        dps-keygen -di:ESM1014e_00:11:22:33:44:55 -mk:Master_PPP
    Note down the responsed "device connection key"(Symmetric key).
    Say it *"Device_SSS"* for example.

2. Set your connection details to the Probe
-   Launch any telnet client tool.
-   The default IP of ESM1014e is 192.168.77.113, you can reset Power Probe to factory default by pressing button "A" for 5 seconds.
-   $telnet 192.168.77.113
-   Inside Probe's telnet shell:

        list_if()
    You can find current network info and device's MAC address.

        set_if("e0", "192.168.77.114", "192.168.77.1", "255.255.255.0")
    You can set new IP, gateway, and netmask.
    Above example sets IP to 192.168.77.114

        list_aziotc()
    You can find current connection details about Azure IoT Central.

        set_aziotc("ESM1014e_00:11:22:33:44:55", "Scope_SSS", "Device_SSS")
    You can set connection details for Azure IoT Central.
    Here the example used device ID, ID scope, and the connection key we got earlier.

<a name="IntegrationwithIoTCentral"></a>
# Integration with IoT Central

-   After what you have done in [Prepare the Device](#preparethedevice), 
    you should find a new device is already provisioned in your Azure IoT Cnetral App automatically.
![Provisioned](ESM1014e/png/PPB_Provisioned.png)
-   An sample view as below
![View](ESM1014e/png/PPB_View.png)
-   Use this [Quickstart: Add a simulated device to your IoT Central application](https://docs.microsoft.com/en-us/azure/iot-central/core/quick-create-simulated-device) 
    doc as an example to try Power Probe in your Azure IoT Central App.

<a name="AdditionalLinks"></a>
# Additional Links

Please refer to the below link for additional information for Plug and Play 

-    [Blog](https://azure.microsoft.com/en-us/blog/iot-plug-and-play-is-now-available-in-preview/)
-    [Plug and Play C SDK](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) 
-    [Plug and Play Node SDK](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview)
-    [Plug and Play Definitions](https://github.com/Azure/IoTPlugandPlay)







