# wavy guy + cronjob - an adhd cleaning reminder
![wavy-guy](https://github.com/user-attachments/assets/1715e92e-76d4-4370-b5b7-1b52e308a994)

A simple arduino MQTT client that triggers a relay switch on the wavy guy toy when a message is received via MQTT for a subscribed topic.
Since Arduinos don't have a OS and a clock/crontab functionality easily, using a RPI zero with a crontab schedule setup to trigger a MQTT message to be sent as reminder to cleanup in the evenings!

- Built upon the MQTT server already running in [Jeeves](https://github.com/traumverloren/jeeves).
- Adds a crontab schedule on the raspberry pi zero to trigger sending a cleaning reminder to the wavy guy as follows:
  - Run `crontab -e`
  - Add schedule (updated with USERNAME & PASSWORD):
    ```
    0 18 * * * mosquitto_pub -t reminder -m "time to clean" -u USERNAME -P PASSWORD
    10 18 * * * mosquitto_pub -t reminder -m "stop clean" -u USERNAME -P PASSWORD
    0 21 * * * mosquitto_pub -t reminder -m "time to clean" -u USERNAME -P PASSWORD
    ```

The MQTT broker allows clients to subscribe to topics. In this case, the arduino is subscribed to receive messages with topic "reminder". The crontab triggers publishing a message to the subscribed client. When the arduino receives this message, it sends a digital write of `HIGH` for 2 seconds to the non-latching relay. This triggers the latch to move to the closed position, therefore connecting the power supply and turning on the wavy guy for 2 seconds.

## Components:
- [Blowup wavy guy toy](https://www.amazon.de/dp/0762462876)
- [9V 2A 2.1mm barrel jack DC adapter](https://www.amazon.de/dp/B07NSMYZXS)
- MQTT Broker:
  - Raspberry Pi Zero W
- MQTT Client:
  - [Adafruit Feather Huzzah](https://www.adafruit.com/product/2821)
  - [Adafruit Feather Non-Latching Relay](https://www.adafruit.com/product/2895)

**Note:** I chose the Feather Huzzah & an old Raspberry Pi Zero W for this product since I already had them and reused them from previous art projects. There are many ways to approach a project like this, but this made sense for me with the materials I already had available.
