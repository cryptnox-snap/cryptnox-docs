# Welcome to Cryptnox
## Dev for Snap
### Manual Activate 


- **When Install Manual or without gadget or not yet store approval**

 
   - Services 
		  
		  snap restart cryptnox.pcscd 
	
	- Connection

			snap connect cryptnox:hardware-observe 
			snap connect cryptnox:raw-usb
		
	- Connection with I2C

			snap connect cryptnox:i2c pi:i2c-1 # Connect i2c - i2c-1
	
	-   Check
    
			snap connections cryptnox
			snap services cryptnox
		

pcscd services is need to connect with hardware and must be started. connect is just for onetime setup who is using manual install snap.





## i2c

### GPIO Pin

![](https://cdn.sparkfun.com/assets/learn_tutorials/1/5/9/5/GPIO.png)

### GPIO Pin - GROUP

![](https://www.theengineeringprojects.com/wp-content/uploads/2021/03/raspberry-pi-4.png)

![Raspberry Pi GPIO Pin Reference](https://i0.wp.com/build5nines.com/wp-content/uploads/2018/08/RaspberryPI_GPIO_Layout_WithOpsOverlay.png?fit=783%2C506&ssl=1)

### GPIO I2C-1 
| BCM PIN|	GPIO NUMBER	|	NAME	| GROUP |
|-------|-----------|-------|-----------|
| 3 	| GPIO 2 	| SDA	| I2C-1 	|
| 5 	| GPIO 3 	| SCL	|  I2C-1 	|
| 4 	| 5v PWD 	| 5V	| Power 	|
| 6 	| GND 		| GND	| Ground 	| 

### NFC v4 
- Available 
	- I2C
	- SPI
	- HSU / UART
![PN532 Pinout, Interfacing with Arduino, Applications, Features](https://microcontrollerslab.com/wp-content/uploads/2020/01/PN532-NFC-RFID-Module-Pinout.jpg) 

![zoomed image](https://i5.walmartimages.com/asr/052853ed-1738-4efb-9dfa-a71e70f2ebf4_1.aeab253cd0ca60b5ea8965c7b664dcf8.jpeg)

**NOTE** 
- NFC v4 for switch configure 
- i2c is 1 0

|  | PIN 1 | PIN 2 | 
|----|----|----|
|NAME| ON | KE | 
|NAME|SEL0|SEL1|
||||
|L/H| H|L|
|0/1| 1 | 0| 

### I2C Test from Raspberry Pi

I2C [i2c-1] is need smbus and i2c-tools
if you are using vanilla raspberry pi images 

Install python-smbus or python3-smbus and i2c-tools
```
sudo apt-get install -y python-smbus 
sudo apt-get install -y i2c-tools
```
Cryptnox snap is already install 
- cryptnox-i2cdetect 

```sudo cryptnox-i2cdetect -y 1```
this command is only for test i2c bus 1, if you want to try 0 1 2 ..
also can check with ```-l``` switch
```sudo cryptnox-i2cdetect -l```


### I2C Driver Activate for PCSC

Pcsc is using acr and other NFC driver, but ```i2c``` or ```spi``` is not auto-detect. you need to install and configure it. when you using manual configure or manual install on raspberry pi. but cryptnox-image and cryptnox-pi-gadget is auto-configure for it. no need other config and setting.

for the raspberry pi
- install and build 
```
sudo apt-get install -yq autoconf automake make build-essential
sudo apt-get install -yq libnfc6 libnfc-examples


wget -O ifdnfc.tgz \
"https://sourceforge.net/projects/ifdnfc/files/latest/download"

tar xvzf ifdnfc.tgz

cd ifdnfc* 

autoconf -is 

./configure --prefix=/usr

make

sudo make install
```

- check config file 
For NFC
```
sudo nfc-scan-device -v
sudo nfc-list -v
```
It's see the your pn532 devices ? It's ok.

If not. follow the this step.

- `/etc/reader.conf.d/libifdnfc`
- `/etc/nfc/libnfc.conf`

```
sudo vi /etc/nfc/libnfc.conf
```
add these line
- IFD-NFC
- pn532_i2c:/dev/i2c-1
```
device.name = "IFD-NFC"
device.connstring = "pn532_i2c:/dev/i2c-1"
```

```
sudo vi /etc/reader.conf.d/libifdnfc
```
add these line
- FRIENDLYNAME      "IFD-NFC"
- LIBPATH           /usr/lib/libifdnfc.so
- CHANNELID         1
```
FRIENDLYNAME      "IFD-NFC"
LIBPATH           /usr/lib/libifdnfc.so
CHANNELID         1
```

and test again
```
sudo nfc-list -v
sudo nfc-scan-device -v

sudo ifdnfc-activate 
# or 
sudo ifdnfc-activate yes
```

now you already activate IFD-NFC and it's support for PCSC

```
sudo service pcscd restart  # its RPi
sudo snap restart cryptnox # It's Core and Cryptnox 
```
and 
```
pscs_scan # RPi
or 
cryptnox.pcsc-scan # Core or Cryptnox Snap
```
Now you can use
```
python -m cryptnox # RPI
# or
cryptnox.card # Core and Snap
```
