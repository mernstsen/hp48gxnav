# Geo

## COO
Input geographical coordinates.

## CAL
Calculate great circle distance (GCD) and initial great circle track (GC0).

## Notes
The calculations may not be accurate. Unfortunately, no derivation of the calculations exist. Do not use for real-life navigation or planning.

## Program listing

### COO
```
<<
"COORDINATES"
{ { "FROM N" }
{ "FROM E" }
{ "TO N" }
{ "TO E" } }
{ 1 5 }
{ }
NS0 EW0 NS1 EW1 4 ->LIST
INFORM

IF 1 == THEN
  'EW1' STO 'NS1' STO 'EW0' STO 'NS0' STO
END
>>
```

## CAL
```
<<
R 90 NS0
HMS-> - SIN
EW0 HMS-> COS * * R 90 NS0 HMS-> -
SIN EW0 HMS-> SIN * * R 90 NS0 HMS-> -
COS * ->V3
'P0' STO

R 90 NS1 HMS-> - SIN
EW1 HMS-> COS * * R
90 NS1 HMS-> - SIN
EW1 HMS-> SIN * * R
90 NS1 HMS-> - COS * ->V3
'P1' STO

NS0 HMS-> NS1 HMS-> - 2 /
SIN EW1 EW0 - ABS * 2 /
'CA' STO P0 P1
DOT LASTARG ABS
SWAP ABS * / ACOS
360 / 2 R π * *
->NUM * 1852 /
'GCD' STO

P0 Z P0 CROSS
CROSS P0 P1 CROSS
P0 CROSS DOT
LASTARG ABS SWAP
ABS * / ACOS
'GC0' STO

Z P0 CROSS P0
P1 CROSS P0 CROSS DOT LASTARG ABS
SWAP ABS * / ACOS ->A

  <<
  IF 'A>90' THEN
    360 GC0 - CA -
    'GC0' STO
  END
  IF 'A<=90' THEN
    GC0 CA +
    'GC0' STO
  END
  >>
>>
```
