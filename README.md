# Smart Classroom MQTT IOT Extension

This extension is modified from [elecfreaks/pxt-esp8266iot](https://github.com/elecfreaks/pxt-esp8266iot). This Version simplifies the steps of connecting WiFi and MQTT for students.

This extension is targeted for [ESP8266](https://en.wikipedia.org/wiki/ESP8266) Wifi modules and Inspiration Board. Therefore, students can connect the SmartClassroom Hub and Control the devices through MQTT.

![](https://github.com/ewenyau123/Education-Inspiration-Development/blob/master/Inspiration_board.png)

| Extension Element | MicroBit Pin |
| ----------------- | ------------ |
| OLE | 19,20 pin   |
| RGB LED x3        | P16          |
| WiFi Module       | P8-RX,P12-TX |
| Push Button x3    | P0, P1, P2   |
| GPIO PIN Header   | 1st row: P1-7|
|                   | 2nd row: P9-11, 13-16|
| I2C Pin Sockets   | 19,20 pin    |

## Basic Usage
1. Download `IFS_IOT.hex`
2. Create a new Project on [Microsoft Makecode/microbit](https://pxt.microbit.org)
3. Click `add Extension`
4. Click `import file`
5. Select `IFS_IOT.hex`
6. Use `SmartClassroom` drawer in extension side bar to edit the blocks
7. Click `Download` to download the project to your micro:bit Board  

## Example with Sensors

### Environment Sensor dht20
Prepare dht20 Sensor and connect correctly with Inspiration Board.

Add `Grove` Extension for reading the sensor dht20.

Find and arrange sensor dht20 blocks in editor. 

```blocks
input.onPinPressed(TouchPin.P0, function () {
    SmartClassroom.publishMqttMessage("" + grove.aht20ReadTemperatureC(), "smartclassroom/demo", SmartClassroom.QosList.Qos0)
})
input.onPinPressed(TouchPin.P2, function () {
    SmartClassroom.publishMqttMessage("" + grove.aht20ReadHumidity(), "smartclassroom/demo", SmartClassroom.QosList.Qos0)
})
input.onPinPressed(TouchPin.P1, function () {
    SmartClassroom.publishMqttMessage("" + grove.aht20ReadTemperatureF(), "smartclassroom/demo", SmartClassroom.QosList.Qos0)
})
SmartClassroom.connectWifi("your_ssid", "your_pwd")
SmartClassroom.setMQTT(
    SmartClassroom.SchemeList.TCP,
    "6A13",
    "6A13",
    "6A13abcd"
)
SmartClassroom.connectMQTT("smartclassroom.lan", 0, false)
pins.setPull(DigitalPin.P0, PinPullMode.PullUp)
pins.setPull(DigitalPin.P1, PinPullMode.PullUp)
pins.setPull(DigitalPin.P2, PinPullMode.PullUp)
```

### LED Control
Add `Neopixel` Extension for displaying different color on the board.

Find and arrange sensor dht20 blocks. 

Define the number of LEDs.

#### ColorLoop Mode
```blocks
pins.setPull(DigitalPin.P16, PinPullMode.PullNone)
let no_of_LEDs = 3
let strip = neopixel.create(DigitalPin.P16, no_of_LEDs, NeoPixelMode.RGB_RGB)
strip.setBrightness(255)
basic.forever(function () {
    strip.clear()
    strip.setPixelColor(0, NeoPixelColors.Red)
    strip.setPixelColor(1, NeoPixelColors.Blue)
    strip.setPixelColor(2, NeoPixelColors.Green)
    strip.show()
    basic.pause(500)
    strip.clear()
    strip.setPixelColor(0, NeoPixelColors.Green)
    strip.setPixelColor(1, NeoPixelColors.Red)
    strip.setPixelColor(2, NeoPixelColors.Blue)
    strip.show()
    basic.pause(500)
    strip.clear()
    strip.setPixelColor(0, NeoPixelColors.Blue)
    strip.setPixelColor(1, NeoPixelColors.Green)
    strip.setPixelColor(2, NeoPixelColors.Red)
    strip.show()
    basic.pause(500)
})
```

#### Colorful Mode
```blocks
pins.setPull(DigitalPin.P16, PinPullMode.PullNone)
let no_of_LEDs = 3
let strip = neopixel.create(DigitalPin.P16, no_of_LEDs, NeoPixelMode.RGB_RGB)
strip.setBrightness(255)
basic.forever(function () {
    strip.clear()
    strip.setPixelColor(0, NeoPixelColors.Red)
    strip.setPixelColor(1, NeoPixelColors.Red)
    strip.setPixelColor(2, NeoPixelColors.Red)
    strip.show()
    basic.pause(500)
    strip.clear()
    strip.setPixelColor(0, NeoPixelColors.Green)
    strip.setPixelColor(1, NeoPixelColors.Green)
    strip.setPixelColor(2, NeoPixelColors.Green)
    strip.show()
    basic.pause(500)
    strip.clear()
    strip.setPixelColor(0, NeoPixelColors.Blue)
    strip.setPixelColor(1, NeoPixelColors.Blue)
    strip.setPixelColor(2, NeoPixelColors.Blue)
    strip.show()
    basic.pause(500)
})
```

### OLED Monitor
Prepare the OLED Monitor 128X64 and connect correctly with Inspiration Board

Add `OLED12864_I2C` Extension for displaying String on the OLED Monitor.

Find and arrange OLED12864_I2C Monitor blocks. 

```blocks
OLED12864_I2C.init(60)
OLED12864_I2C.zoom(false)
OLED12864_I2C.invert(false)
OLED12864_I2C.draw()
OLED12864_I2C.rect(
    5,
    5,
    125,
    62,
    1
)
OLED12864_I2C.showString(
    3,
    3,
    "Hello!",
    1
)
```