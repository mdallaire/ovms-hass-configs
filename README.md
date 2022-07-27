# Sample MQTT configuration for Home-Assistant and OVMS

This repo contain some example MQTT configuration to add to your Home-Assistant that enable you to use your [OVMS](https://www.openvehicles.com).

## Caveats

All of these sensors and automations depend on MQTT and the MQTT refresh settings of your OVMS.

Some sensors tend to update more frequently than other and as such you can see some incoherence in your sensors. This is not a Home-Assistant problem but a OVMS

If you use a SIM card with data you have to find a balance between frequent updates and data usage costs.
As a reference I use the following settings with my OVMS Server V3 configuration.

**This is for reference only, it works for me and the resulting cost is OK for me with my provider (which is not hologram.io). YOU are responsible for your settings and the resulting data cost**

```
Update intervals
...connected: 60
...idle: 600
...on: 15
...charging: 60
...awake: 15
...sendall: 600
```


All of the sensors also have an "availability" configuration that will check if the OVMS is connected to the V3 server. If it is not, all the sensors will show as Unavailable until the connection is re-established. If you prefer to have stale values instead of unavailable you can comment out the availability section for each sensors.

## Pre-requisite

For this to work you will need to have your OVMS configured to use MQTT (refered to Server V3 or OVMS V3) and have your Home-Assistant configured with MQTT.

## How to use

The files content is meant to be copied over in your configuration and ajusted to your needs. [mqtt-client-name] and [vehiculeID] will need to be replaced with the mqtt client name of your OVMS V3 mqtt client and your vehiculeID.

### MQTT Sensors

The mqtt.yaml contains several sensors configuration that I found to be useful and interesting to have in Home-Assistant.

### MQTT Switch

I have a switch configured to start the pre-heating of the car. The tracking of the status can be a bit inconsistent and that is why I have an automation (that you can find in automations.yaml) to update the switch status to what is reported by the car. 

Be aware that the pre-heating switch will not care if the car is plugged in and electricity is available, it will pre-heat no matter what and will stop when its cycle is done or when the minimal battery level threshold of the leaf is met.

### MQTT Button

I have a button to perform a reboot of the OVMS module.

### Device Tracking

To configure your car as a device tracker you will need to create (or append to) a know_devices.yaml. You will then need to add the leaf location automation in the automations.yaml file to trigger updates to the device_tracker device when a new latitude or longitute is sent by OVMS.

# TODO

- Try to implement https://www.home-assistant.io/integrations/device_tracker.mqtt/
- Improvements on the Pre-Heat swith and status
