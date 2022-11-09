## Optimizations
 - Loop unrolling
    - Breaking up loop and explicately calculating more values per loop
 - Parallel Accumulators
    - Storing multiple variables per loop
 - Common subexpression elimination
```c++
                                 // Rewritten optimized code as comments
                                 //long inj = i * n + j
up = val[(i - 1) * n + j]        //up = val[inj - n]
down = val[(i + 1) * n + j]      //down = val[inj + n]
left = val[i * n + j - 1]        //left = val[inj - 1]
right = val[i * n + j + 1]       //right = val[inj + 1]
```
 - Instruction pipelining
    - If two instructions of the same type are executed one immediately after another, for a single functional unit, they will be pipelined faster.
 - Inlining
 - Strength Reduction
 - Machine independent optimization

## Ahmdal's Law
$$S = \frac{1}{(1-\alpha) + \alpha /k},\hspace{1 cm} k = \frac{\alpha S}{(\alpha - 1)S + 1}$$
 - $S$ = overall program performance improvement
 - $\alpha$ = portion of the system being improved (%)
 - $k$ = improvement of portion

## Combinatorial Logic Function, Pipelines and Registers
Given the delays at each stage (ps) $a$, $b$, and $c$ and the register delay, $reg$ get the resulting clockspeed (GHz):
### Fully Pipelined
$$\frac{1000}{\text{max}(a,b,c)+reg}$$

### Only adding one register
$$\frac{1000}{\text{min}({a+b, b+c})+reg}$$

## Access Time for Sector
Given rotational rate (rpm), avg seek time (ms), and avg sec per track (sectors/track), 
$$T_{\text{access}} = T_\text{avg seek} + T_\text{avg rot} + T_\text{avg trans}$$
 - $T_\text{avg seek}$ is given
 - $T_\text{avg rot} = \frac{1}{2} \cdot T_\text{max rot} = \frac{1}{2} \cdot \frac{60}{\text{rot rate}} * 1000\frac{ms}{s}$
 - $T_\text{avg trans} = \frac{60}{\text{rot rate}} \cdot \frac{1}{\text{avg sec/track}} \cdot 1000\frac{ms}{s}$

## File Seek
Make sure to convert seeking time to s from ms (divide seek by 1000)
$$T_{\text{fs}} = T_{\text{seek}} + \frac{\text{file size}}{\text{throughput}},\hspace{1 cm} \text{files per second} = \frac{1}{T_{\text{fs}}}$$

## Random I/O (IOPS)
Given $T_{\text{seek}}$ in ms
$$\frac{1000}{T_{\text{seek}}}$$

## Cache Calculation
Given $C$-byte cache, $M$-bit memory addresses, $B$-byte cache blocks, $E$-way associative cache:
 - $S = \frac{C}{B \cdot E}$
 - (CO bits) $b = \log_2 B$
 - (CI bits) $s = \log_2 S$
 - (CT bits) $t = M - b - s$

If you are instead asked to find $C$, the cache size in bytes, given the previous information, use
 - $C = E \cdot S \cdot B$

## Cache miss rate for loop
Given $L$-byte sized cache lines
```c++
struct DataStruct{
   char a, b, c, d;
}
DataStruct dataStruct[400];

for(int i = 0, i<400;i++){
   dataStruct[i].a = 0;
   dataStruct[i].b = 0;
   dataStruct[i].c = 0;
   dataStruct[i].d = 0;
}
```
Since the cache is empty, the first element accessed will always be a miss. Then, there will be $\frac{\text{cache line bytes}}{\text{sizeof(DataStruct)}}$ elements of the array loaded into the cache everytime a miss happens. So in this code case, if the $\text{sizeof(DataStruct)}=4$ and the cache has 4-byte lines, then every loop iteration will miss `dataStruct[i].a` for every loop, but load in `dataStruct[i].b`, `dataStruct[i].c`, and `dataStruct[i].d`, which will all be hits. Thus, we will have a sequence: `miss, hit, hit, hit, miss, hit, hit, hit,...` which results in a cache miss rate of 25%