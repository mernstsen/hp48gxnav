# Wind

## TH & GS
Calculate true heading and ground speed given true track, true airspeed, wind direction and wind velocity.

## TT & GS
Calculate true track and ground speed given true heading, true airspeed, wind direction and wind velocity.

## Wind
Calculate wind direction and wind velocity given true heading, true airspeed, true track and ground speed.

## Notes
The source of the formula used is unknown and results may not be accurate. Do not use for real-life navigation or planning.

## Program listing

### TH & GS
```
<<
0 FIX
"TH & GS"
{ { "TT" "TRUE TRACK" }
{ "TAS" "TRUE AIRSPEED" }
{ "WD" "WIND DIRECTION" }
{ "WV" "WIND VELOCITY" } }
{ 1 5 }
{ }
TT TAS WD WV 4 ->LIST
INFORM

IF 1 == THEN
  OBJ-> DROP
  'WV' STO 'WD' STO 'TAS' STO 'TT' STO
  'ASIN(WV/TAS*SIN(WD)*COS(TT)-WV/TAS*COS(WD)*SIN(TT))+TT' EVAL
  DUP DUP
  IF 360 > THEN
    360 -
  END
  IF 0 < THEN
    360 +
  END
  'TH' STO
  '√((-WV*SIN(WD)+TAS*SIN(TH))^2+(-WV*COS(WD)+TAS*COS(TH))^2)' EVAL
  'GS' STO
  TH 0 RND "TH " ->TAG
  GS 0 RND "GS " ->TAG
END
STD
>>
```

## TT & GS
```
<<
0 FIX
"TT & GS"
{ { "TH" "TRUE HEADING" }
{ "TAS" "TRUE AIRSPEED" }
{ "WD" "WIND DIRECTION" }
{ "WV" "WIND VELOCITY" } }
{ 1 5 }
{ }
TH TAS WD WV 4 ->LIST
INFORM

IF 1 == THEN
  OBJ-> DROP
  'WV' STO 'WD' STO 'TAS' STO 'TH' STO
  '√((TAS*SIN(TH)-WV*SIN(WD))^2+(TAS*COS(TH)-WV*COS(WD))^2)' EVAL
  'GS' STO GS
  DUP
  'TAS*COS(TH)-WV*COS(WD)' EVAL
  SWAP
  / ACOS
  'TT' STO
  'TAS*SIN(TH)-WV*SIN(WD)' EVAL
  SWAP
  / ACOS
  -> B
  <<
  IF 'B>90' THEN
    360 TT -
    'TT' STO
  END
  >>
  TT 0 RND "TT" ->TAG
  GS 0 RND "GS" ->TAG
END
STD
>>
```

### Wind
```
<<
0 FIX
"WIND"
{ { "TH" "TRUE HEADING" }
{ "TAS" "TRUE AIRSPEED" }
{ "TT" "TRUE TRACK" }
{ "GS" "GROUND SPEED" } }
{ 1 5 }
{ }
TH TAS TT GS 4 ->LIST
INFORM

IF 1 == THEN
  OBJ-> DROP
  'GS' STO 'TT' STO 'TAS' STO 'TH' STO
  '√((GS*SIN(TT)-TAS*SIN(TH))^2+(GS*COS(TT)-TAS*COS(TH))^2)' EVAL
  'WV' STO WV
  DUP
  'GS*COS(TT)-TAS*COS(TH)' EVAL
  SWAP
  / ACOS
  'WD' STO
  'GS*SIN(TT)-TAS*SIN(TH)' EVAL
  SWAP
  / ACOS
  -> B
  <<
  IF 'B<=90' THEN
    WD 180 +
    'WD' STO
  END
  IF 'B>90' THEN
    360 WD -
    180 -
    'WD' STO
  END
  >>
  WD 0 RND "WD" ->TAG
  WV 0 RND "WV" ->TAG
END
STD
>>
```
