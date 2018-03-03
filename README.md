"# adafruit-feather-32u4-lora-raw-apb" 


my notes on getting it to work with a single channel (dragino)  LoRa gateway. Should work with any single channel gateway. 

1) Set up power and antenna , follow instructions here. https://learn.adafruit.com/adafruit-feather-32u4-radio-with-lora-radio-module/

2) Connect Pin 6 and IO1 on the board together. 

Connect Pins (labled on both front and back) 6 and IO1
(IO1 is at the very corner of the chip, oppisite end to the usb and battery ports. 6 is the oppiste side, 4 positions up from bottom). 
why does this need to be done ? see below.

3) Install necessary library..

IBM Arduino-LMIC library, nicely modified for Arduino IDE by Matthijs Kooijman  https://github.com/matthijskooijman/arduino-lmic


Choose ‘Clone or download’

Download ZIP

Mark the location

Go to Arduino IDE

Select ‘Sketch->Include Library->Add .ZIP Library’

Select the downloaded Arduino-lmic-master.zip file from your download location



4) After the new library is installed, there should be example programs for the LMIC-Arduino in the Arduino IDE, find the one called ttn-abp (file->examples->LMIC-Arduino->ttn-abp.   Make a new project with a copy of ttn-abp



5) Modify this file, change pin mappings to..

// Pin mapping
const lmic_pinmap lmic_pins = {
    .nss = 8,
    .rxtx = LMIC_UNUSED_PIN,
    .rst = 4,
    .dio = {7,6,LMIC_UNUSED_PIN},
};

6) Update the following values to match the things network device that you created previously ..

- LoRaWAN NwkSKey, network session key
- LoRaWAN AppSKey, application session key
- LoRaWAN end-device address 


7) If in Australia, change frequencies. I just overwite the frequency values in the ttn-abp file, and changed all to 915000000. Need to tidy this up, its currently trying 0-8 (9) different frequencies, but our gateway only supports one... but it will work for a quick demo, just change  all LMIC_setupChannel freq to 915000000.


    LMIC_setupChannel(0, 915000000, DR_RANGE_MAP(DR_SF12, DR_SF7),  BAND_CENTI);      // g-band
    LMIC_setupChannel(1, 915000000, DR_RANGE_MAP(DR_SF12, DR_SF7B), BAND_CENTI);      // g-band
    LMIC_setupChannel(2, 915000000, DR_RANGE_MAP(DR_SF12, DR_SF7),  BAND_CENTI);      // g-band
    LMIC_setupChannel(3, 915000000, DR_RANGE_MAP(DR_SF12, DR_SF7),  BAND_CENTI);      // g-band
    LMIC_setupChannel(4, 915000000, DR_RANGE_MAP(DR_SF12, DR_SF7),  BAND_CENTI);      // g-band
    LMIC_setupChannel(5, 915000000, DR_RANGE_MAP(DR_SF12, DR_SF7),  BAND_CENTI);      // g-band
    LMIC_setupChannel(6, 915000000, DR_RANGE_MAP(DR_SF12, DR_SF7),  BAND_CENTI);      // g-band
    LMIC_setupChannel(7, 915000000, DR_RANGE_MAP(DR_SF12, DR_SF7),  BAND_CENTI);      // g-band
    LMIC_setupChannel(8, 915000000, DR_RANGE_MAP(DR_FSK,  DR_FSK),  BAND_MILLI);      // g2-band


Thats it, upload and verify data received in the ttn console. 


--------------------------------------------------------------------------------------------



Notes from https://www.thethingsnetwork.org/forum/t/got-adafruit-feather-32u4-lora-radio-to-work-and-here-is-how/6863/44

It seems the board requires the following pin mapping, however IO1 and 6 are not connected, hence why we need to wire it up.

LED - 13 (LED_BUILTIN)

RFM9x/SX127x LoRa module
NSS - 8
MOSI - 16/MOSI
MISO - 14/MISO
SCK - 15/SCK
RST - 4
DIO0 - 7
DIO1 - 6 (Not wired onboard, must be explicitly wired).

OLED display
SDA - 2/SDA
SCL - 3/SCL




