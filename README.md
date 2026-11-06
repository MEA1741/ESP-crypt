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
Master Key Slot: One dedicated slot inside the ATECC608B holds the primary encryption key used to encrypt/decrypt the password vault stored on the SD card. Here is the datasheet for more info:

https://ww1.microchip.com/downloads/aemDocuments/documents/SCBU/ProductDocuments/DataSheets/ATECC608B-CryptoAuthentication-Device-Summary-Data-Sheet-DS40002239B.pdf


## Hardware Branches
   The project is built around two primary targets:

 ### M5STACK CoreS3 Branch (Prototype & Daily Driver):
  Built for quick testing and portable standalone usage.
  Utilizes CoreS3's built-in touchscreen, battery management, and microSD card slot. Here is official documentation for more info:
  
https://docs.m5stack.com/en/core/CoreS3

 ### ESP32-S3 DevKit Branch (Generic & DIY):
  Generic implementation for standard ESP32-S3 breakout boards and custom wiring setups.

## Connection Diagram (I²C)
  The ATECC608B connects to the main MCU via standard I²C (Port A on M5Stack CoreS3 / GPIOs on ESP32-S3 DevKit).
  ```mermaid
flowchart LR
    subgraph Host["Host Workstation (PC / Laptop)"]
        style Host fill:#1e293b,stroke:#3b82f6,stroke-width:2px,color:#fff
        A["Raw Credentials / Auth Request"]
        style A fill:#0f172a,stroke:#475569,color:#fff
    end

    subgraph ESP["ESP-crypt (M5Stack CoreS3 / ESP32-S3)"]
        style ESP fill:#7c2d12,stroke:#f97316,stroke-width:2px,color:#fff
        B["USB-C Interface (HID / WebAuthn)"]
        C["Password Manager Core & UI"]
        D["Encrypted SD Card Vault"]
        style B fill:#18181b,stroke:#52525b,color:#fff
        style C fill:#18181b,stroke:#52525b,color:#fff
        style D fill:#18181b,stroke:#52525b,color:#fff
        
        B <--> C
        C <--> D
    end

    subgraph SecureElement["Secure Element"]
        style SecureElement fill:#064e3b,stroke:#10b981,stroke-width:2px,color:#fff
        E["ATECC608B (Hardware Slot)"]
        F["Hardware Master Key Storage"]
        style E fill:#022c22,stroke:#059669,color:#fff
        style F fill:#022c22,stroke:#059669,color:#fff
        
        E <--> F
    end

    A <== "USB Connection" ==> B
    C <== "I2C Protocol (SDA/SCL)" ==> E

