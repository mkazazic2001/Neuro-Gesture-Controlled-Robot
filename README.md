# Neuro-Gesture-Controlled-Robot
Controls small two-wheeled bot with PWM/Servo Pi HAT and Raspberry Pi 3 using Myo armband and EMOTIV EPOC

## Overview :
* Robot wheels move according to armband gyroscope axes
* EMOTIV overrides robot to force stop when user is not concentrating

## OS / Software Versions :

###### Raspberry Pi 3 OS : Raspbian GNU/Linux 9 (stretch)
###### Python Version : 2.7.13
###### GPIO Version: 0.6.3

## Libraries / SDKs / etc. :
*  [PyNetworkTables](https://github.com/robotpy/pynetworktables) - for using networktables between the Rasp. Pi and the PC
#### On the Rasp. Pi: 
*  [PyoConnect 1.0](http://www.fernandocosentino.net/pyoconnect/) - for using Myo armband with the Pi
*  [Adafruit PWM Servo Library](https://github.com/adafruit/Adafruit-Raspberry-Pi-Python-Code/tree/legacy) / [BBIO 12C](https://learn.adafruit.com/setting-up-io-python-library-on-beaglebone-black/i2c) - for driving motors with 16-Channel PWM Pi HAT
* Pynetworktables
#### On the Local PC :
* [Emotiv SDK](https://github.com/Emotiv/community-sdk) - for utilizing Emotiv EPOC information
* Pynetworktables

## Installing Libraries on Rasp. Pi:
#### PyoConnect :
Plug in bluetooth adapter for armband into Pi.
```
// permission to ttyACM0 - must restart linux user after this
sudo usermod -a -G dialout $USER

// dependencies
sudo apt-get install python-pip
sudo pip install pySerial --upgrade
sudo pip install enum34
sudo pip install PyUserInput
sudo apt-get install python-Xlib
sudo apt-get install python-tk
```
Now reboot.

Download and unzip PyoConnect 1.0 folder.
Then move files to folder with the code that will be used.
> This final step is already done in this repo.

#### Pynetworktables :
```
sudo pip install pynetworktables
```
#### Servo PWM Library:
Install RPi.GPIO library.
```
sudo apt-get update

#For Python 2
sudo apt-get -y install python-rpi.gpio

#For Python 3
sudo apt-get -y install python3-rpi.gpi
```
Install I2C tools
```
sudo apt-get install python-smbus
sudo apt-get install i2c-tools
```
Install Kernel Support with `sudo raspi-config`

> Interfacing Options/Advanced > I2C > Enable

```
sudo reboot
```
After rebooting, detect HAT `sudo i2cdetect -y 1`, which will output a table.
> It should show two I2C addresses are in use – 0x40 and 0x70.

|  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | a | b | c | d | e | f |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ---|
| 00 : | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- |
| 10 : | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- |
| 20 : | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- |
| 30 : | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- |
| 40 : | 40  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- |
| 50 : | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- |
| 60 : | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- |
| 70 : | 70  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- | --  | -- |

> [In the terminal](https://cdn-learn.adafruit.com/assets/assets/000/003/055/medium800/learn_raspberry_pi_i2c-detect.png?1396790698)

Finally, downloading the code.

The older version listed below is what was used in this project.

#### Old:
Download the code (legacy branch)
```
sudo apt-get install -y git build-essential python-dev
git clone -b legacy --single-branch https://github.com/adafruit/Adafruit-Raspberry-Pi-Python-Code.git
```
Use the files in Adafruit_PWM_Servo_Driver.
#### New:
```
sudo apt-get install -y git build-essential python-dev
git clone https://github.com/adafruit/Adafruit_Python_PCA9685.git
cd Adafruit_Python_PCA9685
sudo python setup.py install
```
## Notes :
* The Pi keeps a [static ip address](http://www.circuitbasics.com/how-to-set-up-a-static-ip-on-the-raspberry-pi/) as does the PC
* The PC and Pi connect to same private mobile hotspot
* The Pi runs the program "[main.py](https://github.com/mkazazic2001/Neuro-Gesture-Controlled-Robot/tree/master/Rasp-Pi)" when [on startup using .bashrc](https://www.dexterindustries.com/howto/run-a-program-on-your-raspberry-pi-at-startup/#bash)
