# BuffaloDAC
Class libraries for controlling the ES9018/28/38 Sabre DACs from an Arduino via I2C (the ES9028 library also works with the ES9038 chip as the control logic is the same)

The libraries were originally written to work in conjunction with the Twisted Pair Audio Buffalo DACs, but will work with any ES9018/28/38 implementation providing access to the chip's I2C interface. The libraries gives access to the majority of the chips capabilities. Memory usage has been optimised, and comprehensive debug/diagnostics are built into the libraries.

the ES9028 library has been tested to work with an ES9028/38 chip transplanted onto a Buffalo III 8-channel DAC board (the chips are pin compatible). Only the firmware needs to be changed). You must remove the onboard firmware chip first of course, and upgrade the trident regulators to the latest SR version or provide an alternative capable of satisfying the increased power requrements of the new chips. 

You must also ensure that the ES9028/38 chip's RESET pin is pulled high to enable it before commencing I2C communications (an optocoupler IC does the job nicely). You can simply use a jumper for initial testing, but beware that the ES9028/38 chip will emit loud noise if it is not configured correctly to match the input digital stream (by default it expects 32bit I2S data - whereas many I2S sources provide 24 bit data). 

Each chip library includes an initialise() function that configures the DAC inputs to one of the standard presets specified via the constructor (stereo, 8-channel, mono left/right). This method must be called before configuring any other settings. The ES9028/38 can subsequently be configured to support any input mapping via the mapInputs() function (the ES9018 chip doesn't support this capability). If the input select mode is not specified in the constructor then it defaults to stereo for the ES9018 and 8-channel for the ES9028/38. (note that input select modes other than stereo and mono will only work on the Buffalo III 8-channel DAC or equivalent boards).

All configuration functions return a boolean indicating whether the change was written successfully to the DAC. This and other diagnostic information can be used to detect and resolve I2C issues (usually caused by too long or bad connections), and DAC startup issues (for example, the TPA trident series regulators take around 1.5 seconds to ramp up to full operating voltage, so I2C communications must be delayed appropriately).

(see the WIRE library for details on connecting an I2C device to an Arduino board. Be aware that most I2C devices, including the Sabre DACs use 3.3 volts! - whereas Arduinos use 5 volts. The Sabre DAC I2S input is supposedly 5 volt-tolerant, but you should use an I2C isolator in any case to prevent noise from the Arduino interfering with the DAC. TIP: Keep your I2C leads relatively short to avoid unreliable communications)
