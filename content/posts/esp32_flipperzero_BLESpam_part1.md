+++
date = '2026-03-21T14:16:06+01:00'
draft = true
title = 'ESP32 Flipper Zero: BLE Spamming Attack'
tags = ['ESP32', 'BLE', 'Security']
+++

### Intro
The reason why I started this project, is because I have a bunch of unused single board computers lying around from school. Which, I am not currently using.   
I thought it was kind of a waste to not use them and thought, "Hey, let's see what I can recreate with these boards."

So, I decided to start with the ESP32 I have lying around, and see if I could re-create the BLE spamming attack function from the Flipper Zero.

The specific hardware I have is a Joy-IT SBC-NodeMCU-ESP32-C development board.

When starting off, I decided to challenge myself by using no_std Rust. This means I am writing bare-metal Rust instead of the more common Arduino or C++ environments.

### Setup
I first installed my dependencies. I need `espflash` to be able to flash the code onto the ESP32.   
I also installed `esp-generate` to have an easy starter template I can build upon.   
Lastly I installed `espup` to manage the toolchain and set up the environment for the ESP32. (This is basically a variant of rustup but for the ESP32)

(Yes, I use arch btw)
```bash
paru -S espflash esp-generate espup
```
