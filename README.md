# What is this

This guide will show you how to undervolt the Core2Duo CPU of your old laptop :)

Tried in a Dell Inspiron 1525. Now I do not see it throttling at any moment, ant the laptop rarely needs to spin the fan. It feels amazing.


**Warning**: You are the only one responsible when running this script

## 1 - Download the module from this website:
http://www.linux-phc.org/forum/viewtopic.php?f=7&t=267&sid=a84c3809396f6386615afd059d69cabd

## 2 - Install the required dependencies:

```
dnf install -y kernel-devel dkms rpm-build
```

## 3 - Install the generated rpm

```
dnf localinstall ./phc-intel*.noarch.rpm
```

## 4 - Unload acpi\_cpufreq

```
rmmod acpi_cpufreq
```

## 5 - Load phc-intel

```
modprobe phc-intel
```

## 6 - Check phc-intel is loaded

```
lsmod | grep phc
```

## 7 - Check the CPU0 phc settings appeared

```
ls /sys/devices/system/cpu/cpu0/cpufreq/
```

## 8 - Download Prime95 for Linux

```
https://www.mersenne.org/download/
```

## 9 - Download the stqn configuration script

```
https://pastebin.com/NVwA8JEK
```

NOTE: You might need to adjust the variables at the top of the file

## 10 - Extract Prime95 so it is in the same folder as the script

```
tar zxvf p95v*.tar.gz
```

## 11 - Run the script as root :)

## 12 - Set the new voltages permanently

First of all, we will check that the phc_vids values are stored at the following file

```
cat /sys/devices/system/cpu/cpu0/cpufreq/phc_vids
```

We will create a script to set these voltages:

```bash
VIDS=$(cat /sys/devices/system/cpu/cpu0/cpufreq/phc_vids)
cat <<EOT >> /usr/bin/set_vids.sh
#!/usr/bin/bash
echo ${VOLTAGES} > /sys/devices/system/cpu/cpu*/cpufreq/phc_vids
EOT

chmod +x /usr/sbin/set_vids.sh
```

At the end, we will create a systemd unit to set the vids on boot.

```
cat <<EOT > /etc/systemd/system/set-vids.service
[Unit]
Description=Disable cdrom

[Service]
Type=oneshot
ExecStart=/usr/sbin/set_vids.sh

[Install]
WantedBy=multi-user.target
EOT
```

The module phc-intel needs acpi-cpufreq not loaded to work. It can be blacklisted as follows:

```
echo "blacklist acpi_cpufreq" > /etc/modprobe.d/blacklist-acpi-cpufreq.conf
```

