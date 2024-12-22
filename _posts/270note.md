---
title: Notes on Intro to Logical circuit design
published: true
---

[[CTPP]]

### duality
 when there is theorem (an equation), we flip + and $\cdot$, 0 and 1, get same result
#### Boolean Algebra
- $x+0=x$
- $x\cdot 1=x$
- $x\cdot (y+z)=x\cdot y+x\cdot z$
- $x+(y\cdot z)= (x+y)\cdot (x+z)$
- $x_0 \oplus (x_1\oplus(x_2\oplus(\dots)))$ is 1 iff number of x that are 1 is odd (induction)
- $(x\odot y)\odot z = (x\oplus y)\oplus z$
- ways to prove thm:
	- perfect induction: truth table
![[Pasted image 20241007121409.png]]
### Switching function

- Boole's Expansion theorem
$$f(x_1,x_2,...,x_n) = x'_1\cdot f(0,x_2,\dots,x_n) + x_1\cdot f(1,x_2,\dots.x_n)$$
	think about how you can prove it
- boole's expansion to minterm:
	- by recursively expanding, we can finally get:$$f(x)=a_0m_0+\dots+a_nm_n$$
	- where $a_0=f_{x'y'}$ 
- there are $2^{2^n}$ switching function of functions of n variables(2 var, 4 minterms, 16 combination of minterm coeff).  a two input selector.
- Functional complete: {+,$\cdot$,'}, {+,'}, {$\cdot$,'},NAND, NOR
	- judge whether not, and ,or can be obtained.
#### number conversion
- octal/hex to binary, 3/4 as a pack and convert
- from hex to decimal. $\sum c_k\cdot r^{k}$
- from decimal to hex/binary/octol: short division(more significant terms require more dividing)
- fraction conversion: repeatedly times 2(base), take out the decimal term times two... Up to down, msb to lsb
#### binary arithmetic
- why 2's complement:
	- To avoid two zeros in the middle which is much more troublesome than having asimmetric at the edge

- to complement a number: ~a+1
	- Why is it the case?    
![[Pasted image 20240923111630.png]]
- overflow: larger than the limit of expression
	- Sign bits: attends same result different
	- $S = (x\oplus y)\oplus c_{in}$
		- note that $(x\odot y)\odot z = (x\oplus y)\oplus z$
	- $c_{out} = xy+c_{in}(x\oplus y)$
	- Ovf = $a_{n-1}\oplus S_{n-1} = c_{n-1}\oplus c_n$(take $a_n=b_n$)
### combinational building blocks
- decoder: 
	- Is decoder functional complete?
		- Do we have inverters? 
	- cascading: (see mux)
	- Function implementation
		- the input is minterm
- Encoder
	- Priority encoder:
		- Priority resolver --> encoder
- Multiplexer(MUX):  digital switch
	- Function implementation with MUX
	- **Cascading**: from $2\rightarrow 1$ to $2^n\rightarrow 1$  use $2^n-1$ 
	- function using MUX: n-input func: $2^{n-1}$ (or less) input mux(Boole's theorem)
		-  # of function an n select mux? $2^{2^n}$
- Demux(= decoder)
- Tristate Gates: 
	- multiple agents in a single wire
	- Devices on a line communicate
	- Only one can write but multiple can read
### Sequential circuit
- next state equation $X^+=f(X,Y)$
- A state is a complete *assignment* to the state variable X
- SR Latch
	- Cross-Coupled NOR Gates
	- Remember 1 in a nor gate will result in definite 0. 0 in a nor gate will flip the other input
	- Never go from invalid To hold
	- think of it like the two Q and QB output are competing to be different from the other.
	- D latch (prevent 11->00)
	- Synchronous reset/asynchronous reset: whether only reset with clock change

| S   | R   | Q   | QB  | F       |
| --- | --- | --- | --- | ------- |
| 0   | 0   | Q   | QB  | Hold    |
| 0   | 1   | 0   | 1   | Reset   |
| 1   | 0   | 1   | 0   | Set     |
| 1   | 1   | 0   | 0   | Invalid |

| S   | R   | Q   | QB  | F       |
| --- | --- | --- | --- | ------- |
| 0   | 0   | Q   | QB  | Hold    |
| 0   | 1   | 0   | 1   | Reset   |
| 1   | 0   | 1   | 0   | Set     |
| 1   | 1   | QB   | Q   | toggle |

![jkff.png](assets/imgs/jkff.png)

#### FSM inplementation
1. (optional) Construct state diagram
2. Construct state/output table
3. Create state assignments  (Q0 <=> 00 etc)
4. Create transition/output table (substitute Q0 to 00 in state table)
5. Choose FF type (D-ff)
6. Construct excitation/output table (Q0+ => D0)
7. Find excitation and output logic equations ($D0 = F(input, Q_n)$)
- transition Equations:
- state table/transition table(excitation table)
![state_table.png](assets/imgs/state_table.png)
- *Q*: is there any one to one correspondance between moore machine and mealy machine
#### design optimization?
- 2-level circuit: only two layers of gates
	- SOP & POS
	- NAND/NAND , NOR/NOR
- Cost of implementation: how many literals
- Karnaugh Maps: 2D representation of truth table.
	- combine adjacent minterms(1s).
- Covering
- Implicant -> Prime implicant->Essensial prime implicant(1 cells - only selected once) 
- Procedure:
	1. Find PIs
	2. Remove EPIs (include them I'm the SOP)
	3. repeat the steps
- POS: flip the 1's and do the procedure (check !, duality
	- also, remember to flip the resulting literals
- dont care: try forming EPIs' with dont care and eliminate,
    - Use d cells to make PIs as large as possible
    - No prime implicant should include only d’s
    - Only 1-cells should be considered when finding minimal covering set of PIs
![dontcaresop.png](assets/imgs/dontcaresop.png)
>[!tip] minterm is pi <=> it is epi
#### Counter
- register: multiple D flipflop with common clk(basic way of storing bits)
	- reg and rom? ROM can only implement moore machine?
- **ripple counter** (jk flip flop-11-> Q'): Q\[i] periodic. period doubles.
	- delays, asynchronous
- **parallel counter**(synchronous)
	- polling(1 out of n sequence)
- decrease modulo: add a reset
- cascading counters: when Q0~Q3=1 => enable Q4~Q7 (4-bit -> 8-bit)
- shift registers: Ser_in -> Q0, Q_n -> Q_n+1, four modes: hold, shift left/right, parallel in.
	- parallel/serial conversion
	- **ring counter** (no decoding needed, onehot)
	- n-bit -> n states
        - can be in invalid loops. Solution: only shift in a one when Q0 Q1 Q2 are 0
- nth **jonhson counter**(decode easy)
    - shift register + inverter serie in
    - Q0=Qn', Q1=Q0,...Qn=Qn-1
    - can also be in invalid loop
        - solution: when Qn=Q0=0, load 0001 to resume
- Linear Feedback Shift Registers
#### RTL design
>**register-transfer level** (**RTL**) is a design abstraction which models a synchronous digital circuit in terms of the flow of digital signals (data) between hardware registers, and the logical operations performed on those signals.
- data vs. control bit
- register timing
	- what if input change on edge of clock cycle
    - this will be consider against hold time constraint
- Data path Design
	- Cashier
	- why do we need TREG?
		- to store the values of SREG, so SREG can keep updating without affecting display
	- at posedge, Q = LoadTotal, T_LD goes to 1, and TREG would update, how do we know that T_LD change before TREG got updated or vice versa?
- sequential design structure
- clock frequency too high => metastability
>[!faq] why do we need RTL?
### Timing analysis
![[Pasted image 20241030105903.png]]
![[Pasted image 20241030105835.png]]
- setup time: inorder the register can update
    - remember you have two latches in one flipflop
    - note that when clk is low, the leader will change, clk high the follower change
- hold time: inorder the register don't falsely update
- Q => D, combinational logic
    - propagation:
	- skew time: time for the clock to get from one ff to the next ff
    - $\delta_n = min(\delta_i + t_p^{i\to n})$
    - $\Delta_n = max(\Delta_i + t_p^{i\to n})$
    - $a_0=min(t_p^{C\rightarrow Q}+\delta_p^{Q_0\rightarrow D_0},t_p^{C\rightarrow Q} + \delta_p^{Q_1\rightarrow D_0})$ the fastest signal
    - $A_0=max(t_p^{C\rightarrow Q}+\Delta_p^{Q_0\rightarrow D_0},t_p^{C\rightarrow Q} + \Delta_p^{Q_1\rightarrow D_0})$ the slowest signal
    - do note that a0 denote the first arriving signal into D_0
- departure time from s(assuming reg s -> reg t): $d_s = t_p^{C_s\rightarrow Q_s}$
- condition to satisfy (to avoid timing issue)
    - $a\ge H$
    - $P\ge A+S$
- timing waveform:
![timingwaveform.png|300](assets/imgs/timingwaveform.png)
- asychronous set/reset
    - must also be synchronized with system clock

### Multiplication
- priliminary:
- version 1.0, *just* to save space:
![[Pasted image 20241204111820.png]]
- note that n bit * nbit never exceed 2n bits
- Sequential Multiplier V2.0 - signed multiplication
    - arithematic shift (sign bit unchanged rf. sign extension)
    - state diagram
    - overflow: msb' = ovf $\oplus$ msb

| mi  | mi-1 | mi-1 - mi | Action      |
| --- | ---- | --------- | ----------- |
| 0   | 0    | 0         | NOP         |
| 0   | 1    | 1         | Add Q       |
| 1   | 0    | -1        | Substract Q |
| 1   | 1    | 0         | NOP         |

![statediagram_lec17.png](assets/imgs/statediagram_lec17.png)


### Tabular Generation of Prime Implicants
Procedure:
- Arrange product terms in groups such that all terms in one group have
the same number of 1s in their binary representation, their Group Index
- Starting from minterms, arrange groups in ascending-index order
- Apply adjacency theorem to product terms from adjacent groups only
- Remove subsumed product terms using absorption theorem
- upgrade to the k-map algo(when number of vars goes too high)
summary:
1. continuously(and recursively) merging terms(touching levels)
    - EPIs:A prime implicant that covers a minterm that is not covered by any other **prime implicants**
    - Prime inplicant:01 11 01 11, but not EPI
2. take PIs and get EPIs(in a table)
    - can we use covering function to reduce some calculation?(what would return by the covering function-==check this==)
---
### State minimization(of state assignment)
- safe design: all unused states assigned **a** used state
- Efficient Design: Treat the next-states and outputs of unused states as don’t cares
	- may never return to correct states
- state cluster:assigns unused states to behave like used states
- procedure of identifying equivalent state
- one-hot encoding: heuristically decrease(more d's) the literal cost
![[Pasted image 20241125111256.png]]
- BD equivalent iff BD, so they are equivalent (green check mark $\checkmark$)
- the dependent states(BC) not equivalent => the dependee(DF) not equivalent
- What if A =B -> C = D & C= D -> A=B? **They are equivalent**, you can think of them to be a cycle in the 


### carry look ahead adder
- methodology: we can "look ahead just with $x_i,y_i$"
    - ripple adder problem: too deep the circuit ($c_i$)
    - sol idea1: do 2 level optimization on x_{i-1}...x_0
        - not good: hard to scale
    - CLA(carry look ahead adder): 
        - either generated high part or generated in low part and propagated across low and high part
        - equation(2-bit): 
            - $G_{H,L}=G_H+P_HG_L$
			- equivalently, $G_{j,i}=G_{j,k+1}+P_{j,k+1}G_{k,i}$
            - $P_{H,L}=P_HP_L$
			- equivalently, $P_{j,i}=P_{j,k+1}P_{k,i}$
        - recursive computation (parallel prefix) -> with c_0, get all c_i's
			- $c_{j+1} = G_{j,i}+ P_{j,i}c_i$
			- $c_{i+4}=g_{i+3}+p_{i+3}g_{i+2}+p_{i+3}p_{i+2}g_{i+1}+p_{i+3}p_{i+2}p_{i+1}g_{i}+p_{i+3}p_{i+2}p_{i+1}p_{i}c_i$
			- so $G_{i+3, i}$ is the part before multiply ci
        - $log_2n$ depth of circuit
#### timing analysis
- possible to have more significant carry come out first
- equation (4-bit)
![[Pasted image 20241212175856.png]]

- take the maximum of the gates(since we don't actually know which will be the determining one)
- to calculate each G and P, just add the previous layers' and the gates' delay

---
#### \*verilog tips
- REG and WIRE and `genvar`
You must also declare a data type for all outputs. Verilog has two data types—wire and
reg. A wire cannot hold state and is always evaluated in terms of other values. A reg
(short for register) will hold the last value assigned to it until another assignment changes
its value
	- why do we need these?
	- cause it is **hardware!**
	- from my experience `reg` is used in test bench to give it an initial value. `wire` is used as like an intermediate state holder. `genvar i` is the inital declaration of a loop variable i. In a test bench input use `reg` output use `wire`

- usefull syntax: tenary operator
```verilog
assign D2 = (Digit2 == 'd0) ? OFF : LUT[Digit2];
Always @*
	D<=Q
```
Meaning anytime Q change do a refreshment
