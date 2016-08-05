# EPD_json - program for displaying .json data on a 2,7" EPD

Program displays 1, 2, 3 or 4 values together with a label, respective sensor connection status and a timestamp on a 2.7" E-Paper module with EM027BS013 display connected to a Raspberry Pi 3 B.

Table of contents

* [Configuration](#configuration-of-raspberry-pi)
* [Installation](#installation)
* [Operation](#operation)
* [SW Architecture](#sw-architecture)
* [Input files formatting](#input-files-formatting)
* [Uninstalling and updating](#uninstalling-and-updating)

## Configuration of Raspberry Pi

In order to be able to install this software, the SPI communication has to be enabled. It is done by entering the configuration menu with command 

	$ sudo raspi-config

than navigating to Advanced Options and enabling SPI there. Afterwards the system has to be rebooted.



## Installation

**Steps included in this paragraph can be carried out by executing bash script install_EPD_json.sh.**

Before running these programs a third party driver for EPD as well as Python PIL library has to be installed (see "Drivers + python scripts on GitHub" on https://www.embeddedartists.com/products/displays/lcd_27_epaper.php).
Both can be done by running the bash script install_epd_driver.sh. 

__NOTE__: EPD driver files are incuded in this repositury. However, this clone of driver repository can be an obsolete, yet working version. It is provided in this way because of some changes introduced in the cource file epd_v2.c for the purpose of this project. The changes speed up the refresh sequence of the EPD. If the user wishes to update the driver he can do the following three steps BEFORE running any other script:

-	delete the folder /gratis/ from directory ../ea-epd-sensor-values
-	execute the following command in the directory ../ea-epd-sensor-values to clone the newest verion of driver repository


	$ sudo git clone https://github.com/embeddedartists/gratis.git
	
	
-	replace the file ../gratis/PlatformWithOS/driver-common/epd_v2.c with the version from the directory
	../ea-epd-sensor-values/epd_v2.c 
	(user can also instead make respective changes in the original file epd_v2.c; compare the commented part in 	the modified file)
   
After running the script install_epd_driver.sh the directory ../gratis can be deleted. Furthermore python package ea_epd_sensor_values must be installed. It is done by executing

	$ python setup.py install
	
in the directory ../ea-epd-sensor-values.



## Operation
					
In order to enable the use of EPD_json script from anywhere within the system, an executable script EPD_json from ea-epd-sensor-values/bin/ directory is automatically copied to root/bin directory. Executing the command

	$ EPD_json [arg]
	
	where arg is
	
	- path/to/.json - displays .json data
	- testN, N={1, 2, 3, 4} - display a test data .json file with N records
	- biba - displays BIBA logo
	- bibaqr  - displays BIBA QR code
	- ip - displays the IP and MAC addresses for eth0 and wlan0 network connections
	- without any argument the script clears the EPD
	
will display data or clear the EPD.	

The EPD can also be operated via python script EPD_json.py from Bash shell with
	
	$python path/to/EPD_json.py [arg]
		 
or from another python script using the installed package  ea_epd_sensor_values with

	>>> import ea_epd_sensor_values
	>>> ea_epd_sensor_values.main([ "arg" ])
	


## SW Architecture

The EPD_json.py loads data contained in a .json file and calls a subprogram EPD_display_values displaying N:(1-4) values with labels and information about communication status with respective sensor (in form of a warning sign if a problem occurs), on the EPD.
The path to .json file is passed to EPD_display_json.py program as an argument.
Alternatively the program calls a subprogram EPD_display_img to display BIBA logo or QR code.
For cleaning the EPD no subprogram is called.



## Input files formatting

The .json files in the directory /json_dummies/ shall serve as templates for user-defined input files. 



## Uninstalling and updating

In order to fully uninstall the EPD software first the device needs to be unmounted from the system. Afterwards the python package and the bash executable script has to be removed. All this is done by executing the script __remove_EPD_json.sh__.
To update the software source files need to be updated and again installed with the script __install_EPD_json.sh__. 