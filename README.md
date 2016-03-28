# I2C library for PiMotics
Open-source I2C expansion board library for PiMotics. The target for this project is to make a plugin that extended GPIO pins are treated as general GPIO pins.

# How to install

* Perform a system update/upgrade
```
sudo apt-get update && sudo apt-get upgrade
```

* Install the I2C packages (installs i2c-tools & 12c-tools)
```
sudo apt-get install python-smbus
```

* Configure your Raspberry Pi to support SPI
```
sudo raspi-config ==> select 'advance options' ==> Enable SPI and Enable it as default boot option
```

* Configure your Raspberry Pi to support I2C
```
sudo raspi-config ==> select 'advance options' ==> Enable I2C and Enable it as default boot option
```

* Finish the change
```
Finish ==> reboot
```

* Add the required users to the IC2 eco system
```
sudo adduser pi i2c
sudo adduser root i2c
sudo adduser pim i2c
```

# Testing availability of the I2C

* Detecting the I2C onboard raspberry pi module: (`i2c-1` = 1)
```
root@raspberrypi:~# sudo i2cdetect -l
i2c-1	i2c       	3f804000.i2c                    	I2C adapter
```

* Detecting your expansion board
  * When the board is NOT detected OR connected
  ```
  root@raspberrypi:~# sudo i2cdetect -y 1
       0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
  00:          -- -- -- -- -- -- -- -- -- -- -- -- --
  10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
  20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
  30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
  40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
  50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
  60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
  70: -- -- -- -- -- -- -- --
  ```

  * When the board IS detected AND connected
  ```
  root@raspberrypi:~# sudo i2cdetect -y 1
      0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
  00:          -- -- -- -- -- -- -- -- -- -- -- -- --
  10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
  20: 20 21 -- -- -- -- -- -- -- -- -- -- -- -- -- --
  30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
  40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
  50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
  60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
  70: -- -- -- -- -- -- -- --
  ```
  This shows that two I2C addresses are in use – 0x20 and 1x21.

# Testing the library

* Detecting a button press

```
In [1]: import sys
In [2]: sys.path.append('/opt/pimotics/pimotics/extensions/plugins/ABElectronics_Python_Libraries/library')
In [3]: from GPIOExpander import GPIOExpander

# Button IS NOT pressed
In [4]: GPIOExpander().read(7, 0x20, "input")
Out[4]: 0

# Button IS pressed
In [5]: GPIOExpander().read(7, 0x20, "input")
Out[5]: 1
```

* Toggling a component

```
In [7]: GPIOExpander().toggle(2, 0x20)
Out[7]: 1

In [8]: GPIOExpander().toggle(2, 0x20)
Out[8]: 0
```

* Reading a component it's current status
```
In [9]: GPIOExpander().read(2, 0x20)
Out[9]: 0

# Toggling a component for test purposes
In [10]: GPIOExpander().toggle(2, 0x20)
Out[10]: 1

In [11]: GPIOExpander().read(2, 0x20)
Out[11]: 1
```





