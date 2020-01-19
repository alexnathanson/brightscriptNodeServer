# A more functional Brightsign API

## overview
This repository combines existing Brightsign APIs and functions to create a more functional and practical very that is much more flexible and easier to implement. This provides a wide range of functionality for syncing content remotely, interacting with the brightsign, and trouble shooting, all without using BrightAuthor! This was designed for the LS423 model, but should be widely applicable to the most recent generation of Brightsign devices.

## Syncing media content to server directory
* continuously checks server directory for changes
  * server directory is /[deviceID]/media
  * [deviceID] is automatically detected and does not need to be changed by the user
* downloads new files
* deletes old files

### media controls and interactivity
* UDP on port 13131
	* playfile syntax: 'file [filename]'
	* volume syntax: 'volume [integer in range 0-100]'
* GPIO
	* Default is set to GPIO 0 which will play a random file
* HTML Control interface on port 8000
	* must add full node_modules directory + a few other modules to SD card
		* run 'npm install' on your computer and drag the node_modules directory to the SD card
		* could be rewritten with webpack in the future
	* uses PUT method 
* Additional commands can be sent via the Chrome diagnostic console.
	* see below

### config file
* can set and get data from config file to ensure persistance

## Trouble shooting and diagnostic functions
* displays IP address on screen
* enables diagnostic console on port 3000 (Chrome browsers only)
This allows you to manually work with the BS_API class directly if accessible end points haven't been established.
	* some frequently used commands:
		* restart = BS.reboot()
		* volume = setVolume(an int from 0-100)
		* play a file = playFile('the filename in quotes')
		* turn the mask zone on/off = maskIt(boolean)
* html control interface on port 8000

## Brightsign APIs as a class
* the BS_API class combines a variety of BS tools with Node modules to create a more complete way to interact with the BS with a standardized syntax

### multiple zones
* multiple zones can be enable via the config file, but automating specific behaviours will need to be determined once the robot interaction is determined.
* presently the additional zone is an image, but it could easily be changed to a video

## HTML Control Interface
The control interface uses a Node Express server (controlInterfaceExpress.js) to server the files in the controlInterface directory.

## instructions
Make sure the firmware is up to date.

### brightsign
* update the firmware on the BS
* update the config file as necessary
* drag all of contents of the BrightsignLS423 directory to the SD card

### media server 
There are 2 main functions of the media server machine 1) distribute media content to the media players and 2) collect IPs from the media players for trouble shooting purposes.
<p>
The server must have a static IP, which must be listed in the BS config file.
</p>
Distribution
* currently intended for a generic IIS setup
* must enable directory browsing
* place media files on server directory named "media" within a directory with the name of the device ID
	* root/[deviceID]/media

IP Collector
* a Node JS app
* set it up to start on boot
* must adjust file paths on final version

## possible future additions
* html control interface
	* playback controls
	* display local file list
* generate list of all IPs on server
* test on other BS hardware (its possible a restart function will need to be added after the node server is enable for the first time on a particular device)
* format file names with pertinant info and parse it to update config file and bay info
* split up functionality so it is easier to manage and reuse in the future
	* create seperate functionality for HTML interface, server syncing, IP collector
	* condense Node module stuff
