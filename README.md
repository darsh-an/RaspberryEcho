# AlexaRPi
 
---
## 05/02/2017 I am working on a v1.1 of this incorporating as many fixes and pull requests as possible, look for a new verison in the next week or so.

### Code
 
* Darshan Sarode
 
---
This project helps you to turn you Raspberry Pi into Amazon's Alexa Voice Service.
---
---
Tested on Raspberry Pi 3 model B+
---
 
#### Requirements

You will need:
* A Raspberry Pi
* An SD Card with a fresh install of Raspbian (tested against build 2015-11-21 Jessie)
* An External Speaker with 3.5mm Jack
* A USB Sound Dongle and Microphone
* Raspberry Pi Sence HAT


---
SignUp/Login Into your amazon developer account @
https://developer.amazon.com
ALEXA>ALEXA VOICE SERVICE>REGISTOR AS A PRODUCT(TYPE)[DEVICE]>Device Type ID [NAME]>Display Name[NAME]>SECURITY PROFILE(CREATE-NEW)
>OTHER INFO>HIT "Submit"

##### Installation

Boot your fresh Pi and login to a command prompt as root.

Make sure you are in /root

Clone this repo to the Pi
`git clone https://github.com/darsh-an/AlexaRPi.git`
Run the setup script
`./setup.sh`

Follow instructions....

Enjoy :)

###### Issues/Bugs etc.

If your alexa isn't running on startup you can check /var/log/alexa.log for errrors.

If the error is complaining about alsaaudio you may need to check the name of your soundcard input device, use 
`arecord -L` 
The device name can be set in the settings at the top of main.py 

You may need to adjust the volume and/or input gain for the microphone, you can do this with 
`alsamixer`

####### Advanced Install

For those of you that prefer to install the code manually or tweak things here's a few pointers...

The Amazon AVS credentials are stored in a file called creds.py which is used by auth_web.py and main.py, there is an example with blank values.

The auth_web.py is a simple web server to generate the refresh token via oAuth to the amazon users account, it then appends this to creds.py and displays it on the browser.

main.py is the 'main' alexa client it simply runs on a while True loop waiting for the button to be pressed, it then records audio and when the button is released it posts this to the AVS service using the requests library, When the response comes back it is played back using mpg123 via an os system call, The 1sec.mp3 file is a 1second silent MP3) I found that my soundcard/pi was clipping the beginning of audio files and i was missing the first bit of the response so this is there to pad the audio.

The LED's are a visual indictor of status, I used a duel Red/Green LED but you could also use separate LEDS, Red is connected to GPIO 24 and green to GPIO 25, When recording the RED LED will be lit when the file is being posted and waiting for the response both LED's are lit (or in the case of a dual R?G LED it goes Yellow) and when the response is played only the Green LED is lit. If The client gets an error back from AVS then the Red LED will flash 3 times.

The internet_on() routine is testing the connection to the Amazon auth server as I found that running the script on boot it was failing due to the network not being fully established so this will keep it retrying until it can make contact before getting the auth token.

The auth token is generated from the request_token the auth_token is then stored in a local memcache with and expiry of just under an hour to align with the validity at Amazon, if the function fails to get an access_token from memcache it will then request a new one from Amazon using the refresh token.








---
 

