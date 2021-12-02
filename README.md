# MatchGame
Simple Match Game
# Prior Pay Adjustment Process

## Table of Contents
* [General Info](#general-info)
* [How It Works](##how-it-works)
* [Configuration](#configuration)
   * [Compile](#compile)
   * [Compilation Instructions](#compilation-instructions)
* [Installation](#installation)
   * [Installation Instructions](#installation-instructions)
   * [Service Configuration Instructions](#service-configuration-instructions)
* [Task Scheduler](#task-scheduler)
* [Browser Drivers](#browser-drivers)


## General Info
This project is aimed at reducing the number of hours needed by
payroll entering payroll adjustments in TEAM and making sure
employees are paid faster

## How It Works
------------
- The End User Perspective
  The end users will place the Journal Entry files (xlsm) into their
  appropriate folders (identified in the INI file) and the application
  will automatically ingest and file them in the appropriate folders
  in Image Now (identified in the INI file).

- The Application
  The application actively monitors all folders identified in the INI
  file. When a file matching the identified file extension is
  identified an event is triggered and the ingestion begins

  The application then extracts all necessary metadata from the file
  and uploads the file to the appropriate ImageNow folder via the
  integration server API.

  Once complete, the file is moved out of the ingestion folder into a
  completed folder and an event file is saved in the identified events
  repository.

  A report process is run daily that picks up the event files and writes
  all pertinent data to the report file for that facility.

# Configuration
## Compile
This process is written in the Python scripting language. For deployment
_priorpayprocess.py_ is compiled into an EXE file using "pyinstaller" and installed to
the desired server as a Windows service so that it can run without
dependencies as well as be monitored by some of our enterprise monitoring
tools.

### Compilation Instructions 
1. Go to the folder that contains the file _priorpayprocess.py_
2. In the folder Hold Shift and right click on the mouse
3. Select **Open command window here**
4. Type the following into the command line: 
   ```sh
   pyinstaller -F --clean priorpayprocess.py
   ```
5. Press **Enter**
6. A new file should be visable, **priorpayprocess.exe**

## Installation
Installing the service is an integral part of how this process is designed
to function. Follow the below instructions to install and configure the
service properly

#### Installation Instructions
1. Locate the priorpayprocess.exe file in the dist folder of your
     processes root directory
2. Copy the priorpayprocess.exe file to the server (PHXPVAPP255) you wish to installed
     the service on
3. From the server directory that contains the priorpayprocess.exe file,
     type "priorpayprocess.exe install" into the command window.
4. Update the path to the desired ini file by using the following command
     line script:
   - sc config iapf-jeupload binPath="<path/to/priorpayprocess.exe> --iniFile <path/to/iniFile.ini>"
5. Confirm the user credentials on the service by looking at the
     properties of the service, clicking the "Log On" tab, and confirming
     that the service is being run as the appropriate account.

#### Service Configuration Instructions
1. Confirm that the account running the service has -rwxm rights to all
     file paths that are configured in the INI file.
2. Confirm that the service is set to Automatic (delayed start) to
     insure that it will start back up automatically post reboot.

## Task Scheduler
1.	On the desired server, click the start button and search for "Task Scheduler"
2.	Select the correct option in the results to open the Task Scheduler
3.	On the right, click "Create Basic Task"
4.	Create a Name for your task "PriorPay Adjustment", and provide a meaningful description
5.	Click "Next"
6.	Under the "Trigger" section choose the "Weekly" option and click "Next"
7.	Choose the days you wish the program to run
8.	Select the start date (today) and start time of 7:00 PM
9.	Choose to Recur every "1" week
10. Click "Next"
11. Under the "Action" section choose "Start a Program" click "Next"
12. In the "Program/Script" box, type the location to the path of the priorpayprocess.exe file
13. In "Add arguments" type "--iniFile <path/to/prod ini file>"
14. Click "Next" button


## Browser Drivers
Drivers are needed for this process. If the browser updates you will need to download the latest driver based on the Web Browser version number.
   1. [Google Chrome](https://chromedriver.chromium.org/)
   2. [FireFox](https://github.com/mozilla/geckodriver/releases)





## Modules Required to Develop
termcolor==1.1.0
pandas==1.1.0
Babel==2.8.0
requests==2.24.0
tkcalendar==1.6.1
pytest==6.0.1
pykeepass==3.2.1
selenium==3.141.0
beautifulsoup4==4.9.1
dataclasses==0.7


## Troubleshooting
Sometimes there is an issue with the ADODBAPI
one of the symptoms is an exception when processing ANY file
"TypeError: 'NoneType' object is not iterable"

A solution was to install the 64 bit ADODBAPI engine with /passive
1 - download file "AccessDatabaseEngine_X64.exe" from https://www.microsoft.com/en-us/download/details.aspx?id=13255
2 - Run from command prompt
      AccessDatabaseEngine_X64.exe /passive








