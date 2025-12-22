# Gate Wiring Diagram

## Gate Opener Control Board Terminals

1. `L` AC Live wire
2. `N` AC Neutral wire
3. `U` Motor common
4. `V` Motor forward
5. `W` Motor reverse
6. `LAMP`
7. `LAMP`
8. `DN` Normally closed limit switch (yellow)
9. `COM` Normally closed limit switch (black)
10. `UP` Normally closed limit switch (red)
11. `VCC` Auxilary output 12VDC (rated output less than 100mA). Use in conjuction with `13 - COM`
12. `PHO` Photocell sensor. Use in conjuction with `13 - COM`
13. `COM` Auxilary output 12vdc common/ground (rated output less than 100mA)
14. `O/S/C` Normally open dry contact signal input. Use in conjuction with `13 - COM`
15. `COM` Accept signal input of the gate opening, usually connected to the normally open output of a vehicle sensor
16. `OPSW`Accept signal input of the gate opening, usually connected to the normally open output of a vehicle sensor. Use in conjuction with `15- COM`

## Accessories Calculations

- **Wired Keypad:** 12-24VDC, < `100mA`
- **Vehicle Sensor** 12-24VDC, `40mA`
- **Homelink** 12-24VDC, `70mA`
- **Inrared Sensors** 12-24VDC, RX `15mA` TX `30mA`

## Connections to Control Board

- **Terminal 1** ``
- **Terminal 2** ``
- **Terminal 3** ``
- **Terminal 4** ``
- **Terminal 5** ``
- **Terminal 6** ``
  - Warning Light `Wire 1`
- **Terminal 7** `1`
  - Warning Light `Wire 2`
- **Terminal 8** ``
- **Terminal 9** ``
- **Terminal 10** `3`
- **Terminal 11** `3`
  - Wired Keypad `Red`
  - Homelink `Terminal 7`
  - Infrared Sensor Receiver `+`
  - Infrared Sensor Transmitter `+`
- **Terminal 12** `2`
- **Terminal 14** `3`
- **Terminal 15** `2`
- **Terminal 16** `1`

## Wiring Diagram

```mermaid
flowchart LR

subgraph GateControlBoard [Gate Control Board]
    terminal11["Terminal 11"]
    terminal12["Terminal 12"]
    terminal13["Terminal 13"]
    terminal14["Terminal 14"]
    terminal15["Terminal 15"]
    terminal16["Terminal 16"]
end

subgraph VehicleSensor [Vehicle Sensor Exit Wand]
    vehRed["Red (24VDC +)"]
    vehGreen["Green (24VDC -)"]
    
    vehBlack["Black (15)"]---terminal15VehicleSensor
    vehBlue["Blue (16)"]---terminal16VehicleSensor

    terminal15VehicleSensor---terminal15
    terminal16VehicleSensor---terminal16
end

subgraph WireKeypad [Wired Keypad]
    keypadRed["Red (11)"]---terminal11WireKeypad
    keypadPurple["Purple (13)"]---terminal13WireKeypad
    keypadBlack["Black (13)"]---terminal13WireKeypad
    keypadBlue["Blue (14)"]---terminal14WireKeypad

    terminal11WireKeypad---terminal11
    terminal13WireKeypad---terminal13
    terminal14WireKeypad---terminal14

end


subgraph HomeLink [Homelink Remote Control]
    homelinkTerminal1["Terminal 1 (13)"]---terminal13HomeLink
    homelinkTerminal2["Terminal 2 (14)"]---terminal14HomeLink
    homelinkTerminal7["Terminal 7 (11)"]---terminal11HomeLink
    homelinkTerminal8["Terminal 8 (13)"]---terminal13HomeLink

    terminal11HomeLink---terminal11
    terminal13HomeLink---terminal13
    terminal14HomeLink---terminal14
end

subgraph WarningLight [Warning Light]
    light1["Wire 1 (6)"]---terminal6WarningLight 
    light2["Wire 2 (7)"]---terminal7WarningLight
end

subgraph InfraredSensor
    subgraph emitter [Emitter]
        emitterPositive["~+ (11)"]---terminal11InfraredSensor
        emitterNegative["~- (13)"]---terminal13InfraredSensor
    end

    subgraph receiver [Receiver]
        receiverPositive["~+ (11)"]---terminal11InfraredSensor
        receiverNegative["~- (13)"]---terminal13InfraredSensor
        receiverNC["NC (12)"]---terminal12InfraredSensor
        receiverCOM["COM (13)"]---terminal13InfraredSensor
    end

    terminal11InfraredSensor---terminal11
    terminal12InfraredSensor---terminal12
    terminal13InfraredSensor---terminal13
end


subgraph WallButton [Wall Push Button]
    wbWire1["Wire 1 (13)"]---terminal13WallButton
    wbWire2["Wire 2 (14)"]---terminal14WallButton

    terminal13WallButton---terminal13
    terminal14WallButton---terminal14
end

subgraph MagneticLock [Gate Lock]

    subgraph magRelay [Relay]

        relayCoilNC["Relay Coil NC"]---terminal12MagneticLock
        relayCoilCOM["Relay Coil COM"]---terminal13MagneticLock

        relayContactMagLockPos["24V Contact MagLock +"]
        relayContactMagLockNeg["24V Contact MagLock -"]

        relayContactPowerPos["24V Power Supply +"]
        relayContactPowerNeg["24V Power Supply -"]
    end

    terminal12MagneticLock---terminal12
    terminal13MagneticLock---terminal13

    subgraph maglock [MagLock]

        maglock24VPos["Maglock 24V +"]---relayContactMagLockPos
        maglock24VNeg["Maglock 24V -"]---relayContactMagLockNeg

        magLockRed["Red"]---maglock24VPos
        magLockWhite["White"]---maglock24VPos
        
        magLockBlack["Black"]---maglock24VNeg
        magLockGreen["Green"]---maglock24VNeg
    end
end


subgraph dcPower [24VDC Power Supply]
    dcPowerPositive["DC Power +"]
    dcPowerNegative["DC Power -"]

    dcPowerPositive---relayContactPowerPos
    dcPowerNegative---relayContactPowerNeg

    
    dcPowerPositive---vehRed
    dcPowerNegative---vehGreen
end
```
