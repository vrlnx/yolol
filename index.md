## YOLOL by vrl


### Who is VRL?

VRL is the author of malware known as vrl-package, owner of vrl.sh domain and Founder of the VRL Community Discord

### Support or Contact

Having trouble with VRL's YOLOL scripts? Check out VRL's [wiki page](https://github.com/vrlnx/yolol/wiki) or [create a ticket on Discord](https://discord.io/cardbox) and I'll try to help you sort it out.

> ### Adding codes when requested
> I'll try my best to structure the YOLOL code to make sense.

## Management
**Headless Generator Script**

```
R=:MaxBatteryPower
:GeneratorUnitRateLimit=(R-:Battery)/100 goto2

// :MinEPS = Hybrid button with name "MinESP"
// :MinEPS values On = 65
// This allows you to have a pre-charged power when using Lasers
```

**Generator Script with a MinEPS button**

```
R=:MaxBatteryPower
:GeneratorUnitRateLimit=:MinEPS+(R-:Battery)/100 goto2

// :MinEPS = Hybrid button with name "MinESP"
// :MinEPS values On = 65
// This allows you to have a pre-charged power when using Lasers
```


**Generator Script with Power(shutdown) button and MinEPS button**

```
R=:MaxBatteryPower if:Power==0 then goto3 end :Generator=100
:GeneratorUnitRateLimit=:MinEPS+(R-:Battery)/100 goto1
:Generator=0 goto1

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
a=" Ore" b=0 c=" Crystal" n="\n" :Scan=1
sr=:ScanResults i2="" m2="" v2="" :ScanWait=3*(sr>2)
:Index=b i1=:Index+1+":" m1=:Material-a-c v1=:Volume goto5-(sr>1)
b+=1 :Index=b i2=:Index+1+":" m2=:Material-a-c v2=:Volume
:Scanned = n+i1+m1+n+v1+n+i2+m2+n+v2 b*=b<(sr-1) goto2

// :ScanWait = Changed the YOLOL chip name from "ChipWait"
// :Scanned = Text panel with name "Scanned"
```

**Smart Auto Centering Lasers**

```
co=90
:ML=22*:Mine-:R :RT=co/:R :LT=-:RT goto2

// R = Rangefinder Distance
// Mine = Rangefinder On State
// ML = MiningLaser On State
// RT = Right turret
// LT = Left Turret
// co = Convergent point
// 22 = The start length of the Rangefinder, if it's more than 22 Lasers will not turn on.
```


### Smart scanning with 2 YOLOL chip and 1 Memory Chip


*Config - Memory Chip*
```
Field name 1: v1 Value: 0
Field name 2: v2 Value: 0
Field name 3:  s Value: 11
```


*Name the YOLOL chip: ScanWait*
```
n="\n" k=" kv" a=" Ore" b=0 c=" Crystal" d="Tier: "
a=" Ore" c=" Crystal" k=" kv" s=" stk" d="Tier: " :Scan=1
sr=:ScanResults :ScanWait=5
:Index=0 m1=:Material-a-c :v1=:Volume
:Index=1 m2=:Material-a-c :v2=:Volume
:calc=0 z=:v1 x=:v2 zz=:v1/1728 xx=:v2/1728
L1=n+m1+n+z+k L2=n+m2+n+x+k
L3=n+m1+n+zz+s L4=n+m2+n+xx+s
T=n+d+:s
:Scanned=L1+n+L2+n+T
:ScanWait=20 if:Scanner==1 thengoto1end

:Scanned=L3+n+L4+n+T
:ScanWait=20 if:Scanner==1 thengoto1end
goto9
```


*Name the YOLOL chip: Calc*
```
:s=0 a=(:v1+:v2) b=6047 c=7212 d=7359 e=7522 f=14055 g=18061 h=30546
i=75923 j=198390 k=433682 l=811196 ifa>=b then :s=1 end
if a>=c then :s=2 end if a>=d then :s=3 end if a>=e then :s=4 end
if a>=f then :s=5 end if a>=g then :s=6 end if a>=e then :s=4 end
if a>=e then :s=4 end if a>=f then :s=5 end if a>=g then :s=6 end
if a>=k then :s=10 end if a>=l then :s=11 end
calc=-1 goto1
```

## Ship specific modification
#### Ship: Pincer mod


 **Smart Astroid grabber with Auto-approach Lite modified**

```
if:TractorOn==1 then:Rmk=1 goto2end if:Approach==-1 then:Rmk=0endgoto1
:Beam_length=(:RF-.2) :Beam=:Beam_length
if:TractorOn AND:RF<=2 then:Lock=1 goto3endgoto1

// TractorOn = On state of the tractor beam
// Beam_length = is the replacement for "tractorBeamPosition"
// Beam = is the replacement for "tractorBeamSearchLength"
// RF = is the replacement for "RangeFinderDistance"
// Rmk = is the replacement for "RangeFinderOnState"
// lock = is the replacement for "CargoBeamOnState"
// Approach is the name of the YOLOL chip that is with the auto-approach script. But is used as a dead-man switch in this script to cross check and verify.
```



**Auto-Approach Lite Modified for Smart Astroid grabber**

```
z=20 x=1000.000 y=65 :Rmk=1
if:RF<x then:FcuForward=y*:RF/100 goto3 end goto5
if:RF<z then goto4 end goto2
:FcuForward=0 :Rmk=0 :Approach=-1
goto1

// [Z] - Approach Limit [Changable]
// [Y] - Approach coefficency [Changable]
// :RF - RangeFinder Distance
// :Range - Renamed from RangeFinderOnState
// `Approach` is the name of the YOLOL Chip, 
// and also a Hybrid button with off state `-1` and on state `0`
```
