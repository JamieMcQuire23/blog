+++
title = "How to add WiFi capabilities to your Arduino Nano BLE Sense board"
date = "2021-08-17"
author = "Jamie McQuire"
+++

## Introduction

The Arduino Nano BLE Sense development board is a powerful piece of kit with multiple sensors (accelerometer, microphone, humidity, ...) included - perfect for anyone wanting to start out in IoT development. Fitted with a 
[nRF52840](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fstruct_nrf52%2Fstruct%2Fnrf52840.html) [1] microcontroller, the development board boasts an ARM Cortex-M4 processor, enabling the deployment of machine learning models
via TensorFlow Lite. Unfortunately, one of the downsides to this board is the lack of WiFi capabilities. Compared to the Arduino Nano 33 IoT development board, the wireless capabilities appear to have been exchanged for the wealth of additional sensors. Applications that record and store massive amounts of data require WiFi for fast wireless data transfers - something that I found could not be achieved using BLE. This blog will teach you how to add WiFi capabilities to your BLE Sense projects via the inclusion of the ESP-01. I found the whole process to be quite tedious and the lack of a single source of information made it quite the challenge! Hopefully anyone needing WiFi support for this board can read this blog and have a more straightforward setup. The blog will outline how to upgrade the firmware of an off-the-shelf ESP-01, configure it with the BLE sense board, and then connect the board to your local WiFi network.

## Equipment

