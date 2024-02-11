# Lenovo-Smart-Clock-2
## How to control wireless charing pad light

1. Root your clock, install Magisk and wireless ADB following tutorial https://github.com/ThomasPrior/LenovoSmartClock2?tab=readme-ov-file#enable-wireless-adb
2. In HA install Android Debug Bridge integration https://www.home-assistant.io/integrations/androidtv/ and enter your IP address, port (default is 5555 or 1234) and select androidtv.
3. Add your device in configuration.yaml
  
5. Enjoy

How it works?
First of all there is no charging pad led in /sys/class/leds. Whole communication between charging pad goes via /dev/ttyACM0. So I disassembled com.google.oem.apk and I found IAssistantOemAccessoryService with methods setLedBrightness(I) and links to service charge_base: [android.app.IChargeBaseManager]. The function setLedBrightness has index 7, and is expecting and value between 0 to 10, so finall service call look like this:
adb shell service call charge_base 7 i64 <Numbers ranging from 0 to 10>

Here is full list of charge_base function:
```
adb shell service call charge_base 1
ChargeBaseService: setUpdate:0

adb shell service call charge_base 2
ChargeBaseService: getMCUVersion:

adb shell service call charge_base 3
ChargeBaseService: getFirewareVersion:

adb shell service call charge_base 4
ChargeBaseService: checkSN:

adb shell service call charge_base 5
hargeBaseService: checkModel

adb shell service call charge_base 6
ChargeBaseService: getStatus:

adb shell service call charge_base 7
ChargeBaseService: setLedBrightness:0

adb shell service call charge_base 8
AssistantOemAccessoryService: Starting assistant oem accessory startPlay:

adb shell service call charge_base 9
ChargeBaseService: mcuUpdate, force?false

adb shell service call charge_base 10 
ChargeBaseService: setLedAndPower:0,power:0

adb shell service call charge_base 11
ChargeBaseService: initNative

adb shell service call charge_base 12
ChargeBaseService: release

adb shell service call charge_base 13
AssistantOemAccessoryService: OEM app: chargeStateCheck,listener_size:1

adb shell service call charge_base 14
ChargeBaseService: wirelessUpdate, force?false

adb shell service call charge_base 15
AssistantOemAccessoryService: OEM app: chargeStateCheck,listener_size:1

adb shell service call charge_base 16
AssistantOemAccessoryService: OEM app: chargeStateCheck,listener_size:2
```
