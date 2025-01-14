# Eight Sleep Integration for Home Assistant
[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg?style=for-the-badge)](https://github.com/hacs/integration)

Home Assistant Eight Sleep integration that works with Eight Sleep's V2 API and OAUTH2

## Installation
### HACS

1. Add this repository to HACS *AS A CUSTOM REPOSITORY*.
2. Search for *Eight Sleep*, and choose install. 
3. Reboot Home Assistant and configure from the "Add Integration" flow.

NOTE: Ensure neither side is in away mode when setting up the Eight Sleep integration.

## Prerequisites ##
### Authentication ###
 

To get the OAuth2 login to work you need:
- user email
- user password

UPDATE: You now only need your user name and password for Eight Sleep's current OAuth2 implementation. If you don't have your client_id and/or client_secret, leave those values blank when setting up the integration.
<strike>

- client_id
- client_secret


To get the client_id and client_secret you can setup a packet capture and a mitm CA to get the unencrypted traffic from your app. You can also decompile the APK to get the values.

The process I used was:
- Open pcapdroid and install PCAPDroid mitm
- download and install rootAVD https://github.com/newbit1/video-files/blob/master/rootAVD_Windows.gif 
  - 2 ways to root an AVD (android studio); Magisk (rootAVD) and SuperSU
- Should auto install magisk
- Run the avd root install steps then open a cmd in ..\rootavd\rootAVD
- Install the mitm cert (mitmproxy-ca-cert.cert)
- https://emanuele-f.github.io/PCAPdroid/tls_decryption the MagiskTrustUserCerts module, and then install the hashed certificate (replace mitmproxy-ca-cert.cer with the PCAPdroid certificate name) as a system certificate. 	
- Run the app and capture date in pcapdroid. 
  - Make sure you capture the data during an app login session. The data should be in the POST request from the app to auth-api.8slp.net.~~</strike>

## Usage ##
The integration will function similarly to the previous Home Assistant core Eight Sleep integration. It will import the Eight Sleep bed sides and account as devices.
<br>Setting the temperature on the bed is between a -100 to 100 range. This range is unit-less in the API.

There are a few services you can use on the <..>_bed_temperature entities:
- **Heat Set**
  - Sets heating/cooling level for a <..>_bed_temperature entity.
- **Heat Increment**
  - Increases/decreases the current heat level for a <..>_bed_temperature entity.
- **Side Off**
  - Turns off 8 sleep side. Input entity must be a <..>_bed_temperature entity.
- **Side On**
  - Turns on 8 sleep side in smart mode. Input entity must be a <..>_bed_temperature entity.
- **Start Away Mode**
  - Turns on away mode for an 8 sleep side. Input entity must be a <..>_bed_temperature entity.
- **Stop Away Mode**
  - Turns off away mode for an 8 sleep side. Input entity must be a <..>_bed_temperature entity.

  <br>

There are a few possible sensor values for each Eight Sleep side. Some ones with caveats are
- **Bed Presence**
  - There is no direct way to pull this from the API. There is an algorithm that tries to guess presence depending on if the bed temp is changing or not, but I haven't found it to be very accurate.
- **Next Alarm**
  - This will be the datetime value of your next alarm. If you have no alarms set, then it will be set to Unknown.
  
Sensor values are updated every 5 minues

## TODO ##
- Translate "Heat Set" and "Heat Increment" values to temperature values in degrees for easier use.

### Credits ###
Thanks to @mezz64 and @raman325 for developing the previous Eight Sleep integration.

This is also based on work from https://github.com/lukas-clarke/pyEight and I will likely maintain this repo over the aforementioned one.