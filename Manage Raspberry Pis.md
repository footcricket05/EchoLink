# EchoLink: Wireless Multi-Room Audio System For Home 🎶🏡

Turn your home into a centralized audio haven with EchoLink, a DIY wireless multi-room audio system using Raspberry Pi's and affordable Bluetooth speakers. 🛠️

## Basic Hardware 🚀
Here's what you need to create your WiFi-based multi-room audio system:

<table>
  <tr>
    <td>
      <a href="http://amzn.to/2p9RVhQ"><img src="https://raw.githubusercontent.com/skalavala/skalavala.github.io/master/images/rpi3.jpg" alt="Raspberry Pi 3" /></a>
    </td>
    <td>
      <a href="https://amzn.to/2VcAPMk"><img src="https://raw.githubusercontent.com/skalavala/skalavala.github.io/master/images/rpi-power.jpg" alt="Raspberry Pi 3 power Supply" /></a>
    </td>    
    <td>
      <a href="https://amzn.to/2Vj7iAW"><img src="https://raw.githubusercontent.com/skalavala/skalavala.github.io/master/images/audio-cable.jpg" alt="3.5mm Audio Cable" /></a>
    </td>
    <td>
      <a href="http://amzn.to/2pU2V1Y"><img src="https://raw.githubusercontent.com/skalavala/skalavala.github.io/master/images/bluetooth-speaker.jpg" alt="Bluetooth Speakers" /></a>
    </td>
  </tr>
  <tr>
    <td><a href="http://amzn.to/2p9RVhQ">Raspberry Pi 3</a></td>
    <td><a href="https://amzn.to/2VcAPMk">RPi3 Power Supply</a></td>
    <td><a href="https://amzn.to/2Vj7iAW">Audio Cable</a></td>
    <td><a href="http://amzn.to/2pU2V1Y">Portable Speakers</a></td>
  </tr>
</table>

## Software Components 🖥️
Utilize Mopidy, Snapcast server, and client software for seamless audio distribution. Mopidy acts as the media player, and Snapcast facilitates broadcasting to multiple clients.

1. [Install Mopidy](https://github.com/footcricket05/EchoLink/blob/main/Install%20Mopidy.md)
2. [Install Snapcast Server](https://github.com/footcricket05/EchoLink/blob/main/Install%20Snapcast%20Server.md)
3. [Install Snapcast client on each Raspberry Pi](https://github.com/footcricket05/EchoLink/blob/main/Install%20Snapcast%20Client.md)
4. [Setting up PulseAudio](https://github.com/footcricket05/EchoLink/blob/main/Setup%20Pulseaudio.md)
5. [Setting Snapcast Client Names](https://github.com/footcricket05/EchoLink/blob/main/Naming%20Clients.md)  
6. **Optional** [Enable Airplay](https://github.com/footcricket05/EchoLink/blob/main/airplay.md)
7. **Optional** [Setup Virtual Sound Card on VMWare ESXi 6.0 or Above](https://github.com/footcricket05/EchoLink/blob/main/vmware.md)

All set! 🎉 Enjoy the whole house audio system, play music, connect to home assistant, make announcements, or even integrate with online radio channels!

Optionally, install Mopidy Web Extensions for Spotify playlists. Integrate Mopidy with [Home Assistant (HA)](http://www.home-assistant.io) to view media players on the dashboard (Note: HA supports older versions of Snapcast server and clients).

Good luck! 😊

## Basic Raspberry Pi Administration using MQTT 🖥️🤖

![My Home Assistant View](https://raw.githubusercontent.com/skalavala/Multi-Room-Audio-Centralized-Audio-for-Home/master/images/image.png)

## Prelude

Managing multiple Raspberry Pis at home can be challenging. This guide introduces a Python program that listens for specific commands via MQTT, providing an easy way to perform basic management tasks such as restarting services or checking system status. This is particularly useful for automation and remote administration.

### Prerequisites

Make sure you have Python and the Paho MQTT library installed. Run the following command to install Paho MQTT:

```bash
sudo pip install paho-mqtt
```

## Python Client Program

Below is a Python program that runs as a daemon/service on each Raspberry Pi. It listens for predefined commands on an MQTT topic, allowing you to execute actions remotely.

```python
# OnSubscribe Callback
######################################
def on_subscribe(mosq, obj, mid, granted_qos):
    print("Subscribed: " + str(mid) + " " + str(granted_qos))

# OnLog Callback
######################################
def on_log(mosq, obj, level, string):
    print(string)

# Set event callbacks
mqttc.on_message = on_message
mqttc.on_connect = on_connect
mqttc.on_subscribe = on_subscribe

# Uncomment below line to enable debug/console messages
# mqttc.on_log = on_log

# Connect to MQTT using the username/password set above
mqttc.username_pw_set(MQTT_USERNAME, MQTT_PASSWORD)
mqttc.connect(MQTT_SERVER, MQTT_SERVER_PORT)

# Start subscribe, with QoS level 0
mqttc.subscribe(MQTT_TOPIC, 0)

# Continue the network loop, exit when an error occurs
rc = 0
while rc == 0:
    rc = mqttc.loop_forever()

# when the program exits, print the error code
print("MQTT Return Code: " + str(rc))
```

### Note: Update the MQTT server details and any other hardcoded values.

## Running as a Service

1. **Create Service File:**

   Run the following command to create a service file:

   ```bash
   sudo nano /etc/systemd/system/raspi-client.service
   ```

   Add the following contents, save, and exit:

   ```ini
   [Unit]
   Description=Raspberry Pi MQTT Command Listener
   After=network.target
   Requires=network.target

   [Service]
   ExecStart=/usr/bin/python /home/pi/pi_mqtt_cmds.py
   Restart=on-failure
   RestartSec=5

   [Install]
   WantedBy=multi-user.target
   ```

2. **Reload Daemon Config:**

   Reload the daemon config:

   ```bash
   sudo systemctl --system daemon-reload
   ```

3. **Enable and Start Service:**

   Enable the service:

   ```bash
   sudo systemctl enable raspi-client
   ```

   Start the service:

   ```bash
   sudo systemctl start raspi-client
   ```

4. **Optional Commands:**

   - Restart Service: `sudo systemctl restart raspi-client`
   - Check Status: `sudo systemctl status raspi-client`

## Testing

Now that the service is running, test it by publishing commands to the specified MQTT topic. Examples of commands include:

- `CMD_RESTART_SNAPCLIENT`: Restarts Snapcast client.
- `CMD_REBOOT_PI`: Reboots the Raspberry Pi.
- `CMD_SHUTDOWN_PI`: Shuts down the Raspberry Pi.
- `CMD_REPORT_WIFI_DETAILS`: Responds with WiFi information.
- `CMD_REPORT_DISK_DETAILS`: Provides disk usage statistics.

Modify and expand these commands based on your requirements. 

**Note:** There are various ways to achieve similar results; this is just one approach.

Good luck with your Raspberry Pi administration and automation journey! 🚀🤖
