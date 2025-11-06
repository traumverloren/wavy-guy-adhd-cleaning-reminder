# wavy-guy-adhd-cleaning-reminder
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
