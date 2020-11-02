# Guide For Setup of AIP
Note: This tool is meant to be used in conjunction with at least one honeypot from which you are gathering your data from. The data input cannot be simply data collected from a normal network, since the tool does not differentiate between attacks and regular connections. It currently treats every IP in the input data file as an attacker, although a web crawler filtration module is in the works.

In this document, I will describe how to use the AIP algorithm tool. Since it is written in python, it is quite simple to set up for your system.
## Requirements
1. A Linux based OS. I have used it on Ubuntu, Kali, Mint, PopOS, and Elementary.
2. Python 3.7+….. I have never tested it on an older version, so I am not sure. If you test it on an older version and it works, please let me know.
3. Python modules: csv, operator, time, datetime, os, inspect, netaddr and shutil (most of these are default so it should not be a problem).
4. An input data file collected from your honeypots in a format described below.
## First time Setup
1. Download or clone the repo to a directory of your choosing. It contains all necessary files and folders
2. cd /the/location/of/the/AIP_repo
3. chmod u+x manual_run.sh
4. To run AIP, execute ./manual_run.sh
5. Follow the onscreen instructions. Depending on whether this is the first time you have run it or not, it will ask you for the location of your input data files, and then the location that you want the output to be saved to.
6. cd /the/location/you/specified/for/the/output/Files
7. There you will see a number of folders and files. These are the files AIP uses to remember past runs.
8. Go to the directory /Historical_Ratings/ to see the output blacklists.
## Automatic Run Setup
After following the above instruction for a first time setup and run, you can schedule AIP to run automatically using the automatic_run.sh script. This is only useful if you have new input data flows every day. If you do, you can schedule AIP to run every day and automatically generate new Blacklists at a certain time using crontab or nohup.
1. Edit the automatic_run.sh file: nano automatic_run.sh
2. Put in line 4 the FULL path to the OUTPUT folder you specified in the first time run
3. Put in line 7 the FULL path to the INPUT folder you specified in the first time run
4. Save the file and exit
5. To schedule the script using crontab, do the following:
6. crontab -e
7. Add a line at the bottom of the file using crontab syntax. This line will schedule the script at 1am every day for example: 1 * * * * sh /path/to/automatic_run.sh
8. Save and exit


## The Input Data
In terms of file format for the input data, the program accepts a .csv file that has one IP per line, with each of the following data inputs for each IP on that line, separated by commas:
1. The IP address
2. Number of events - Meaning the total connections to your honeypots originating from the given IP
3. Total Duration - How long did this IP connect for the total of its events
4. Average duration - The average length in seconds of all the connections by this IP
5. Amount of Bytes - Total bytes sent and received by this IP
6. Average number of bytes - For bytes transferred in each connection by this IP
7. Total packets - Of all the connections by this IP
8. Average packets - Average packets sent per connection by this IP
9. Last event time - UNIX time of the last time this IP tried to connect to something in the last 24 hours
10. First event time - UNIX time of the first time the IP tried to connect in the last 24 hour

For example, a single line in the file could look like this:
"IPv4-Address", ”26049”, "7415310", "284.6", "41808957", "1605.0", ”284577”, "10.92", "157899154", "1578968762.519"

## Useful Information
The tool is designed to be run once a day at a time of your choosing after copying the .csv file that contains the data from the last 24 hours to the Input-Data directory. If it is not run once a day, the rating system will be thrown off, but it will still work.
The Run-AIP script is designed to simply run the python script for AIP, and then copy the generated blacklist files to another location to be saved.
