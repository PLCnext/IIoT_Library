# IIOT_Library

Contains examples showing how to use Industrial Internet of Things (IIoT) functions in PLCnext Engineer, including:

- JSON_Coder function block.
- JSON_Decoder function block.
- IIOT_MqttClient function block.

At present the function blocks used in these examples are contained in the following libraries in the PLCnext Store:

- JSON_utility_library
- MQTT_Client_Library

In the future, these will be combined into a single IIOT_Library in the PLCnext Store.

## Example

The following PLCnext Engineer project is available in the `Examples` directory:

   * JSON_utility_MQTT_client_EXA.pcweax

For this example, the following is required:

   * Any PLCnext Control device with firmware version 2021.0 or later.
   * PLCnext Engineer version 2021.0 or later.
   * JSON_utility_library_1 and MQTT_Client_Library_2.
   * Access to an MQTT server from the PLC, e.g. test.mosquitto.org.

### Common setup tasks

1. Open the PLCnext Engineer project.

1. Check that the controller model and firmware version in the project matches the controller you are using. If necessary, [replace the PLC in the project](https://www.plcnext.help/te/About/#replacing_a_plc) with the type of PLC that you are using.

1. In the Project *Settings* window, set the fields in the **IP range** section to suit your network. In particular, if you want to use the default Mosquitto test broker, the *Default gateway* must be set to the IP address of your internet router.

1. In the controller *Settings* window, set the fields in the **Ethernet [Profinet]** section to suit your controller.

1. In the Project *Online Devices* window, check that the PLC is online and that the **Status** field is OK (indicated with a check mark).

1. Download the project to the PLC and check that the PLC starts OK.

### MQTT setup tasks

Before using the MQTT programs `Publish` and/or `Subscribe`, check the values of the following variables, and change them if required:

   | Name           | Description                                                                                                 | Default value                               |
   |:---------------|:------------------------------------------------------------------------------------------------------------|:--------------------------------------------|
   | SERVER_ADDRESS | The address of the MQTT broker.<br/>Must be accessible from the PLC.<br/>(check using `ping` if necessary). | tcp://5.196.95.208:1883               |
   | CLIENT_ID      | The ID of the MQTT client.<br/>Must be unique on the MQTT broker.                                           | PLCnext Subscriber or<br/>PLCnext Publisher |
   | TOPIC          | The topic to publish/subscribe to on the broker.                                                            | plcnext                                     |

### Quick start

1. In the program instance called "JSON_Code_Decode", set the variable `udtCoderExample.udtCoder.x_EN` to `TRUE`. This creates a JSON stream as a byte array. This is passed to the "MQTT_Publish" program instance via Port variables.

1. In the program instance called "MQTT_Publish", set the variable `Run` to TRUE. This starts publishing the JSON stream to the Mosquitto test broker. Successful publishing can be checked by subscribing to the same topic from any machine with MQTT client software installed (e.g. the `mosquitto_sub` command-line tool on Linux).

1. In the program instance called "MQTT_Subscribe", set the variable `Run` to TRUE. This subscribes to the topic on the Mosquitto test broker where the JSON messages are being published.

1. Each time a new message is received from the server and consumed by the client, the JSON Decode process is triggered via Port variables.

1. In "JSON_Code_Decode", check the value of the structs udtJsonValue_Dot_INT and udtJsonValue_Dot_DINT. The values of the INT and DINT variables from the incoming JSON stream should appear in these structs.

### Program details: MQTT Publish

The MQTT Publish program implements a simple state sequencer that progresses through the following steps:

   * Idle - waiting for the start conditions.
   * Connecting - waiting for the client to connect to the server.
   * Running - publishing a simple message every 5 seconds, and waiting for the Run signal to disappear.

### Program details: MQTT Subscribe

The MQTT Subscribe program implements a simple state sequencer that progresses through the following steps:

   * Idle - waiting for the Run signal.
   * Connecting - waiting for the client to connect to the server.
   * Subscribing.
   * Running - receiving messages, and waiting for the Run signal to disappear.
   * Unsubscribing - waiting for the unsubscription to complete.

### Using SSL

The example can be adjusted to use an SSL connection using the following steps:

1. Download the server certificate for the Mosquitto test broker (in PEM format) from [test.mosquitto.org](https://test.mosquitto.org/).

1. Copy the server certificate to the PLC. Take note of the full path to the server certificate, e.g. `/opt/plcnext/mosquitto.org.crt`

1. Change the value of the SERVER_ADDRESS variable to the following: `ssl://5.196.95.208:8883`

1. In the code, before calling the Connect method, set the following values:
   * connOpts.xUseSslOptions := TRUE
   * connOpts.udtSsl.strTrustStore := `/opt/plcnext/mosquitto.org.crt` (* the full path to the server certificate on the PLC *)
   * connOpts.udtSsl.xEnableServerCertAuth := TRUE

1. Write and start the project.

The client will now use SSL to connect and communicate with the MQTT broker.
