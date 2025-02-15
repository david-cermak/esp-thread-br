# ESP Thread Boarder Router SDK

ESP-THREAD-BR is the official [ESP Thread Border Router](https://openthread.io/guides/border-router/espressif-esp32) SDK. It supports all fundamental network features to build a Thread Border Router and integrates rich product level features for quick productization.

# Software Components

![esp_br_solution](docs/images/esp-thread-border-router-solution.png)

The SDK is built on top of [ESP-IDF](https://github.com/espressif/esp-idf) and [OpenThread](https://github.com/openthread/openthread). The OpenThread port and ESP Border Router implementation is provided as pre-built library in ESP-IDF.

# Hardware Platforms

## Wi-Fi based Thread Border Router

The Wi-Fi based ESP Thread Border Router consists of two SoCs:

* An ESP32 series Wi-Fi SoC (ESP32, ESP32-C, ESP32-S, etc) loaded with ESP Thread Border Router and OpenThread Stack.
* An ESP32-H 802.15.4 SoC loaded with OpenThread RCP.

### ESP Thread Border Router Board

The ESP Thread border router board provides an integrated module of an ESP32-S3 SoC and an ESP32-H2 RCP.

![br_dev_kit](docs/images/esp-thread-border-router-board.png)

The two SoCs are connected with following interfaces:
* UART and SPI for serial communication
* RESET and BOOT pins for RCP Update
* 3-Wires PTA for RF coexistence

### Standalone Modules

The SDK also supports manually connecting an ESP32-H2 RCP to an ESP32 series Wi-Fi SoC.

ESP32 pin | ESP32-H2 pin
----------|-------------
  GND     |      G
  GPIO17  |      TX
  GPIO18  |      RX
  GPIO4   |      RST
  GPIO5   |  GPIO9 (BOOT)

The following image shows an example connection between ESP32 DevKitC and ESP32-H2 DevKitC:

![br_standalone](docs/images/thread-border-router-esp32-esp32h2.jpg)

In this setup, only UART interface is connected, so it doesn't support RCP Update or RF Coexistence features. You can refer to [ot_br](https://github.com/espressif/esp-idf/tree/master/examples/openthread/ot_br) example in esp-idf as a quick start.

## Ethernet based Thread Border Router

Similar to the previous Wi-Fi based Thread Border Route setup, but a device with Ethernet interface is required, such as [ESP32-Ethernet-Kit](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/hw-reference/esp32/get-started-ethernet-kit.html).

# Provided Features

These features are currently provided by the SDK:

* **Bi-directional IPv6 Connectivity**: The devices on the backbone link (typically Wi-Fi) and the Thread network can reach each other.
* **Service Discovery Delegate**: The nodes on the Thread network can find the mDNS services on the backbone link.
* **Service Registration Server**: The nodes on the Thread network can register services to the border router for devices on the backbone link to discover.
* **Multicast Forwarding**: The devices joining the same multicast group on the backbone link and the Thread network can be reached with one single multicast.
* **NAT64**: The devices can access the IPv4 Internet via the border router.
* **RCP Update**: The built border router image will contain an updatable RCP image and can automatically update the RCP on version mismatch or RCP failure.
* **Web GUI**: The border router will enable a web server and provide some practical functions including Thread network discovery, network formation and status query. 

In the future releases, the following features will be added:

* SPI Communication
* RF Coexistence

# Resources

* Documentation for the latest version: https://docs.espressif.com/projects/esp-thread-br/. This documentation is built from the [docs directory](docs) of this repository.

* The [esp32.com forum](https://esp32.com/) is a place to ask questions and find community resources.

* [Check the Issues section on github](https://github.com/espressif/esp-thread-br/issues) if you find a bug or have a feature request. Please check existing Issues before opening a new one.

* If you're interested in contributing to ESP-THREAD-BR, please check the [Contributions Guide](https://docs.espressif.com/projects/esp-idf/en/latest/contribute/index.html).
