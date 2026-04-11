---
{"dg-publish":true,"permalink":"/esphome-nous-a1-t-smart-plug-ev-power-usage/"}
---

#it #esphome #grafana

This month my EV car (Leapmotor T03) should arrive, and i need a way to track my power consumption and how much i spend on charging the car.

After a failed attempt to flash tasmota on a Sonoff S60TPF, i bought a Nous A1T Smart plug that come with Tasmota flashed.

Thanks to this [thread](https://community.home-assistant.io/t/nous-a1t-tasmota-esphome/522530/7) i managed to flash esphome (using this device template https://devices.esphome.io/devices/nous-a1t/).

I've added the prometheus component following the documentation https://esphome.io/components/prometheus/

> Updating the image using the webui doesn't work, it works only using `esphome run`

And finally i stole an already well done grafana dashboard from [here](https://github.com/t0mer/tasmota-exporter)

That's it! Now i have a full local setup for tracking Energy consumption :)