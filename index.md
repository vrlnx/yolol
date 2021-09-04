## YOLOL by vrl


### Who is VRL?

VRL is the author of malware known as vrl-package, owner of vrl.sh domain and Founder of the VRL Community Discord

### Support or Contact

Having trouble with VRL's YOLOL scripts? Check out VRL's [wiki page](https://github.com/vrlnx/yolol/wiki) or [create a ticket on Discord](http://d.vrl.sh/) and I'll try to help you sort it out.

> ### Adding codes when requested
> I'll try my best to structure the YOLOL code to make sense.

## Management

**Generator Script**

```
MaxRate=:MaxBatteryPower
:GeneratorUnitRateLimit=:MinEPS+(MaxRate-:StoredBatteryPower)/100 goto2

// :MinEPS = Hybrid button with name "MinESP"
// :MinEPS values On = 65
// This allows you to have a pre-charged power when using Lasers
```


**Generator Script with Power button**

```
MaxRate=:MaxBatteryPower if:Power==0 then goto3 end
:GeneratorUnitRateLimit=:MinEPS+(MaxRate-:StoredBatteryPower)/100 goto2
:GeneratorUnitLimit=0 goto1

// :MinEPS = Hybrid button with name "MinEPS"
// :MinEPS values On = 65
// This allows you to have a pre-charged power when using Lasers
// :Power = Name of the button that cut power to the system
// :Power on state value of `1` and off state value of `0`
```


## Navigation

**Auto-Approach Lite**

```
z=20 x=1000.000 y=25
if :RF<x then :FcuForward=y*:RF/1000 goto3 end goto5
if :RF<z then goto4 end goto2
:FcuForward=0 :Range=0 :Approach=-1
goto1

// [Z] - Approach Limit [Changable]
// [Y] - Approach coefficency [Changable]
// :RF - RangeFinder Distance
// :Range - Renamed from RangeFinderOnState
// `Approach` is the name of the YOLOL Chip, 
// and also a Hybrid button with off state `-1` and on state `0`
```


**Secure Transponder**

```
if NOT:insideSafeZone then:transponderActive=0 end goto1

// ":transponderActive" can be changed to ":Transponder"
// if your ship is using a setup like this.
```

## Mining

**Pulseating Lasers with Pulseating Collector**

```
if:Mine==1 then:MiningLaserOn=1:Collector=0 goto2 end goto3
:MiningLaserOn=0:Collector=1 goto1
:MiningLaserOn=0:Collector=0 goto1

// :Mine = A hybrid button with name "Mine"
```

**Smart scanning**

```
a=" Ore" b=0 c=" Crystal" n="\n"
sr=:ScanResults i2="" m2="" v2="" :ScanWait=3*(sr>2)
:Index=b i1=:Index+1+":" m1=:Material-a-c v1=:Volume goto5-(sr>1)
b+=1 :Index=b i2=:Index+1+":" m2=:Material-a-c v2=:Volume
:Scanned = n+i1+m1+n+v1+n+i2+m2+n+v2 b*=b<(sr-1) goto2

// :ScanWait = Changed the YOLOL chip name from "ChipWait"
// :Scanned = Text panel with name "Scanned"
```

