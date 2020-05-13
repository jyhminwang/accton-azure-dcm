---
SLEplatform: Linux
device: SLE111GW Service Controller
language: c
---

Connect SLE111GW service controller to your Azure IoT Central Application
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

This document describes how to connect SLE111GW to Azure IoT Central application using the IoT plug and Play model. Plug and Play simplifies IoT by allowing solution developers to integrate devices without writing any device code. Using Plug and Play, device manufacturers will provide a model of their device to cloud developers to be integrated quickly into IoT Central or any solution built on the Azure IoT platform. IoT Plug and Play will be open to the community by way of a definition language and SDKs.

SLE111GW is a ZigBee HA IoT gateway. It manages Accton certified ZigBee devices. It routes commands and information between and ZigBee IoT devices to Azure IoT Cloud. Kinds of ZigBee HA profile devices are supported by this controller.

This document can also be found on [SLE111GW_GettingStarted.md](https://github.com/jyhminwang/accton-azure-dcm/blob/master/SLE111GW/README.md).

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
-   Connect WAN port to internet.
-   Connect LAN to you configuring PC.
-   Connect Power adapter and power on.
-   Onboard LCD will display current date and time after connecting to internet. 

<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/SLE111GW/png/GW_HW_Connection.png?raw=true">

**Software Environmental Setup**

1. Get your Gateway's connection details
-   You should already have your "Scope ID"(ID scope) and "Primary key"(master key) in [Device Connection Details](#DeviceConnectionDetails).
    Say them *"Scope_SSS"* and *"Master_PPP"* for example.
-   Determine your "Device ID"(registration ID).
    You can assign any string which is unique in your ID scope.
    Example here it is named *"SLE111GW_00:11:22:33:44:55"* where 00:11:22:33:44:55 is its ethernet MAC.
-   Use [dps-keygen](https://github.com/Azure/dps-keygen) to get this Gateway's corresponding "Symmetric key"

        dps-keygen -di:SLE111GW_00:11:22:33:44:55 -mk:Master_PPP
    Note down the responsed "device connection key"(Symmetric key).
    Say it *"Device_SSS"* for example.

2. Set your connection details in gateway

-   Launch a web browser on you PC which is connected to LAN port of this gateway.
-   Browse to 192.168.80.1, it is the default IP of SLE111GW gateway. You should see a web page as below
-   Click "Smart Home" => "Azure"
-   Input the "Device ID", "ID Scope", and "Symmetric Key" on the screen
-   Click "Apply"

<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/SLE111GW/png/SLE111GW_Web.png?raw=true">

<a name="IntegrationwithIoTCentral"></a>
# Integration with IoT Central

-   After what you have finished all hardware and software setup in [Prepare the Device](#preparethedevice),  you should find a new device is already provisioned in your Azure IoT Central App automatically.

<img src="https://github.com/jyhminwang/accton-azure-dcm/blob/master/SLE111GW/png/SLE111GW_AICAbout.png?raw=true">

<a name="AdditionalLinks"></a>
# Additional Links

Please refer to the below link for additional information for Plug and Play 

-    [Blog](https://azure.microsoft.com/en-us/blog/iot-plug-and-play-is-now-available-in-preview/)
-    [Plug and Play C SDK](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) 
-    [Plug and Play Node SDK](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview)
-    [Plug and Play Definitions](https://github.com/Azure/IoTPlugandPlay)







