+++
date = '2026-03-21'
lastmod = '2026-03-21'
draft = true
title = 'ESP32 Flipper Zero: BLE Spamming Attack'
tags = ['ESP32', 'BLE', 'Security']
+++

### Intro
The reason why I started this project, is because I have a bunch of unused single board computers lying around from school. Which, I am not currently using.   
I thought it was kind of a waste to not use them and thought, "Hey, let's see what I can recreate with these boards."

So, I decided to start with the ESP32 I have lying around, and see if I could re-create the BLE spamming attack function from the Flipper Zero.

The specific hardware I have is a Joy-IT SBC-NodeMCU-ESP32-C development board.

When starting off, I decided to challenge myself by using no_std Rust. This means I am writing bare-metal Rust instead of the more common C on top of the ESP-IDF framework which runs on top of FreeRTOS. This means I have direct access to the hardware using esp-hal which acts as a translation layer between rust code and the espressif hardware.

### Setup
I first installed my dependencies. I need `espflash` to be able to flash the code onto the ESP32.   
I also installed `esp-generate` to have an easy starter template I can build upon.   
Lastly I installed `espup` to manage the toolchain and set up the environment for the ESP32. (This is basically a variant of rustup but for the ESP32)

(Yes, I use arch btw)
```bash
paru -S espflash esp-generate espup
```

After installing everything I need with `espup install` and setting it up in my environment.   

I could finally set up the project using `esp-generate`.
```bash
esp-generate --chip esp32 esp32-flipper_zero
```

![esp-generate](images/esp-generate-1.png)

Here I enabled unstable HAL feature, to be able to talk to the `esp-radio` crate. This allows me to control the Bluetooth directly.   
According to the [docs](https://docs.espressif.com/projects/rust/esp-radio/0.17.0/esp32/esp_radio/index.html) it is also recommended to enable `esp-alloc`, which I also did. This allows me to use the Heap (dynamic memory) which is required for Bluetooth.   
Finally I enabled [bleps](https://github.com/bjoernQ/bleps) as my bluetooth stack. The reason why I chose bleps over [TrouBLE](https://github.com/embassy-rs/trouble) is because I do not need async for this project and I want to keep it as simple as possible. (Even though I chose bare-metal rust... )

Then under the Rust toolchain I chose to use the `esp` toolchain. This is important for Rust to be able to build and run for the ESP32 using the Espressif Rust compiler (which I downloaded via `espup`) instead of the default one. Since the default Rust compiler does not support the Xtensa CPU architecture of the ESP32.

![esp-generate-rust-toolchain](images/esp-generate-2.png)

Finally, I can generate the project and start coding!

### The Code
