# eth2wlan
The project enables ethernet to Wi-Fi data forwarding function (Ethernet to Wi-Fi bridge). It supports both station and access point mode.

## Overview
The similarities on MAC layer between Ethernet and Wi-Fi make it easy to forward packets from Ethernet to Wi-Fi and vice versa. This example illustrates how to implement a simple "router" which only supports forwarding packets between Ethernet port and Wi-Fi interface.

In this example, ESP32 works like a *bridge* between Ethernet and Wi-Fi, and it won't perform any actions on Layer3 and higher layer, which means there's no need to initialize the TCP/IP stack.

## Hardware
To run this example, it's recommended that you have an official ESP32 Ethernet development board - [ESP32-Ethernet-Kit](https://docs.espressif.com/projects/esp-idf/en/latest/hw-reference/get-started-ethernet-kit.html). This example should also work for 3rd party ESP32 board as long as it's integrated with a supported Ethernet PHY chip. Up until now, ESP-IDF supports up to four Ethernet PHY: `LAN8720`, `IP101`, `DP83848` and `RTL8201`, additional PHY drivers should be implemented by users themselves.

Besides that, `esp_eth` component can drive third-party Ethernet module which integrates MAC and PHY and provides common communication interface (e.g. SPI, USB, etc). This example will take the **DM9051** as an example, illustrating how to install the Ethernet driver in the same manner.

### Common Pin Assignments
See [here](https://github.com/espressif/esp-idf/blob/release/v4.4/examples/ethernet/README.md#common-pin-assignments) the common pin assignments for Ethernet examples.

### Common Boards
* wESP32
  | Parameter | Value   |
  | --------- | ------- |
  | type      | LAN87XX |
  | MDC       | 16      |
  | MDIO      | 17      |
  | PHY Address | 0     |

* Olimex ESP32-POE
  | Parameter | Value   |
  | --------- | ------- |
  | type      | LAN87XX |
  | MDC       | 23      |
  | MDIO      | 18      |
  | PHY Address | 0     |
  | Power PIN | 12      |

* Olimex ESP32-EVB
  | Parameter | Value   |
  | --------- | ------- |
  | type      | LAN87XX |
  | MDC       | 23      |
  | MDIO      | 18      |
  | PHY Address | 0     |

* LILYGO TTGO T-Internet-POE ESP32-WROOM LAN8270A Chip
  | Parameter | Value   |
  | --------- | ------- |
  | type      | LAN87XX |
  | MDC       | 23      |
  | MDIO      | 18      |
  | PHY Address | 0     |

* OpenHacks LAN8720
  | Parameter | Value   |
  | --------- | ------- |
  | type      | LAN87XX |
  | MDC       | 23      |
  | MDIO      | 18      |
  | PHY Address | 1     |

* Wireless Tag WT32-ETH01
  | Parameter | Value   |
  | --------- | ------- |
  | type      | LAN87XX |
  | MDC       | 23      |
  | MDIO      | 18      |
  | PHY Address | 1     |
  | Power PIN | 16      |

## Configuration
```bash
docker-compose run --rm idf idf.py menuconfig
```

In addition to the common configurations, you might need to update the default values of following configuration:

In the `Ethernet to WLAN Configuration` menu:
* Set the SSID and passphrase for Wi-Fi interface under `Wi-Fi SSID` and `Wi-Fi Password`.

## Build and Flash

Build the project and flash it to the board, then run monitor tool to view serial output:

```bash
docker-compose run --rm idf idf.py build flash monitor
```

To exit the serial monitor, type ``Ctrl-]``.

See the [Getting Started Guide](https://docs.espressif.com/projects/esp-idf/en/latest/get-started/index.html) for full steps to configure and use ESP-IDF to build projects.

## Troubleshooting

See [here](https://github.com/espressif/esp-idf/blob/release/v4.4/examples/ethernet/README.md#common-troubleshooting) the common troubleshooting for Ethernet examples.

* If you got error message like `WiFi send packet failed` when running the example, you may need to enlarge the value of `FLOW_CONTROL_WIFI_SEND_DELAY_MS` in "ethernet_example_main.c", because Ethernet process packets faster than Wi-Fi on ESP32.
* If you got error message like `send flow control message failed or timeout` when running the example, you may need to enlarge the value of `FLOW_CONTROL_QUEUE_LENGTH` in "ethernet_example_main".
