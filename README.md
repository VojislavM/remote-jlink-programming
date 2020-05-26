# remote-jlink-programming

In the reasent events many have found there selfs at home and not beaing able to go to office. For me phisicaly this was not something new as I have been working remote for caple of years now and I wanted to show one less known feature of Segger JLink which provide programming interface over the internet as the device you are programming is sitting next to you on the bench. 

## server configuration
There are three ways how to do programming with JLink. I will demonstrate the "LAN" way, where device is connected to one computer using JLink programmator and I am programming that chip over LAN connection. The computer with target device has Windows 10 OS and it is a simple process to start JLink server on it.After installing JLink software search for **SEGGER J-Link Remote Server** and start the application. On first configuration windows select parameters as in the picture:  

<p align="center">
  <img src="pics/jlink_server_1.png">
</p>

After configuration server has started and ip address in my case is as shown on the picture. To be able to see what is happening with the server enable the **show log** configuration for easier tracking.  

<p align="center">
  <img src="pics/jlink_server_2.png">
</p>

## client configuration
On the client side main deevelopment maschine is using Ubuntu 18.04 LTS OS. This will alow us to easily create scripts that will help us to achive firmware download over LAN.
First it is nessesery to install [Segger J-Link software package](https://www.segger.com/downloads/jlink/). After that connection with the server need to be astablised so the firmware can be downloaded. To do that you can call JLink software from the command line with IP address of server maschine as parameter, for example:
```bash
$ JLinkExe ip 192.168.13.229
```
This will get you a connected command line into the remote terminal. You can then start configuration manualy in the Jlink terminal by typing `connect` or `help` for more information. But the goal is to to this in one "click".
This is why we will create script called **flash_remote.sh** with all the commands you would manualy type to program MCU on the server side. 

```
si SWD
Speed 4000
device nrf52811_xxaa
connect
loadbin _build/nrf52811_xxaa.bin 0x00019000
r
exit
```
now we can call JLink software with next parameters:
```bash
$ JLinkExe ip 192.168.13.229 flash_remote.sh
```

and the programm will be downloaded to the MCU.

## References:
1. https://iosoft.blog/category/nordic-nrf52/
2. http://wiki.hivetool.org/NRF52840_Development
3. https://docs.electronut.in/papyr/programming_guide/#upload-hex-file
4. https://aurabindo.in/nordic-and-open-source-goodness/
5. https://diyiot.wordpress.com/2015/11/29/ble-application-with-nrf51822-firmware-flashing/
6. JLinkExe documentation "?"
