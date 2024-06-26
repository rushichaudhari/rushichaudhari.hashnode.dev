---
title: "T490 Fan Speed Configuration With Thinkfan"
datePublished: Tue May 24 2022 17:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clmctws1o01xvo9nvf4ee032d
slug: t490-fan-speed-configuration-with-thinkfan
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1694314551074/c89b379e-dd86-4f6e-8406-35102b774682.jpeg
tags: ubuntu, linux, cpu, thinkpad, cooling

---

I love Thinkpads. They are robust and durable. And no need to mention about their world class keyboards!

It works like a charm, but we need to configure it a bit for silence. Let's start!

### 1\. Install the necessary package

```plaintext
sudo apt install thinkfan
```

### 2\. Create the configuration file

After installing the package we need to create a config file in the following directory: `/etc/thinkfan.conf`

This file requires three main items:

* The fan information
    
* The temperature information
    
* Speed level for temperature levels
    

In order to provide fan information, we write this line:

`tp_fan /proc/acpi/ibm/fan`

For the temperature information we should scan the sensors of the device.

```plaintext
find /sys/devices -type f -name 'temp*_input'
```

Enter fullscreen mode Exit fullscreen mode

Running this comman should give you something like this but **without hwmon** keyword. **Don't forget to add it** in front of each line.

```plaintext
hwmon /sys/devices/platform/thinkpad_hwmon/hwmon/hwmon4/temp6_input
hwmon /sys/devices/platform/thinkpad_hwmon/hwmon/hwmon4/temp3_input
hwmon /sys/devices/platform/thinkpad_hwmon/hwmon/hwmon4/temp7_input
hwmon /sys/devices/platform/thinkpad_hwmon/hwmon/hwmon4/temp4_input
hwmon /sys/devices/platform/thinkpad_hwmon/hwmon/hwmon4/temp1_input
hwmon /sys/devices/platform/thinkpad_hwmon/hwmon/hwmon4/temp5_input
hwmon /sys/devices/pci0000:00/0000:00:18.3/hwmon/hwmon3/temp1_input
hwmon /sys/devices/pci0000:00/0000:00:18.3/hwmon/hwmon3/temp2_input
hwmon /sys/devices/pci0000:00/0000:00:02.1/0000:01:00.0/hwmon/hwmon2/temp3_input
hwmon /sys/devices/pci0000:00/0000:00:02.1/0000:01:00.0/hwmon/hwmon2/temp1_input
hwmon /sys/devices/pci0000:00/0000:00:02.1/0000:01:00.0/hwmon/hwmon2/temp2_input
hwmon /sys/devices/pci0000:00/0000:00:08.1/0000:06:00.0/hwmon/hwmon8/temp1_input
hwmon /sys/devices/virtual/thermal/thermal_zone0/hwmon5/temp1_input
```

Enter fullscreen mode Exit fullscreen mode

After adding these lines to the configuration, we need to add our fan speed data for each temperature level. This is a sane configuration for daily usage but **use at your own risk.**

```plaintext
# speed level | start temp | end temp

(0, 0,  55)
(1, 48, 60)
(2, 50, 61)
(3, 52, 63)
(4, 56, 65)
(5, 59, 66)
(7, 63, 32767)
```

Now the config file is ready!

### 3\. Enable acpi\_fancontrol

We need to enable `acpi_fancontrol` on kernel module options! Add this line to `/etc/modprobe.d/thinkpad_acpi.conf` file:

```plaintext
options thinkpad_acpi fan_control=1
```

Enter fullscreen mode Exit fullscreen mode

Activate kernel settings by running this:

```plaintext
sudo modprobe -r thinkpad_acpi && sudo modprobe thinkpad_acpi
```

Enter fullscreen mode Exit fullscreen mode

### 4\. Enable the thinkpan.service

In order to start the service any time you boot up your machine, we need to enable it!

```plaintext
sudo systemctl enable --now thinkfan.service
```

Enter fullscreen mode Exit fullscreen mode

### 5\. Done!

You can check if the service is running or not with this command:

```plaintext
sudo systemctl status thinkfan.service
```