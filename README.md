# IIoT_Library

Contains examples showing how to use Industrial Internet of Things (IIoT) functions in PLCnext Engineer, including:

- IIoT_JSON_Parser function block.
- IIoT_JSON_Writer function block.
- IIoT_JSON_Coder function block.
- IIoT_JSON_Decoder function block.
- IIoT_MqttClient function block.
- IIoT_CerfificateInfo function block group.

## Example Overview

| Cloud Brand | Path           | Brief Introduction                                           |
| :---------- | :------------- | :----------------------------------------------------------- |
| Ali cloud   | Examples/Ali   | ProductKey, DeviceName and DeviceSecret should be correctly feed to IIoT_AliCertificateInfo FB, which can automatically convert these three element to elements that can be directly applied to IIoT_MqttClient FB |
| AWS         | Examples/AWS   | Connection is based on X509 certificates. The certs folder should be put to plcnext device (e.g /opt/plcnext/certs). and if you want to see the messages on cloud side, you should register your own device on AWS, and use the affliliated certificates and keys. |
| Azure       | Examples/Azure | the use of certs folder is just like AWS.                    |
| JSON        | Examples/Parser&Writer|Support the conversion between JSON format of structure object and IEC61131 format |

Examples are classified with Device type and Firmware versions. 
Default Firmware version of Demos 2021.0 are 2021.0.5
Default Firmware version of Demos 2022.0 and 2022.6 are 2022.6

Demos 2021.0 are created with PLCnext Engineer 2021.6
Demos 2022.0 are created with PLCnext Engineer 2022.0.4
Demos 2022.6 are created with PLCnext Engineer 2022.6

### Common setup tasks

1. Open the PLCnext Engineer project.

1. Check that the controller type and firmware version in the project matches the controller you are using. 

1. In the Project *Settings* window, set the fields in the **IP range** section to suit your network. In particular, if you want to use the default Mosquitto test broker, the *Default gateway* must be set to the IP address of your internet router.

1. In the controller *Settings* window, set the fields in the **Ethernet [Profinet]** section to suit your controller.

1. In the Project *Online Devices* window, check that the PLC is online and that the **Status** field is OK (indicated with a check mark).

1. Download the project to the PLC and check that the PLC starts OK.

### Program details: Publish

The MQTT Publish program implements a simple state sequencer that progresses through the following steps:

   * Idle - waiting for the start conditions.
   * Connecting - waiting for the client to connect to the server.
   * Running - publishing a simple message every 5 seconds, and waiting for the Run signal to disappear.

### Program details: Subscribe

The MQTT Subscribe program implements a simple state sequencer that progresses through the following steps:

   * Idle - waiting for the Run signal.
   * Connecting - waiting for the client to connect to the server.
   * Subscribing.
   * Running - receiving messages, and waiting for the Run signal to disappear.
   * Unsubscribing - waiting for the unsubscription to complete.

