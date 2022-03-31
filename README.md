# Multiwfn


In order to calculate the electrostatic potential atomic charges, some methods are employed: RESP, if MD charges are to be calculated.

Several helpful tutorials: http://sobereva.com/441, http://sobereva.com/476, http://sobereva.com/531
(1)结构优化：对于计算拟合静电势电荷的目的，结构必须经过优化，但结构优化精度只要达到不错就可以了，不用要求超级精确，当然也不能太次。一般情况，理论方法就用常用的B3LYP即可，如果你不是量子化学内行的话，那么建议你在优化时总是带上DFT-D3色散校正。对前三周期元素，基组就用6-311G**即可。如果体系里有第四周期及之后的原子，对它们就用SDD赝势和它标配的赝势基组即可。

(2)计算静电势：泛函还是用B3LYP就可以。带不带DFT-D3校正对静电势计算结果无任何直接影响，因此可带可不带。如前述的《原子电荷计算方法的对比》一文的图5所示，只要基组达到中等质量，继续增大基组对结果也没有什么显著影响。对前三周期元素基组用6-311G**就够了，过渡金属还用SDD即可，而诸如Br、I等第四周期及之后的主族元素，我建议用lanl08(d)，这是带d极化函数的赝势基组（SDD赝势原始标配的赝势基组对主族是不带d极化函数的，因此对静电势的描述注定不会很理想）。

RESP电荷拟合分为以下两步：
• 第一步：拟合电荷时使用双曲惩罚函数对非氢原子施加弱限制（a=0.0005），不约束原子的等价性，所有原子的电荷都被拟合。这一步允许原子电荷变化有最大的自由度，以充分让极性原子尽可能好地拟合静电势。
• 第二步：拟合电荷时使用双曲惩罚函数对非氢原子施加强限制（a=0.001），只允许sp3杂化的碳、亚甲基的碳，以及它们上面的氢的电荷被拟合，而其它原子的电荷保持上一步最后的状态。拟合中还约束每个-CH3, =CH2, -CH2-基团上的氢的电荷保持等价性。

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

step1: Use GaussView to symmetrise the system
step2: go through Optimisation process using Gaussian and output file: `AQx.fch` and `AQx.out`
step3: `AQx.out` is the `AQx.log` file, put it into GaussView and save the file as `AQx.mol2` file  
step4: use multiwfn: 
```module load …
ulimit -s unlimited
multiwfn AQx.fch
7
18
1
```
output file: 
step5: go to ACPYPE and type `acpype -i AQx.mol2 -c user -k maxcyc=0`, it will output: `gro`, `itp`, `top` files
step6: opt `itp` and replace the charges `0.00000` with the RESP calculated charges using sublime txt app to easily replace