* [Arduino Nano BLE Sense](https://store.arduino.cc/arduino-nano-33-ble-sense)
* [Arudino Uno Rev3](https://thepihut.com/products/arduino-uno-rev-3?variant=32106770530366&currency=GBP&utm_medium=product_sync&utm_source=google&utm_content=sag_organic&utm_campaign=sag_organic&gclid=Cj0KCQjw0emHBhC1ARIsAL1QGNevBR7mGPRkpdbQlG4ddNRNB-DKE7xGAASXi7iC2-PE5yFi4kwBoMcaAqqZEALw_wcB) (for programming the ESP-01)
* [ESP8266 ESP01S 1MB Serial Wi-Fi WIFI Transceiver Module 802.11 b/g/n](https://www.ebay.co.uk/itm/284156792200?_trkparms=ispr%3D1&hash=item4229108d88:g:CWcAAOSw83lgCsHk&amdata=enc%3AAQAGAAACgPYe5NmHp%252B2JMhMi7yxGiTJkPrKr5t53CooMSQt2orsSwcmzw5CLtzTE60FqHcnq2N4ut5Ttm5Ru3vp0lIZTqEOMKce1Jq6hSEW9B7BdkXsRxsIJySlwKfkpUfC0HRbTnDdKZx%252Bxd9uDD3053UK0GaCoV%252FRiE8VQtGPB8CH7U5zAdFtm7bMY%252FbPWQVK0lQs3LgiA9As13U%252BGv7irOwV%252BDHMlD2GdP2aWBgfWSRd3Y9llcvi1ygPEVzUZUUks6ZiBMYYHs%252FfVef0fN1Ont64UZoaCn9xYVxtMzulDISne8p9uJ%252BEyT5G6wrlW2WWQ%252Fe5yV3dzLPIDmMp6m7YrEyUBFkey%252Bq3MwQmjMsN1iwn7%252F%252B88yEJeeAdbD6D9npJNMe3bOH5qjO8NrL0fWVw3n9CBC5Jziur0lNznRrIhx3SXryXZfFu94soU2UM%252BC73fNBRyQAonJ9Szr3gWJ74DwZOMJOFaThsd6p%252FYe7S9Zt%252Bpr7SCv%252FTX3EjM8kAkw%252BZI%252Bs%252F6IG%252FKqNVtLpipeja4AWgLstiN9p1OK7X%252FT52etIsrf9FzkiuBFiYfs7gh19UJZ5X0qOf2hMuDBWokn1%252B7JJLXC4qnBtEvNBlVffxwjdKi72AD9vtu%252FzkA8zZ35%252BcxS0pz1yCStPlG6bAmuotCDe7dQaKd2gNkpmsmdfOfCR3CQAMUXN3rsjf63ZxicP1fNCPMDW2jfAbItTGIAfLS5%252F9eXYhat3LTfGRvh9FuL7VmOI1QTgAR3vN0eQECT8BufLqboZResKudNocDFqX%252FnFbhXBe9tBXK2bWRaA9PnORx7DYqekMpGk9YUnqz9GHvEsy8L6ZX9PoWkeHtZQ378ANiWgw%253D%7Campid%3APL_CLK%7Cclp%3A2334524)
* [Breadboard x2](https://www.ebay.co.uk/itm/284000081310?_trkparms=ispr%3D1&hash=item421fb9559e:g:PdEAAOSwvChfUhtF&amdata=enc%3AAQAGAAACgPYe5NmHp%252B2JMhMi7yxGiTJkPrKr5t53CooMSQt2orsSwcmzw5CLtzTE60FqHcnq2C8IqtTsqpHnEVyIzlS%252B12i4cbFXpPVEYbr7r69hPiKhY9ahHVcDdPYaSAMP6mvHYkR3V4cGGUuviA81TI1rDZdZ3nw4DixPCd5pTua9wRkHT1MngClY%252Fq0hlxsQ%252FYe0P03U%252BF6I%252BLJtv0owUwJ%252Bj4PmPULysNt%252BxnYG5B5O0QJlzsDpkaCYx%252Bkw0BODdHbk0MYwy00XoZK0aTGk3JeDkDFwL0GNUS%252Fz9IcEenHkSYN8wcL%252BbHjTjnzAIPdtM0JZUjWuqqlFHzynHeCMCtoLCFcFjlwLBxF9SN3YYG0kBCppMYlyiEQqV9e6tFYxwfKXS%252F79MVrbpxwIjzgSk5X34wQNyDu2r69%252F3hsZYETuBboUhElPCZZ7WEYllfEu1hBg0mBsk17Snrp4KGd9vMLVDv3t6o8PbFxQmFGZ7%252Bvdn1USVOmo7rlxYSYHrsMEIGSNL%252BvZRlAKdTORsbQPWFMgt8HFZyURbSnxcBYK%252FDdAlmzLKguYaKLAHtzeP90QbClqtflqRyDytfD8M2vvWRAlKOPElbr%252FkO%252FK1o4P%252BuLrY3q%252FoxENwcxLt9qRQo%252FCPZKff4aG2e%252FiUtuoqiIdmRVyT7l0%252Bo66HzDd8sCyX2EEgPxd7nxvPj%252FWE7QzTWAy6LzsvVje%252F6sEHYkAF1lx7rt%252B%252Fejuv0VNRyaSb22penyQhCbdYEpb0i7ltz9bZQF0LaMBOMURZJ5y8zd0Q90RKPksAf%252BeHB%252BZ5FFiPODfjG%252Bcwjoc2httt8nY2Pft09LVhaAnRLCGCGMCwxod3Xv4Rp6dH68%253D%7Campid%3APL_CLK%7Cclp%3A2334524)
* [Jump Wires](https://www.ebay.co.uk/itm/164282038373?_trkparms=ispr%3D1&hash=item263ff8f865:g:sA0AAOSwCFhfBeiU&amdata=enc%3AAQAGAAACkPYe5NmHp%252B2JMhMi7yxGiTJkPrKr5t53CooMSQt2orsSLY2M1Gjmuwt9c03vWNfiRqWGGC5iqHwQUQrkLthq0SVpolLym2nIp2HdvoWqYvO%252F3YI72RDgnSeBpy57bbwkVRG6RTHtiPMaSJJM0q39HUqa4%252BIkXfLNeEIH8CoCz%252BcK4FEtU%252FJPC8jkcIt8p9RHHzqriqWulkBb6mj6mLZsmdoDhCkzOWEOmWyAFgxlFiev69y2BecSfWs8SD8g9GV3ppPS2Vwiwp9ABLQ6AV1d%252B3pmdPGlvWEHBea%252BLDdxz9rLy6lJR%252FOgUKwkslwC%252Fb7lIYAD5LgNzMgZnGYc6E3gXqWQypwlqDpVftUMdZDftnEinZb1TRHvtmdfdkYMKm0krvVDWLrbCFUlBZS9wzXiB0AickPugGQ7SKk5cJxYtWhi50wyH1bV1X1xfaFp7lKVswIFNj4GK1wxrQNAsUXFi%252FHJBJPt%252Fv0yoFSkIy7zZXiKrDPfvaOyqut%252BplYdw16OlwI1HQ5GhTPSBbH274%252BFsHPfpoRWeiruVgqs%252Fr3Xbky9xzzylFM4QJ4SUoyRbTiK%252FxEfUr6X3HLwxZNwdPEuTXJXJOI94C7H89LRTtO3v0vF2klTmXvBTX6apH9VFqt8KHU2JV1JN0jY9019zMXk%252BhkE2AsF5tGat1tEObQdxJP31Al1nyNrLCnKMoZJH2ucvjCm6SmSGbZVhdeu47s11TLElrAX18Gb%252FoAOkk5CRTPejF0vL9%252FQ9Vx1mPAaWaJV8q9d8c6PzgUgpDvSDbut%252F6jCSzyQvIXWu0OcNoF9JwQ5dodeEAa9hQpdfy5Nwl7STWq7n%252FodExmBK9yU3EhKTNIAlE3n%252F0FFcM2Jt83tSgF4%7Campid%3APL_CLK%7Cclp%3A2334524) (always good to have some around...)

## Configuring the ESP-01

For this project were going to use the `<WiFiEspAT.h>` [library](https://github.com/jandrassy/WiFiEspAT) - a fantastic community built ESP-01 library that enables Arduino devices to communicate over UART with the ESP-01. A requirement for this library is that the ESP-01 has to run AT firmware verison 1.7 or higher. When purchasing ESP-01 modules from vendors (I buy my demo components from Ebay) the firmware pre-installed on the module can vary. We are going to update the firmware of the ESP-01 using an Arduino Uno Rev3 (note: I chose this method as I had one of these devices lying around... there might be an alternative available!).

### Uno Connection

For the firmware update you are going to connect the Uno to the ESP-01 using the following connections:

- M2M jumper wire connecting the Uno's RESET to GND (bypass the bootloader and communciate directly with the ESP-01.
- M2M jumper wire connecting the Uno's 3.3V power supply to the +ve breadboard power-rail (need this to power the ESP-01 and the programming circuit).
- M2M jumper wire connecting the Uno's GND to the -ve breadboard power-rail (same thing as above but for GND).
- M2F jumper wire connecting the Uno's TX to the ESP-01 TX.
- M2F jumper wire connecting the Uno's RX to the ESP-01 RX.
- M2F jumper wire connecting the ESP-01's Vin to the 3.3V power supply on the breadboard.
- M2F jumper wire connecting the ESP-01's GND to the GND on the breadboard.
- M2F jumper wire connecting the ESP-01's EN to the 3.3V power supply on the breadboard.
- M2F jumper wire connecting the ESP-01's IO0 to breadboard GND (white wire in image below).
- Assemble the switch and resistor circuit seen in the image below (LED included for sanity checking the power...).
- M2F jumper wire connecting the ESP-01 RESET (purple wire in images below) to the button pin (this is the reset button).

![Uno Connections](/img/nano_ble_wifi/esp01_firmware_01.jpg)
![Uno Connections](/img/nano_ble_wifi/esp01_firmware_02.jpg)

**Note: The only pin not used on the ESP-01 is IO1**

Now you're ready to get talking to the ESP-01! Remove the IO0 wire from GND, connect your UNO, fire up the Arudino IDE, and then open a serial terminal. At the serial terminal you need to choose a **115200** baud rate along with the **Both NL and CR** option selected. Press the reset button and you should get a response (its mainly gibberish so don't worry...). Type in the serial terminal `AT` and then wait for a response - the serial terminal should now print *OK* and you should be ready to check the firmeware. Type `AT+GMR` to get the device firmware. Once you've got the firmware, press the reset button, connect the IO0 wire to ground, and then press the reset button again - the ESP-01 should now be in programming mode. 

**Note: Make sure here that you close the Arduino serial monitor**

![serial response](/img/nano_ble_wifi/serial_response.png)

### Flash the ESP-01

We are now going to flash the ESP-01 with the correct firmware. Download the [ESP8266 NonOS AT Bin V1.7.4](https://www.espressif.com/en/support/download/at?keys=) firmware binaries and extract the files to your chosen location. Following this you will need to install via pip the esptool using the command `pip install --upgrade esptool`. You can check the ESP8266 device information by running the following command:

`esptool.py --port /dev/ttyACM0 flash_id`

**Note: Make sure you change the port to the correct location!**

 `cd` to the binaries and then run the following command:

`esptool.py write_flash --flash_size 1MB 0x0 boot_v1.7.bin 0x01000 at/512+512/user1.1024.new.2.bin 0xfb000 blank.bin 0xfc000 esp_init_data_default_v08.bin 0xfe000 blank.bin 0x7e000 blank.bin`

![flashing](/img/nano_ble_wifi/cmd_flash.png)

At the end of this - press the reset button - and then remove the IO0 wire. This should *hopefully* upgrade your ESP-01 firmware to AT 1.7.4... We can verify this by opening the serial monitor, pressing the reset button, and then typing the command `AT+GMR` into the serial terminal. 

![success!](/img/nano_ble_wifi/serial_response_complete.png)

## Connecting the Nano

Now that our ESP-01 has the correct firmware, we can connect the Nano's RX to the ESP-01's TX and the Nano's TX to the ESP-01's RX. Our Nano is now connected to the WiFi module. On the software side of things we are going to start the boards second UART connection with `Serial1.begin(115200);` - ensuring the baud rate is set at 115200.

We can now use the `<WiFiEspAT.h>` module to create a simple WiFi client and connect to the network. The following code block should connect your Nano to the local WiFi network.

```c
#include "Arduino.h"
#include <WiFiEspAT.h>

char * ssid = "YOUR SSID GOES HERE";
char * pswd = "YOUR PASSWORD GOES HERE";

WiFiClient client;

int status = WL_IDLE_STATUS;	

void setup() {

	Serial.begin(115200);
	Serial1.begin(115200);
	
	while (!Serial1) {
		Serial.println(ERROR: SECOND SERIAL IS UNAVAILABLE");
	}
	
	WiFi.init(Serial1);

	// check if the WiFi module is available and cancel if unavailable  
	if (WiFi.status() == WL_NO_MODULE) {
		Serial.println();
	        Serial.println("Communication with WiFi module failed!");
	        // don't continue
	        while (true);
	}

  	// reconnection loop that attempts to connect to the WiFi network
  	while (WiFi.status() != WL_CONNECTED) {
    		Serial.print("SETUP: ATTEMPTING TO CONNECT TO WPA SSID: ");
    		Serial.println(ssid);
    		Serial.print("SETUP: STATUS CODE = ");
    		Serial.println(WiFi.status());
    		status = WiFi.begin(ssid, pass);
    		delay(5000);
  	}
	
}

void loop() {
	/*
	place here your code for using the now connected WiFi client
	*/
}

```

There you go! Your Nano should now be connected to the local WiFi network! If you have any issues with this feel free to send me an email. In my next blog I will extend the WiFi capabilities of this device to send MQTT readings to a local machine!

Jamie McQuire

## References

1. Nordic Semiconductor, nRF52840. Available at https://infocenter.nordicsemi.com/index.jsp?topic=%2Fstruct_nrf52%2Fstruct%2Fnrf52840.html
