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

## 12 - Make the new voltages permanent

**TODO (W.I.P guide) **
