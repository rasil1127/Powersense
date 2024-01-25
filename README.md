# Powersense

## Introduction
PowerSense enables you to parallel multiple batteries in your battery-powered equipment. It has several advantages over other devices that do the same thing.
- High voltage & current support
- Utilise N-channel mosfets for efficient low-side switching
- Open-source hardware
- Can intelligently switch between 2 connected batteries to bring the voltage difference to within ±1V then connect them in parallel
- Batteries with different chemistries, voltage levels, and cell counts can be connected.

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
![RitaAD](https://media.discordapp.net/attachments/816082953951248444/1199885421094654082/image.png?ex=65c42b61&is=65b1b661&hm=b6f9a830095a822535ad37fae55222e75469591bc772b40bc36fa02cd35ac359&=&format=webp&quality=lossless)

### Aliexpress battery switchers
#### What do they do?

They have a small integrated IC internally that uses P-channel mosfets like Rita to switch between batteries positive with the ground being common. this approach is not that great since P-channel mosfets are much more inefficient since they usually have a higher RDS<sub>on</sub> resistance.

#### Advantages and disadvantages
![AlieAD](https://media.discordapp.net/attachments/816082953951248444/1199885462316273705/image.png?ex=65c42b6b&is=65b1b66b&hm=72bde0050724ae7a2f749754ddbe0c7b1fb10f433b2f315f34eafe8945639199&=&format=webp&quality=lossless)

> [!IMPORTANT]
> PowerSense is designed to help you parallel multiple batteries in your battery-powered equipment. However, PowerSense comes as-is and does not come with any warranty or support and the creator is not responsible for any damages or losses that may occur as a result of using PowerSense. You are using PowerSense at your own risk and discretion. It is intended to be used appropriately and safely, following the instructions and specifications provided by the creator. If you use PowerSense in an improper or unsafe manner, such as exceeding the voltage or current limits, modifying the firmware or hardware, or exposing PowerSense to extreme temperatures or moisture, you may cause fires, explosions, injuries, or property damage. The creator holds no liability for any of these consequences and you are solely responsible for any claims or costs that may arise from them. The parameters shown by the creator are not guaranteed to be error-free or flawless. PowerSense may malfunction or fail due to various factors, such as manufacturing defects, software bugs, or external interference. The creator is not obligated to provide any updates, fixes, or replacements for PowerSense and you are advised to use PowerSense with caution and care. **By using PowerSense, you agree to this disclaimer. If you do not agree to this disclaimer, do not use PowerSense.**
