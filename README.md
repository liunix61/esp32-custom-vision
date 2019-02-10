ESP32-CUSTOM-VISION
===================

So, my company just got a free [ESP-EYE](https://www.espressif.com/en/products/hardware/esp-eye/overview) development board from Espressif (the maker of ESP32, ESP8266 chip) itself. It's developed officially by Espresif as an attempt to quickly get started to create image recognition or audio processing-related application.

Espressif develops a sample application that makes the most use of the board, namely: [**esp-who**](https://github.com/espressif/esp-who). To me, it's an awesome project that shows how to do speech recognition and face recognition, all done at the edge or on the board itself, not in the cloud.

While **esp-who** is great for making use of the board and to embrace edge-intelligence, I want to do something else. As a Microsoft Most Valuable Professional (MVP) of Microsoft Azure ([my profile](https://mvp.microsoft.com/en-us/PublicProfile/4020410?fullName=Andri%20%20Yadi)), I want to make use of Azure services, specifically Azure Custom Vision, to be used as cloud-based image recognition engine. It is exactly the reason I created this repo.

I did a live coding to show the step by step how to develop the firmware from scratch and show how to setup Azure Custom Vision. There're 4 videos and indeed long ones with the total of 5 hours. If you're keen to know the details, go to this [Youtube playlist](https://www.youtube.com/playlist?list=PLh27GVXl10PcpnDkodCiUaSc8Mgma7FxW&fbclid=IwAR2crTT5b8NWRlZ1zcuys_Qde_VkkA5TkFkh7GlD0dEXJJcdiJ6fxt3aB5w)

To see what we can do with the project, I made a video that shows you how we can do "live" face recognition:
[![LIVE RECOG](http://img.youtube.com/vi/-L6gU6Gga5o/0.jpg)](http://www.youtube.com/watch?v=-L6gU6Gga5o)

This project is made with `Espressif IoT Development Framework` ([ESP-IDF](https://github.com/espressif/esp-idf)). So, I assume you're already familiar with it and have the development environment set up. Please check [ESP-IDF docs](https://docs.espressif.com/projects/esp-idf/en/latest/) for getting started instructions.

## Getting Started

* Clone this repo, recursively: `git clone --recursive https://github.com/andriyadi/esp32-custom-vision.git`
* If you clone project without `--recursive` flag, please go to the `esp32-custom-vision` directory and run command this command to update submodules which it depends on: `git submodule update --init --recursive` 
* Create `secrets.h` file inside `main` folder. Explained below.
* On Terminal/Console, in root folder, do `make menuconfig`. Go to `App Configuration` --> `Select Camera Dev Board (ESP-EYE)`. Here you can select the development board, either: ESP-EYE or ESP-WROVER-KIT. Exit and save the menuconfig.
* Still in root folder, try to `make flash monitor`. Fingers crossed :)

## `secrets.h` file
Under `main` folder, create a file named `secrets.h` with the content:
```
#ifndef MAIN_SECRETS_H_
#define MAIN_SECRETS_H_


#define SSID_NAME "[YOUR_OWN_WIFI_SSID_NAME]"
#define SSID_PASS "[YOUR_OWN_WIFI_SSID_PASSWORD]"

#define AZURE_CV_PREDICTION_KEY "[YOUR_OWN_AZURE_CUSTOM_VISION_PREDICTION_KEY]"

#endif /* MAIN_SECRETS_H_ */
```

Replace all value with [...] inside quote.

## Azure Custom Vision
Obviously, you need to have access to Azure Custom Vision to make this project works. You can try for free at [customvision.ai](https://www.customvision.ai/). If you already have Microsoft Azure account, you're good to go.

In the live coding videos mentioned above, I explained and showed to get started with Azure Custom Vision. Watch [this video](https://www.youtube.com/watch?v=-MavqOWtUvI&list=PLh27GVXl10PcpnDkodCiUaSc8Mgma7FxW&index=2)

## Usage
Upon successful build and flashing the firmware to the board, on Terminal/Console you'll see the firmware runs and eventually show these lines:
```
I (2870) DXWIFI: SYSTEM_EVENT_STA_CONNECTED. Station: 44:79:57:61:72:65 join, AID: 45
I (6130) event: sta ip: 192.168.0.20, mask: 255.255.255.0, gw: 192.168.0.1
I (6130) DXWIFI: SYSREM_EVENT_STA_GOT_IP. IP Address: 192.168.0.20

I (6130) DXWIFI: WiFi connected
I (6130) APP: Starting web server on port: '80'
```
Take a look that there's: `IP address 192.168.0.20`. It's the IP address of the board when it's connected to specified WiFi Access Point. It will be different on your machine.

Now, open your browser and type `http://[BOARD_IP_ADDRESS]` with `[BOARD_IP_ADDRESS]` is the IP addrees you got above. You should see the `hello` text.

Now, type URL: `http://[BOARD_IP_ADDRESS]/capture`, you should see the captured image by the board's camera.

Then, type URL: `http://[BOARD_IP_ADDRESS]/recog`, the board will capture an image, send the image to Azure Custom Vision for inferencing, then show the detected face on the browser.

For showing live video streaming on the browser and do live recognition, you can use `http://[BOARD_IP_ADDRESS]/stream` URL.