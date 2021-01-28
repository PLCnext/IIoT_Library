# IIOT_Library

Contains examples showing how to use Industrial Internet of Things (IIoT) functions in PLCnext Engineer, including:

- JSON_ function block.
- IIOT_MqttClient function block.

At present the function blocks used in these examples are contained in the following libraries in the PLCnext Store:

- JSON_Library_1
- MQTT_Client_Library_2

In the future, these will be combined into a single IIOT_Library in the PLCnext Store.

## Examples

For examples showing how to use the MQTT Client, the following PLCnext Engineer project is installed with this library:

   * MQTT_1_EXA_MQTT_Client.pcwex

For these examples, the following hardware is used:

   * !AXC_F_1152

### Common setup tasks

To use the examples in the project, open the project in PLCnext Engineer and complete the following tasks:

1. In the Project *Settings* window, set the fields in the **IP range** section to suit your network. 
In particular, if you want to use the default Mosquitto test broker, the *Default gateway* must be set to the IP address of your internet router .

1. Check that the controller model and firware version in the project matches the controller you are using.

1. In the controller *Settings* window, set the fields in the **Ethernet [Profinet]** section to suit your controller.

1. In the Project *Online Devices* window, check that the PLC is online and that the **Status** field is OK (indicated with a check mark).

1. Add the MQTT Client library to the project using the "Add User Library..." context menu in the **Libraries** area of the Component view.

1. Download the project to the PLC and check that the PLC starts OK.

You are now ready to try the example programs.

Before using any of the example programs, check the values of the following variables, and change them if required:

   | Name           | Description                                                                                                 | Default value                               |
   |:---------------|:------------------------------------------------------------------------------------------------------------|:--------------------------------------------|
   | SERVER_ADDRESS | The address of the MQTT broker.<br/>Must be accessible from the PLC.<br/>(check using `ping` if necessary). | tcp://5.196.95.208:1883               |
   | CLIENT_ID      | The ID of the MQTT client.<br/>Must be unique on the MQTT broker.                                           | PLCnext Subscriber or<br/>PLCnext Publisher |
   | TOPIC          | The topic to publish/subscribe to on the broker.                                                            | plcnext                                     |

Note that every program in this example project contains two code sheets, one written in Ladder and the other in Structured Text. 
These two code sheets implement exactly the same functions, but only one will be executed at a time. 
You can control which sheet is executed using the `Language` variable in the respective program.

### Publish

This is an example of how to send messages as an MQTT publisher. The example demonstrates:

   * Connecting to an MQTT server/broker
   * Publishing messages
   * Automatic reconnects
   * Off-line buffering
   * Default file-based persistence

To use the Publish example, open the program named "Publish", and complete the following tasks:

1. Check the values of the SERVER_ADDRESS, CLIENT_ID and TOPIC variables, and change them if required.
1. In the PLCnext *Tasks and Events* window, create an instance of the Publish program in the *Cycle100* task.
1. Write and Start the project. PLCnext Engineer will go online to the PLC and enter Debug mode.
1. Add the `Run` variable to the Watch window.
1. Set the value of `Run` to TRUE.
1. Using an MQTT subscriber, check that the test messages are being published.
1. Set the value of `Run` to FALSE.

The program implements a simple state sequencer that progresses through the following steps:

   * Idle - waiting for the Run signal.
   * Connecting - waiting for the client to connect to the server.
   * Running - publishing a simple message every 5 seconds, and waiting for the Run signal to disappear.
   * Disconnecting - waiting for the client to disconnect from the server.

This example is based on the [async_publish][async_publish] sample in the Paho MQTT C++ library.

### Subscribe

This is an example of how to receive messages as an MQTT subscriber.

The example demonstrates:

   * Connecting to an MQTT server/broker
   * Subscribing to a topic
   * Receiving messages
   * Attempting manual reconnects.
   * Using a "clean session" and manually re-subscribing to topics on reconnect.

To use the Subscribe example, open the program named "Subscribe", and complete the following tasks:

1. Check the values of the SERVER_ADDRESS, CLIENT_ID and TOPIC variables, and change them if required.
1. In the PLCnext *Tasks and Events* window, create an instance of the Subscribe program in the *Cycle100* task.
1. Write and Start the project. PLCnext Engineer will go online to the PLC and enter Debug mode.
1. Add the `Run` variable to the Watch window.
1. Set the value of `Run` to TRUE.
1. Using an MQTT publisher, check that messages are being received on the subscribed topic.
1. Set the value of `Run` to FALSE.

The program implements a simple state sequencer that progresses through the following steps:

   * Idle - waiting for the Run signal.
   * Connecting - waiting for the client to connect to the server.
   * Subscribing - waiting for the subscription to complete.
   * Running - receiving messages, and waiting for the Run signal to disappear.
   * Unsubscribing - waiting for the unsubscription to complete.
   * Disconnecting - waiting for the client to disconnect from the server.

This example is based on the [async_subscribe][async_subscribe] sample in the Paho MQTT C++ library.

### Using SSL

The previous examples can be adjusted to use an SSL connection using the following steps:

1. Download the server certificate for the Mosquitto test broker (in PEM format) from [test.mosquitto.org](https://test.mosquitto.org/).

1. Copy the server certificate to the PLC. Take note of the full path to the server certificate, e.g. `/opt/plcnext/mosquitto.org.crt`

1. Change the value of the SERVER_ADDRESS variable to the following: `ssl://5.196.95.208:8883`

1. In the code, before calling the Connect method, set the following values:
   * connOpts.xUseSsl := TRUE
   * connOpts.udtSsl.strTrustStore := `/opt/plcnext/mosquitto.org.crt` (* the full path to the server certificate on the PLC *)

1. Write and start the project.

The client will now use SSL to connect and communicate with the MQTT broker.
