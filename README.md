# Multiwfn


In order to calculate the electrostatic potential atomic charges, some methods are employed: RESP, if MD charges are to be calculated.

Several helpful tutorials: http://sobereva.com/441, http://sobereva.com/476, http://sobereva.com/531


Extract data: 
`awk '{FS = ";"}{for (f=1; f <= NF; f+=1) {if ($f ~ /C/) {printf("%-10s%s\n",$1,$2)}}}' AQx.mol2 > eqcon.txt `
