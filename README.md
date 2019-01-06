# B3SEpro-I2C-Control

This is a shell script to control B3SEpro (Twisted Pear Audio) via I2C, focusing on the 128fs function of the es9028pro/es9038pro DACs in sync mode. Also this is a modification of the original script specified to es9018 DAC control written by Miroslav Rudišin (https://github.com/miero) and I'd like to be grateful to him for his great contribution.

For now, I'm using a set of Hermes-BBB (running on debian stretch with Botic driver) and Cronus to control B3SEpros (both of 9028pro and 9038pro) with the main setting based on the contentns from Twisted Pear Audio GitHub page.

The setup is as follows:

1. Use an isolated I2C header for connection to B3SEpro (SCL, SDA and GND only; no 3.3V line required).
2. Short the DAC_RESET (1) and DVCC (2) GPIO pins on the B3SEpro by a jumper before powering-on and during operation.
3. Make sure the DAC (0x48) is well recognized by sending: i2cdetect -r -y 1
4. The condition to establish 128fs may vary dependent on the source frequency. For example, I usually play upsampled DSD512 sources by HQ player. In this case, a set of 45/49 clocks will be required with a jumper for clock divider set to 1/4 (I don't know why) instead of usual 1/2.
5. If this script is confirmed to work, making a link in /etc/rc.local will be convenient for automatic setting.
