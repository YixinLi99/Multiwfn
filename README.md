# Multiwfn


In order to calculate the electrostatic potential atomic charges, some methods are employed: RESP, if MD charges are to be calculated.

Several helpful tutorials: http://sobereva.com/441, http://sobereva.com/476, http://sobereva.com/531

## Generation of eqvcons.file
See how many elements: `sort ele.txt |uniq -c|sort -nr`
```
  88 H
  84 C 
   8 N 
   4 S 
   4 F 
   2 O 
 ```
`AQx.mol2` -> `num.txt` where only $1 and $2  
**Extract data:**
`awk '{for (f=1; f <= NF; f+=1) {if ($f ~ /H/) {printf("%-10s%s\n",$1,$2)}}}' num.txt > H_eqvcons.txt `  
**Count output numbers:**
`cat H_eqvcons.txt|cut -c 15|uniq -c`  
**Output:**
`awk '{printf "," $1}' H_eqvcons.txt > H_out.txt`  
**Useful Links:** > https://www.cnblogs.com/along21/p/10366886.html
