# ESP32 Digital RGB(W) LED Drivers

A library for driving self-timed digital RGB/RGBW LEDs (WS2812, SK6812, NeoPixel, WS2813, etc.) using the Espressif ESP32 microcontroller's RMT output peripheral

Based upon the [ESP32 WS2812 driver work by Chris Osborn](https://github.com/FozzTexx/ws2812-demo)

<hr>

### Notes

The RMT peripheral of the ESP32 is used for controlling up to 8 LED "strands" (in whatever form factor the serially-chained LEDs are placed). These strands are independently controlled and buffered.

There are working demos for Espressif's IoT Development Framework (esp-idf) and Arduino-ESP32 core. Some demos are ONLY for the ESP IDF (demonstrating C-only techniques). Otherwise, a given demo should be exactly the same on either framework.

This library currently works well with WS2812-type and NeoPixel RGB LEDs (3 bytes of data per LED) - SK6812 RGB LEDs should work equally well. This also works fine with WS2813 (the backup channel is not used currently - tie `Backup In` to `Data In` for now).

This also works well with SK6812 RGBW LEDs (4 bytes of data per LED). These are similar to the WS2812 LEDs, but with a white LED present as well - keep in mind that RGBW LEDs draw a fair amount of extra power versus the usual RGB LEDs due to the added white LED element present.

Timings for a given LED type can vary even within that line: use the version variant that works best for you (see `ledParamsAll` for details).

<hr>

### ESP-IDF build notes - Important!

## The ESP-IDF demos are WIP right now (not currently buildable) and may be that way indefinitely

The ESP-IDF varies so regularly and so significantly it's more difficult to keep up with it, versus the abstracted Arduino-ESP32 interfaces. As time permits, I'm working on making it buildable with v3.2.2 and later sub-versions but it's slow going.

While the Arduino-ESP32 side is my main focus, the ESP-IDF build ought to be very similar (demo*.ino --> main.cpp, and the header and source libs aren't in their canonical locations by default). Beyond convenience, there was no real value in maintaining duplicate copies of the code and libraries as I did previously so those duplicates were removed. Also, symlinks in the repo are specifically disallowed in Arduino so that's not happening.

Please see the `sdkconfig.defaults` file for details of the defaults I use. If you run `make menuconfig` or `make sdkconfig` this file will be parsed for initial settings ONLY if `sdkconfig` doesn't exist. However, this file will be processed every time you run `make defconfig`.

<hr>

### TODO

- API (current):
  - init --> initStrands (all strands)
  - setColors --> update --> updatePixels (per strand)
  - (n/a) --> reset --> resetPixels (per strand)

- Future additional (class-driven):
  - (n/a) --> updateTimings (for fine-tuning, debugging, or marginal/odd devices)
  - (n/a) --> updateType (for fine-tuning, debugging, or marginal/odd devices)
  - (n/a) --> addStrand/deleteStrand? (maybe not...)

- Other:
  - Make the whole library a proper class - more robust and extensible!
  - ledParams - refine and clarify
  - ledParams - lower bounds? Currently bit timing constants are pre-padded +50ns
  - ledParams - add a small constant string as a mapping to the name?
    - Nope! C99 designator 'name' outside aggregate initializer
  - ledParams - instead of bytesPerPixel, specify color format (GRBW, RGB, etc.)?
    - Would have the same problem as naming...
  - WS2813 - handle backup channel
  - Resolve open TODOs in code
  - Add more interleaved demos, and more demos in general
  - Make Arduino side a true Arduino library? May not be practical.
  - APA102/DotStar support?
