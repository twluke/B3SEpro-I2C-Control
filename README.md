# B3SEpro-I2C-Control

This is a shell script to control B3SEpro (Twisted Pear Audio) via I2C, focusing on the 128fs function of the es9028pro/es9038pro DACs in sync mode.

For now, I'm using a set of Hermes-BBB (running on debian stretch with Botic driver) and Cronus to control B3SEpros (both of 9028pro and 9038pro).

The setup is as follows:

1. Use an isolated I2C header for connection to B3SEpro (SCL, SDA and GND only; no 3.3V line required).
2. Make sure the DAC (0x48) is well recognized by this connection as shown below:

[CODE]root@arm:~# i2cdetect -r -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: 20 -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- UU
40: -- -- -- -- -- -- -- -- 48 -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --   
[/CODE]
3. Short the DAC_RESET and DVCC GPIO pins on the B3SEpro by a jumper during operation.
4. The condition to establish 128fs may vary dependent on the source frequency. For example, I usually play upsampled DSD512 sources by HQ player. In this case, a set of 45/49 clocks will be required with a jumper for clock divider set to 1/4 (I don't know why) instead of usual 1/2.
5. If this script is confirmed to work, making a link in /etc/rc.local will be convenient for automatic setting.
