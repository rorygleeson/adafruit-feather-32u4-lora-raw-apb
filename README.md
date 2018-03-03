"# adafruit-feather-32u4-lora-raw-apb" 


my notes on getting it to work with a single channel (dragino)  LoRa gateway. Should work with any single channel gateway. 

1) Set up power and antenna , follow instructions here. https://learn.adafruit.com/adafruit-feather-32u4-radio-with-lora-radio-module/

2) Connect 2 pins on the board together. 
Connect Pins (labled on both front and back) 6 and IO1
(IO1 is at the very corner of the chip, oppisite end to the usb and battery ports. 6 is the oppiste side, 4 positions up from bottom). 

2) Then follow instructions here https://www.thethingsnetwork.org/labs/story/creating-a-ttn-node

2) After the new libraries are installed, there should be example programs for the LMIC-Arduino, find the one called ttn-abp (file->examples->LMIC-Arduino->ttn-abp.   Make a new project with a copy of ttn-abp

3) Modify this file, change pin mappings to..

// Pin mapping
const lmic_pinmap lmic_pins = {
    .nss = 8,
    .rxtx = LMIC_UNUSED_PIN,
    .rst = 4,
    .dio = {7,6,LMIC_UNUSED_PIN},
};

4) Update the following values to match the things network device that you created previously ..

- LoRaWAN NwkSKey, network session key
- LoRaWAN AppSKey, application session key
- LoRaWAN end-device address 


5) If in Australia, change frequencies. I just overwite the frequency values in the ttn-abp file, and changed all to 915000000. Need to tidy this up, its currently trying 0-8 (9) different frequencies, but our gateway only supports one... but it will work for a quick demo, just change  all LMIC_setupChannel freq to 915000000.


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
