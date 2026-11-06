# ESP-crypt
ESP-crypt is an open-source, zero-trust physical hardware authentication and password manager device. Designed to protect credentials using hardware-grade security, it bridges the gap between commercial security keys (like Yubikeys) and custom embedded hardware.
## Key Features
  Zero-Trust Architecture: Passwords are encrypted on-storage using a master key tied directly to a physical secure element.
  Hardware-Backed Cryptography: Microchip ATECC608B secure element protects key generation and storage against physical and side-channel attacks.
  Dual Target Hardware: Supports both standalone dev kits for DIY builds and integrated modules for portable daily use.
  Encrypted Storage: Full SD card (or dedicated directory) AES encryption protected by a master key stored inside a locked slot of the secure element.
## System Architecture & Security Model
The project utilizes the Microchip ATECC608B-SSHDA-B cryptographic companion element over I²C.
It includes 16 secure key slots, hardware cryptographic accelerators (ECDSA, ECDH, AES-128, SHA-256), and protected storage.
Master Key Slot: One dedicated slot inside the ATECC608B holds the primary encryption key used to encrypt/decrypt the password vault stored on the SD card.

  ATECC608B Datasheet

## Hardware Branches
   The project is built around two primary targets:

 ### M5STACK CoreS3 Branch (Prototype & Daily Driver):
  Built for quick testing and portable standalone usage.
  Utilizes CoreS3's built-in touchscreen, battery management, and microSD card slot.
 M5Stack CoreS3 Documentation

 ### ESP32-S3 DevKit Branch (Generic & DIY):
  Generic implementation for standard ESP32-S3 breakout boards and custom wiring setups.

## Connection Diagram (I²C)
  The ATECC608B connects to the main MCU via standard I²C (Port A on M5Stack CoreS3 / GPIOs on ESP32-S3 DevKit).
