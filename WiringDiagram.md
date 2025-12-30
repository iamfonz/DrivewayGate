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
- **Terminal 6**
  - Warning Light `Wire 1`
- **Terminal 7**
  - Warning Light `Wire 2`
- **Terminal 8**
- **Terminal 9**
- **Terminal 10**
- **Terminal 11**
  - Wired Keypad `Red`
  - Homelink `Terminal 7`
  - Infrared Sensor Receiver `+`
  - Infrared Sensor Transmitter `+`
- **Terminal 12**
  - Infrared Sensor Receiver `NC`
  - MagLock Relay `NC`
- **Terminal 13**
  - Wired Keypad `Purple`
  - Wired Keypad `Black`
  - Homelink `Terminal 1`
  - Homelink `Terminal 8`
  - Infrared Sensor Emitter `~ -`
  - Infrared Sensor Receiver `~ -`
  - Infrared Sensor Receiver `COM`
  - Wall Button `Wire 1`
  - MagLock Relay `COM`
- **Terminal 14**
  - Wired Keypad `Blue`
  - Homelink `Terminal 2`
  - Wall Button `Wire 2`
- **Terminal 15**
  - Vehicle Sensor `Black`
- **Terminal 16**
  - Vehicle Sensor `Blue`

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

subgraph WireKeypad [Wired Keypad]
    keypadRed["Red (11)"]---terminal11
    keypadPurple["Purple (13)"]---terminal13
    keypadBlack["Black (13)"]---terminal13
    keypadBlue["Blue (14)"]---terminal14
end


subgraph HomeLink [Homelink Remote Control]
    homelinkTerminal1["Terminal 1 (13)"]---terminal13
    homelinkTerminal2["Terminal 2 (14)"]---terminal14
    homelinkTerminal7["Terminal 7 (11)"]---terminal11
    homelinkTerminal8["Terminal 8 (13)"]---terminal13

end

subgraph WarningLight [Warning Light]
    light1["Wire 1 (6)"]---terminal6 
    light2["Wire 2 (7)"]---terminal7
end

subgraph InfraredSensor
    subgraph emitter [Emitter]
        emitterPositive["~+ (11)"]---terminal11
        emitterNegative["~- (13)"]---terminal13
    end

    subgraph receiver [Receiver]
        receiverPositive["~+ (11)"]---terminal11
        receiverNegative["~- (13)"]---terminal13
        receiverNC["NC (12)"]---terminal12
        receiverCOM["COM (13)"]---terminal13
    end
end


subgraph WallButton [Wall Push Button]
    wbWire1["Wire 1 (13)"]---terminal13
    wbWire2["Wire 2 (14)"]---terminal14
end

subgraph MagneticLock [Gate Lock]

    subgraph magRelay [Relay]

        relayCoilNC["Relay Coil NC"]---terminal12
        relayCoilCOM["Relay Coil COM"]---terminal13

        relayContactMagLockPos["24V Contact MagLock +"]
        relayContactMagLockNeg["24V Contact MagLock -"]

        relayContactPowerPos["24V Power Supply +"]
        relayContactPowerNeg["24V Power Supply -"]
    end

    subgraph maglock [MagLock]

        maglock24VPos["Maglock 24V +"]---relayContactMagLockPos
        maglock24VNeg["Maglock 24V -"]---relayContactMagLockNeg

        magLockRed["Red"]---maglock24VPos
        magLockWhite["White"]---maglock24VPos
        
        magLockBlack["Black"]---maglock24VNeg
        magLockGreen["Green"]---maglock24VNeg
    end
end


subgraph VehicleSensor [Vehicle Sensor Exit Wand]
    vehRed["Red (24VDC +)"]
    vehGreen["Green (24VDC -)"]
    vehBlack["Black (15)"]---terminal15
    vehBlue["Blue (16)"]---terminal16
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
