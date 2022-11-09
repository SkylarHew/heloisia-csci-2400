## Optimizations
 - Loop unrolling
    - Breaking up loop and explicately calculating more values per loop
 - Parallel Accumulators
    - Storing multiple variables per loop
 - Common subexpression elimination
 - Instruction pipelining
 - Inlining
 - Strength Reduction
 - Machine independent optimization

## Ahmdal's Law
$$S = \frac{1}{(1-\alpha) + \alpha /k},\hspace{1 cm} k = \frac{\alpha S}{(\alpha - 1)S + 1}$$
 - $S$ = performance improvement
 - $\alpha$ = amount of system that can be improved
 - $k$ = system improvement 

## Combinatorial Logic Function, Pipelines and Registers
Given the delays at each stage (in picoseconds `ps`) $a$, $b$, and $c$ and the register delay, $reg$ get the resulting clockspeed (`GHz`):
### Fully Pipelined
$$\frac{1000}{\text{max}(a,b,c)+reg}$$

### Only adding one register
$$\frac{1000}{\text{min}({a+b, b+c})+reg}$$

## Access Time for Sector
Given rotational rate (`rpm`), avg seek time (`ms`), and avg sec per track (`sectors/track`), 
$$T_{\text{access}} = T_\text{avg seek} + T_\text{avg rot} + T_\text{avg trans}$$
 - $T_\text{avg seek}$ is given
 - $T_\text{avg rot} = \frac{1}{2} \cdot T_\text{max rot} = \frac{1}{2} \cdot \frac{60}{\text{rot rate}} * 1000\frac{ms}{s}$
 - $T_\text{avg trans} = \frac{60}{\text{rot rate}} \cdot \frac{1}{\text{avg sec/track}} \cdot 1000\frac{ms}{s}$

## File Seek
Make sure to convert seeking time to `s` from `ms` (divide seek by 1000)
$$T_{\text{fs}} = T_{\text{seek}} + \frac{\text{file size}}{\text{throughput}},\hspace{1 cm} \text{files per second} = \frac{1}{T_{\text{fs}}}$$

## Random I/O (IOPS)
Given $T_{\text{seek}}$ in `ms`
$$\frac{1000}{T_{\text{seek}}}$$

## Cache Calculation
Given $C$-byte cache, $M$-bit memory addresses, $B$-byte cache blocks, $E$-way associative cache:
 - $S = \frac{C}{B \cdot E}$
 - (CO bits) $b = \log_2 B$
 - (CI bits) $s = \log_2 S$
 - (CT bits) $t = M - b - s$