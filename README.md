* Title: RGB Simple Communication
* Author: Brian Khuu
* Website: briankhuu.com
* Date: 28th August 2016
* Description:
  - The concept is to transmit data via LED in the simplest method possible.
  - The targets situation is a smartphone pointed at an LED.

  - The self imposed challenge is:
    * No external clocking : The issue with having an led dedicated to the
      clock signal is that the state transition between data bits is at least
      twice. It would be better to embed the clocking in the data.
      This was acheived by having the encoding and decoding algorithm
      send every half nibble, encoded as a transition of one colour state to
      another. This makes this communication system stateful, as it relies on
      the previous color state as well current colour to decide what it had
      received.
    * Timing insensitive : The hardware recording the colour changes like
      a smartphone camera should not be relied upon to be timing accurate.
      Also by making the system timing insensitive, it makes
      implementation much much easier even in slow hardware.
    * LED is either fully on or off : By keeping the requirement simple
      it would allow the seqence player to play almost on any hardware with
      at least 3 GPIO pins to have this transmission output method.
      It also makes detection via RGB sensor much easier as well,
      since colour levels detection accuracy is not needed as much.
    * If the red green and blue leds are off, then the communication
      channel is down.

  - What was acheived is a working algorithm for transmission of variable
  length bytes. In this implementation if the output colour is dark, then
  the channel is closed. The channel is online if at least one led is active.
  Every 2bits is represented as a transition between two data colour state.
  This leaves just two more states, yellow and white. These two colour were
  named Mark 1 and Mark 2, this can be customised for any particular purpose.
  (But in the example, it is useds as a character mark and optionally as a
  parity bit)

* Optional Objectives:
  - Would be good to implement a bit of parity checking
  - Package this as a libary for ready use in anything.
  - Write an android program that can visually decode such signals.
  - Or maybe a hardware with colour sensor that can decode such signals.

Application:
  - Honestly there is not too much use for something like this, considering
  that there are already bluetooth.
  - As a slower alternative to UART... really?
  - Debug output? You might not have a UART engine in your mcu, but may have
    at least 3 pins to spare.


This is this table showing the LED colours and the meaning assigned to each state in this algo


  | COLOUR   | R | G | B | Description                                                                |
  |----------|---|---|---|----------------------------------------------------------------------------|
  | DARK     | 0 | 0 | 0 | - Channel Off                                                              |
  | BLUE     | 0 | 0 | 1 | 2bit Data Colour State 0                                                   |
  | GREEN    | 0 | 1 | 0 | 2bit Data Colour State 1                                                   |
  | CYAN     | 0 | 1 | 1 | 2bit Data Colour State 2                                                   |
  | RED      | 1 | 0 | 0 | 2bit Data Colour State 3                                                   |
  | MAGENTA  | 1 | 0 | 1 | 2bit Data Colour State 4                                                   |
  | YELLOW   | 1 | 1 | 0 | - Mark 2 (Mark stderr ?) (Mark&Parity Bit 0 ?) (bitmap mode: vsync ?)      |
  | WHITE    | 1 | 1 | 1 | - Mark 1 (Mark stdout ?) (Mark&Parity Bit 1 ?) (bitmap mode: hsync ?)      |
