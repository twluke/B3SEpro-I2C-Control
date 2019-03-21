# B3SEpro-I2C-Control

1. cfg

This is a shell script to control B3SEpro (Twisted Pear Audio) via I2C, focusing on the 128fs function of the es9028pro/es9038pro DACs in sync mode. Also this is a modification of the original script specified to es9018 DAC control written by Miroslav Rudišin (https://github.com/miero/botic-tools) and I'd like to be grateful to him for his great contribution.

For now, I'm using a set of Hermes-BBB (running on debian stretch with Botic driver) and Cronus to control B3SEpros (both of 9028pro and 9038pro) with the main setting based on the contents from Twisted Pear Audio GitHub page (https://github.com/twistedpearaudio/Buffalo-III-SE-Pro-On-Board-Firmware).

The setup is as follows:

* Make sure that the on-board firmware has been removed. Use an isolated I2C header for clean connection to B3SEpro (SCL, SDA and GND only; occasionally addition of a 3.3V line may be required).
* Short the DAC_RESET and DVCC GPIO pins on the B3SEpro by a jumper before powering-up and during operation.*

* *Recently I changed the way to connect the RESET and DVCC pins. Though the method above does not do anything harm to the DAC, I though that the better way to utilize this RESET pin is to apply a short period of active low after power-up so that the input is held high after that. To satisfy this condition, I introduced a 3-pin reset monitor (TCM809 from Microchip) to the DAC_RESET, DVCC and GND pins of the GPIO header. I will recommend this method for more secure use of the DAC.

* Make sure the DAC (0x48) is well recognized by sending: i2cdetect -r -y 1
* The condition to establish 128fs may vary dependent on the source frequency. For example, I usually play upsampled DSD512 sources by HQ player. In this case, a set of 45/49 clocks will be required with a jumper for clock divider set to 1/4 (I don't know why) instead of usual 1/2.
* If this script is confirmed to work, making a link in /etc/rc.local will be convenient for automatic setting.

The script is not yet completed and may vary day to day but I'm now convinced that it is feasible for daily use. I'd like to appreciate any comments or advices from you, thank you.

2. cfg.py

This is a python script to control B3SEpros via I2C online, originally written by francolargo at diyaudio and transferred here by his courtesy, adding a few modifications. I'm very grateful to him for this fancy script.

Usage: With this script you can select either serial/DSD input or SPDIF input on the fly. First of all, execute the command line below:

root@arm:~# python ./cfg.py &

Then send one of the command lines below according to your selection:

echo 'serial' | nc xxx.xxx.xxx.xxx 8192 or echo 'spdif' | nc xxx.xxx.xxx.xxx 8192 (here xxx.xxx.xxx.xxx is the IP address where the script is running; nc -q .1 xxx.xxx.xxx.xxx 8192 if nc xxx.xxx.xxx.xxx 8192 does not work) )

echo '+' | nc xxx.xxx.xxx.xxx 8192 or echo '-' | nc xxx.xxx.xxx.xxx 8192 will work for volume control.

