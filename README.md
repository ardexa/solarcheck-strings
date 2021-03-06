# Purpose
Phoenix Contact supply solar string monitors. The purpose of this project is to collect data from Phoenix Contact SOLARCHECK string monitors and send the data to your cloud using Ardexa. Data from solarcheck string monitors is read using an Ethernet connection using Modbus RTU (over Ethernet) and a Linux device such as a Raspberry Pi, or an X86 intel powered computer. 

## How does it work
This application is written in Python. This application will query 1 or more connected inverters at regular intervals. Data will be written to log files on disk in a directory specified by the user. Usage and command line parameters are shown below.

## Install
On a raspberry Pi, or other Linux machines (arm, intel, mips or whetever), make sure Python is installed (which it should be). Then install the package as follows:
```
sudo pip install solarcheck_ardexa
```

You also need to install the `modpoll` tool
```
cd
mkdir modpoll
cd modpoll
wget http://www.modbusdriver.com/downloads/modpoll.3.4.zip
unzip modpoll.3.4.zip 
cd linux/
chmod 755 modpoll 
sudo cp modpoll /usr/local/bin
modpoll -h
```

## Usage
- This script will query a single Phoenix Contact SOLARCHECK string monitor.
- To discover RS485 address range and print out the inverter metadata
```
Usage: solarcheck_ardexa discover ip_address port rs485_addresses strings
Eg; solarcheck_ardexa discover 192.168.1.2 502 10-20 3
```

Send production data to a file on disk 
```
Usage: solarcheck_ardexa ip_address port rs485_addresses log_directory strings
Eg; solarcheck_ardexa get 192.168.1.2 502 14-21 /opt/ardexa 8
```
{IP Address} = IP address of the string combiner, eg; 192.168.1.2
{Port} = Modbus port, eg; 502
{RS485 Address(es)} = RS485 address or range of addresses, eg; 1-5 or 1,2,3 or 4 (for example)
{log directory} = logging directory, eg; /opt/ardexa/
{number of strings to query} = 3  (can query up to 8 strings)
To view debug output, increase the verbosity using the `-v` flag.
- standard (no messages, except errors), `-v` (discovery messages) or `-vv` (all messages)


## Collecting to the Ardexa cloud
Collecting to the Ardexa cloud is free for up to 3 x Raspberry Pis (or equivalent). Ardexa provides free agents for ARM, Intel x86 and MIPS based processors. To collect the data to the Ardexa cloud do the following:
- Create a `RUN` scenario to schedule the Ardexa PVS 800 script to run at regular intervals (say every 300 seconds/5 minutes).
- Then use a `CAPTURE` scenario to collect the csv (comma separated) data from the filename (say) `/opt/ardexa/logs/`. This file contains a header entry (as the first line) that describes the CSV elements of the file.

## Help
Contact Ardexa at support@ardexa.com, and we'll do our best efforts to help.
