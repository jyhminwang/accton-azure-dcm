---
platform: RTThread
device: ESM1014 PowerProbe
language: c
---

Connect ESM1014 PowerProbe to your Azure IoT Central Application
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

This document describes how to connect ESM1014 to Azure IoT Central application using the IoT plug and Play model. Plug and Play simplifies IoT by allowing solution developers to integrate devices without writing any device code. Using Plug and Play, device manufacturers will provide a model of their device to cloud developers to be integrated quickly into IoT Central or any solution built on the Azure IoT platform. IoT Plug and Play will be open to the community by way of a definition language and SDKs.

ESM1014 is a wireless (ZigBee) electric power monitor. It connects to Accton's IoT service controller (gateway). It offers the measurement capabilities required to monitor the power consumption of the interested main/branch circuit. (e.g. Lighting, Air conditioning or Power outlet circuits...etc.).

This document can also be found on [ESM1014_GettingStarted.md](https://github.com/jyhminwang/accton-azure-dcm/blob/master/ESM1014/README.md).

<a name="Prerequisites"></a>
# Prerequisites

You should have the following items ready before beginning the process: 

-   [Azure Account](https://portal.azure.com)
-   [Azure IoT Central App](https://docs.microsoft.com/en-us/azure/iot-central/core/overview-iot-central)
-   [dps-keygen](https://github.com/Azure/dps-keygen)
-   [A SLE111GW ZigBee gateway](https://github.com/jyhminwang/accton-azure-dcm/blob/master/SLE111GW/README.md)
-   Accton ESM1014 Power Probe
-   Provide Network connectivity supported by the SLE111GW gateway

**Note:** Azure IoT plug and play code is preinstalled in this device. Follow instructions in [Prepare the device](#preparethedevice) to setup device registration ID, ID scope, and the symmetric key.

<a name="Create_AICA"></a>
# Create Azure IoT Central application
You should have your [IoT Central App](https://apps.azureiotcentral.com/) that the plug and play device can connect to.

<a name="DeviceConnectionDetails"></a>
# Device Connection Details
Login to your Azure IoT Central App with a privileged account. Select the Administration tab , and then select "Device connection". Note down the "ID Scope".

<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/SLE111GW/png/AICA_DC_IDScope.png?raw=true">

Click "View Keys" somewhere below "ID scope", note down the "Primary key"(master key) as well.

<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/SLE111GW/png/AICA_DC_Keys.png?raw=true">

<a name="preparethedevice"></a>
# Prepare the Device.

**Hardware Environmental setup**

As figure below:
1. Prepare SLE111GW gateway
-   Connect ethernet WAN to internet, ethernet LAN to PC, and the power adapter.
-   You can also refer to [SLE111GW Getting Started](https://github.com/jyhminwang/accton-azure-dcm/blob/master/SLE111GW/README.md) for more.

2. Prepare ESM1014 Power Probe
-   Connect Current Transformer to Probe's CT.L1+/CT.L1-. Please pay attention to the direction of current flow. It should be marked on the current transformer.
-   Connect Probe's Volt.Line1/Volt.Neutral to the L/N of the power cable respectively. The power cable supplies the load we plan to monitor.

3. Pairing
-	Pressing button "T" on SLE111GW gateway.
-	Pressing button "A" on ESM1014 Power Probe for 5 seconds.
-	The status LED on ESM1014 Power Probe will be green after paired successfully.

<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/ESM1014/png/ESM1014_basic.png?raw=true">

**Software Environmental Setup**

1. Get your Probe's connection details
-   You should already have your "Scope ID"(ID scope) and "Primary key"(master key) in [Device Connection Details](#DeviceConnectionDetails).
    Say them *"Scope_SSS"* and *"Master_PPP"* for example.
-   Determine your "Device ID"(registration ID).
    You can assign any string which is unique in your ID scope.
    Example here it is named *"ESM1014_00:11:22:33:44:55:66:77"* where 00:11:22:33:44:55:66:77 is its ZigBee MAC.
-   Use [dps-keygen](https://github.com/Azure/dps-keygen) to get this Probe device's corresponding "Symmetric key".

        dps-keygen -di:ESM1014_00:11:22:33:44:55:66:77 -mk:Master_PPP
    Note down the responsed "device connection key"(Symmetric key).
    Say it *"Device_SSS"* for example.

2. Set your ESM1014 Power Probe's connection details on SLE111GW gateway Web user interface
-   Launch a web browser on you PC which is connected to LAN port of this gateway.
-   Browse to 192.168.80.1, it is the default IP of SLE111GW gateway. You should see a web page as below.
-   Click "Smart Home" => "Configure PowerProbe".
-   Find and click the corresping Power Probe in the list.
-   Input the "Device ID", "ID Scope", and "Symmetric Key" on the Azure IoT Central area.
-   Click "Apply".

<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/SLE111GW/png/SLE111GW_Web.png?raw=true">

<a name="IntegrationwithIoTCentral"></a>
# Integration with IoT Central

-   Download the device template file, [Accton ESM1014 PowerProbe.json](https://github.com/jyhminwang/accton-azure-dcm/blob/master/ESM1014/Accton%20ESM1014%20PowerProbe.json)

-   Create a new custom device template, choose "IoT Device".
<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/SLE111GW/png/AICA_DT_Import1_IoTDev.png?raw=true">

-   DO NOT check "Gateway device".
<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/SLE111GW/png/AICA_DT_Import2_NoGWDev.png?raw=true">

-   Give the device template a proper name, for below example, "Accton ESM1014".
<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/ESM1014/png/AICA_DT_PPB_Import3_Name.png?raw=true">

-   Import device capability model  (i.e. the saved template file),  [Accton ESM1014 PowerProbe.json](https://github.com/jyhminwang/accton-azure-dcm/blob/master/ESM1014/Accton%20ESM1014%20PowerProbe.json). And then "Publish".
<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/ESM1014/png/AICA_DT_PPB_Import4_CM.png?raw=true">

-   Since you have finished all hardware and software setup in [Prepare the Device](#preparethedevice), restart your device.  You should find this device is provisioned in your Azure IoT Central App automatically.
<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/ESM1014/png/AICA_DT_PPB_Provisioned.png?raw=true">

-   Use this [Quickstart: Add a simulated device to your IoT Central application](https://docs.microsoft.com/en-us/azure/iot-central/core/quick-create-simulated-device) doc as an example to try Power Probe in your Azure IoT Central App.

<a name="AdditionalLinks"></a>

# Additional Links

Please refer to the below link for additional information for Plug and Play 

-    [Blog](https://azure.microsoft.com/en-us/blog/iot-plug-and-play-is-now-available-in-preview/)
-    [Plug and Play C SDK](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) 
-    [Plug and Play Node SDK](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview)
-    [Plug and Play Definitions](https://github.com/Azure/IoTPlugandPlay)







