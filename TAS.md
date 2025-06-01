# TAS
Compute true airspeed (TAS) given calibrated airspeed (CAS), pressure altitude and outside air temperature as inputs.

## Notes
The source of the formula used is unknown and results may not be accurate. Do not use for real-life navigation or planning.

## Program listing
```
<<
0 FIX

"TAS"
{ { "CAS" "CALIBRATED AIRSPEED IN ANY UNIT" }
{ "PRESS ALT" "PRESSURE ALTITUDE IN FEET" }
{ "OAT" "OUTSIDE AIR TEMPERATURE IN ºC" } }
{ 1 7 }
{ }
CAS PALT OAT 3 ->LIST
INFORM

IF 1 == THEN
  OBJ-> DROP 'OAT' STO 'PALT' STO 'CAS' STO
  'CAS*√((OAT+273)/((1-0.00688*(PALT/1000))^5.256*288))' EVAL
  DUP
  UPDIR WIND 'TAS' STO
  UPDIR TAS 0 RND
  CAS "CAS" ->TAG SWAP
  PALT "PALT" ->TAG SWAP
  OAT "OAT" ->TAG SWAP
  "TAS" ->TAG
END

STD
>>
```
