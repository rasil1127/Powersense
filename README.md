# Powersense

## Introduction
PowerSense is designed for personal light electric vehicles, aimed at optimizing battery usage through intelligent switching. This addresses the common challenge of limited battery life in electric vehicles by efficiently managing power distribution between multiple batteries. The system not only prolongs the range of each journey but also contributes to the longevity of the batteries themselves. This repository provides detailed information on the device's design, functionality, and instructions for implementation. It serves as a valuable resource for enthusiasts and professionals interested in enhancing electric vehicle performance through advanced battery management.
<sub>Abstract written by [lekrsu](https://github.com/lekrsu)</sub>
### Advantages
- High voltage & current support
- Utilises N-channel mosfets for efficient low-side switching
- Open-source hardware
- Can intelligently switch between 2 connected batteries to bring the voltage difference to within ±1V then connect them in parallel
- Batteries with different chemistries, voltage levels, and cell counts can be connected.
### About Powersense
It has many easily accessible communication ports on the back of the board which follow this pinout: 

![ABTPWRSNE](https://github.com/rasil1127/Powersense/blob/main/Pinout.png)

The reference design is as follows: 

![ref](https://github.com/rasil1127/Powersense/blob/main/ref_top.jpg)

![ref](https://github.com/rasil1127/Powersense/blob/main/ref_bottom.jpg)

> [!Tip]
> The exposed traces connecting the mosfets must be reinforced with solid core copper wire.

> [!WARNING]
> Fuses labelled (F3,F1,F2) must be populated with the appropriate fuse for your use-case leaving these unpopulated may cause damage. 

## competition

### Rita
#### What is Rita?
> Rita analyzes voltages of the internal and external batteries, switching their positive power leads so that the charge does not flow from one battery to another, keeping the ability for the controller to consume current from both batteries and recover energy back when braking.

Source [rita_manual_gen6_en.pdf](https://public.embedden.com/Rita/Docs/English/rita_manual_latest_en.pdf).
#### What does it do in a nutshell?
> [!NOTE]
> I have not analysed a Rita module to see its working principles this information may not be 100% correct.

It will switch to whatever battery has a higher voltage and drain that first. It reads both batteries' voltages to determine which one has a higher voltage potential, the same as what Powersense does. However, it is using P-channel mosfets as explained above by the original creator "*switching their positive power leads*" It cannot be N-channel mosfets since those would require the positives to be common and negatives to be switched. It also emulates BMSes for the scooter lineup by NINEBOT and XIAOMI.

#### Advantages and disadvantages
![RitaAD](https://github.com/rasil1127/Powersense/blob/main/rita_ad.png)

### Aliexpress battery switchers
#### What do they do?

They have a small integrated IC internally that uses P-channel mosfets like Rita to switch between batteries positive with the ground being common. this approach is not that great since P-channel mosfets are inefficient since they usually have a higher RDS<sub>on</sub> resistance.

#### Advantages and disadvantages
![AlieAD](https://github.com/rasil1127/Powersense/blob/main/alie_ad.png)

## Specification
With the traces reinforced the Powersense device is rated for up to **40A**.

It is also rated for a maximum voltage of **134V**.

Physical switching turn on delay is **30ns** and the turn off delay is **48ns** 

Maximum and minimum operating tempratures include **-20°C to 100°C**

> [!CAUTION]
> All of the tests above were done at 40A, VGS=10V, VDS=30V unless mentioned otherwise.
> The current limit and voltage limit have not been tested as of yet it will be updated in the future.

## Firmware details
The firmware will not be open-sourced with the Powersense device. **However the disclaimer still applies**. I will make it as easy as possible for anyone to create a custom firmware for example there exists an Arduino-like API for the STM8 family of microprocessors such as [Sduino](https://tenbaht.github.io/sduino/) which will be easy to pick up and work with. however, the STM8 can be programmed via ST visual develop and COSMIC C compiler.

### pin definitions

![Pindef](https://github.com/rasil1127/Powersense/blob/main/pin_def.png)

> [!NOTE]
> The analogue inputs of the STM8 (A2, A1) are multiplexed between STM8 physical pin 20, 19 respectively. 

### Voltage accusation calculations

#### A/D calculations
```
(AIN / 1024) * 5
```

**AIN** is the multiplexed analogue input and it is divided by **1024** which is the internal 10-bit ADC resolution. it is multiplied by **5** which is the supply voltage of the IC. 

#### Resistor divider divisions
```
537.3134328358209
```

This number is how much the original voltage is divided by going through the on-board resistor divider thus in software the acquired voltage must be multiplied by the same number to get the original voltage. 

#### AMC1200 voltage mapping calculation
```
0 + (0.251 - 0) * (AIN - 2.55) / (3.59 - 2.55)
```

**0.251** is the maximum value in volts that going to be outputted from the voltage divider.

**AIN** is the multiplexed analogue input

**2.55** is the minimum voltage that the AMC will output this can be seen on the datasheet for the IC.

**3.95** is the maximum voltage that the AMC will output this can be seen on the datasheet for the IC.

## BOM
![BOM](https://github.com/rasil1127/Powersense/blob/main/bom.png)

LCSC numbers can be used on LCSC.com to purchase these components.
> [!NOTE]
> WX-DC12003 buck converter can be purchased from any other retailer such as aliexpress.com


> [!IMPORTANT]
> PowerSense is designed to help you parallel multiple batteries in your battery-powered equipment. However, PowerSense comes as-is and does not come with any warranty or support and the creator is not responsible for any damages or losses that may occur as a result of using PowerSense. You are using PowerSense at your own risk and discretion. It is intended to be used appropriately and safely, following the instructions and specifications provided by the creator. If you use PowerSense in an improper or unsafe manner, such as exceeding the voltage or current limits, modifying the firmware or hardware, or exposing PowerSense to extreme temperatures or moisture, you may cause fires, explosions, injuries, or property damage. The creator holds no liability for any of these consequences and you are solely responsible for any claims or costs that may arise from them. The parameters shown by the creator are not guaranteed to be error-free or flawless. PowerSense may malfunction or fail due to various factors, such as manufacturing defects, software bugs, or external interference. The creator is not obligated to provide any updates, fixes, or replacements for PowerSense and you are advised to use PowerSense with caution and care. **By using PowerSense, you agree to this disclaimer. If you do not agree to this disclaimer, do not use PowerSense.**
