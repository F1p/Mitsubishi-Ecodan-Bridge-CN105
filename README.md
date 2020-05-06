# Mitsubishi-CN105-Protocol-Decode
For Ecodan ASHP Units
- [Physical](#physical)
- [Command Format](#command-format)
  * [Checksum](#checksum)
  * [Preamble](#preamble)
    + [Query Preamble](#query-preamble)
    + [Response Preamble](#response-preamble)
- [Commands](#commands)
  * [0x01](#0x01)
  * [0x09](#0x09)
  * [0x0b](#0x0b)
  * [0x0c](#0x0c)
  * [0x0d](#0x0d)
  * [0x26](#0x26)
  
  


# Physical
Serial, 2400, 8, E, 1

# Command Format

| Preamble | Command | Payload | Checksum |
| --- | --- | --- | --- |
| 5 Bytes | 1 Byte | 15 Bytes | 1 Byte |

## Checksum

Checksum = 0xfc - Sum ( PacketBytes[0..20]) ;
## Preamble
### Query Preamble
| 0 | 1 | 2 | 3 | 4 |
| ---  | ---  | ---  | ---  | ---  |
| 0xfc | 0x42 | 0x02 | 0x7a | 0x10 |

### Response Preamble

| 0 | 1 | 2 | 3 | 4 |
| ---  | ---  | ---  | ---  | ---  |
| 0xfc | 0x62 | 0x02 | 0x7a | 0x10 |

# Commands
| Command | Description |
| ------- | ----------- |
| 0x00 |     |
| [0x01](#0x01) | Time & Date |
| [0x09](#0x09) | Zone1, Zone2, FlowSetPoint, FlowTemp, WaterSetPoint |
| [0x0b](#0x0b) | Zone1, Outside |
| [0x0c](#0x0c) | WaterHeatingFeed, WaterHeatingReturn, HotWater |
| [0x0d](#0x0d) | fBoilerFlow,  fBoilerReturn |
| [0x26](#0x26) | HWSetPoint, ExternalSetPoint, ExternalFlowTemp, Operation Mode |

## 0x01
### Query
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0  | 0  |  0 |  0 |  0 |

### Response
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
| Y | M | D | h | m | s |   |   |   |   |    |    |    |    |    |

* Y: Year
* M: Month
* D: Day
* h: Hour
* m: Minute
* s: Second

## 0x09
### Query
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0  | 0  |  0 |  0 |  0 |

### Response
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
| Z1.u | Z1.l | Z2.u | Z2.l | SP.u | SP.l | FT.u | FT.l | HWSP.u  | HWSP.l  |    |    |    |    |    |

* Zone1 Temperature:  ((Z1.u <<8 ) + Z1.l) / 100;
* Zone2 Temperature:  ((Z2.u <<8 ) + Z2.l) / 100;
* Flow Setpoint    :  ((SP.u <<8 ) + SP.l) / 100;
* Flow Temperature :  ((FT.u <<8 ) + FT.l) / 100;
* Hot Water Temp   :  ((HW.u <<8 ) + HW.l) / 100;


## 0x0b
### Query
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0  | 0  |  0 |  0 |  0 |

### Response
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
| Z1.u | Z1.l |    |    |   |  | Z2.u | Z2.l |  |  | O | |  |   |    |    |    |    |    |

* Zone1 Temperature:  ((Z1.u <<8 ) + Z1.l) / 100;
* Zone2 Temperature:  ((Z2.u <<8 ) + Z2.l) / 100;
* Outside Temp     :  (O/2) -39; 

## 0x0c
### Query
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0  | 0  |  0 |  0 |  0 |

### Response
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
| F.u | F.l |  | R.u |R.l |  | Hw.u | Hw.l |   |   |    |    |    |    |    |

* Water Heater Feed Temperature  :  ((F.u <<8 ) + F.l) / 100;
* Water Heater Return Temperature:  ((R.u <<8 ) + R.l) / 100;
* Water Temperature              :  ((HW.u <<8 ) + HW.l) / 100;

## 0x0d
### Query
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0  | 0  |  0 |  0 |  0 |

### Response
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
| F.u | F.l |  | R.u |R.l |  |  |  |   |   |    |    |    |    |    |

* Boiler Flow Temperature  :  ((F.u <<8 ) + F.l) / 100;
* Boiler Return Temperature:  ((R.u <<8 ) + R.l) / 100;

## 0x26
### Query
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0  | 0  |  0 |  0 |  0 |

### Response
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|
|  |  |  |  | |  |  |  |   |   |    |    |    |    |    |

* HotWater SetPoint  :  (( <<8 ) + F.l) / 100;
* External Flow SetPoint:  (( <<8 ) + R.l) / 100;
* External Flow Temp:
* Operation Mode: 
** 0 : Temperature Mode
** 1 : Flow Control Mode
** 2 : Compensation Curve Mode


