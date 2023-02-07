## Project 
As a computer engineer, you are ask to design a 4-bit ALU that can perform the following
operations:
<br>

## Analysis

Designing a 4 BIT ALU though possible, will be such a tedious task to do. So let us use a
more tactical approach to do so. By using the principle of Divide and Conquer, splitting the 4 bit
ALU into smaller components and assembling them at the end, achieving a 4 bit ALU will be made
easier and simpler to implement. So the principle would be to create a 1 BIT ALU, and then cascade
4 of them to get the 4 BIT ALU.
<br>

Note that the ALU is able to perform 8 main operations, so the need of 3 SELECTION BITS that
would provide a basis for choosing the expected result.
<br>

## For the Arithmetic unit (AU): <br>
 Addition<br>
 Subtraction<br>
 Increment input A<br>
 Decrement input A<br>
<br>

## For the logic unit (LU)<br>
 ANDing<br>
 ORing<br>
 Complement input A<br>
 XORing<br>
 Comparator<br>

Solution. <br>
Inputs;
### A = A3 A2 A1 A0 <br>
### B = B3 B2 B1 B0<br>
### S = S2 S1 S0<br>
Outputs;<br>

### FAU – Arithmetic unit output<br>
### FLU – Logic unit output<br>
### F -- Final output<br>

# I- CONCEPTION OF THE ARITHMETIC UNIT
The AU is able to perform the 4 Mathematical operations which are:<br>
1- Addition ie A+B<br>
2- Subtraction ie A-B<br>
3- Increment ie A+1<br>
4- Decrement ie A-1<br>

But the computer uses only addition to perform any operation so how then
can we transform the additive function?<br>
In 2’s complement notation, we have: <br>
Addition A+B+0<br>
Subtraction A+ 1’s (B) +1<br>
Increment A+0+1<br>
Decrement A+ 1+0<br>


Basically, the following table shows us how an Additive function of three bits operates
A B S2 S1 S0 F(A,B)<br>
X X 0  0  0   A + B<br>
X X 0  0  1   A – B<br>
X X 0  1  0   A + 1<br>
X X 0  1  1   A – 1<br>
X X 1  0  0   A ^ B<br>
X X 1  0  1   A v B<br>
X X 1  1  0  -A<br>
X X 1  1  1   A B<br>
X X X X X     A>B A<B A=B<br>
ARITHMETIC UNIT && LOGIC UNITAddition
A B Cin S Cout
0 0 0 0 0
0 0 1 1 0
0 1 0 1 0
0 1 1 0 1
1 0 0 1 0
1 0 1 0 1
1 1 0 1 1
1 1 1 1 1
S= ∑m(1,2,4,6,7)
Cout=∑m(3,5,6,7)
K MAP of S
Cin\A B 00 01 11 10
0 1 1 1
1 1 1
S= (A B) Cin
K MAP of Cout
Cin\A B 00 01 11 10
0 1
1 1 1 1
S= Cin(A+B)+AB
So to get subtraction, decrementation and incrementation, all we have to do is to change
the mode bit (Cin) and the entries of B to get the appropriate result.
Now, we need a component Y GEN that would select if it’s either B, 1’s (B), 0 or 1 that is to
come out with respect to the users selection code.
S1 S0 Y0 Cgen
0 0 Bi 0
0 1 1’s(Bi) 1
1 0 0 1
1 1 1 0
S1 S0 Bi Y0
0 0 0 0
0 0 1 1
0 1 0 1
0 1 1 0
1 0 0 0
1 0 1 0
1 1 0 1
1 1 1 1
K MAP OF Y GEN
S1 S0 \Bi 0 1
00 1
01 1
11 1 1
10
Y= S0 1’s (S1) BiWe need a Component Cgen that will be able to select if the mode bit would be 0 or 1
From the table above, one can conclude that Cgen=S1 S0
So, our AU will be the following,II- CONCEPTION OF THE LOGIC UNIT
The LU is able to perform the following operations:
1- ANDing ie A^B
2- ORing ie AvB
3- Negation ie 1’s(A)
4- XORing ie A B
Since all of these operations are performed at the same time, a 4-1 MUX is required. Recall that a
Multiplexer takes n selector, 2 exp (n) inputs and gives only 1output
S1 S0 Z
0 0 I0
0 1 I1
1 0 I2
1 1 I3
From the table above: Z=1’s (S1) 1’s (S0) I0 + 1’s(S1) S0 I1 + S1 1’s(S0) I2 + S1 S0 I3
NB: Note from table 1 above that, whatever the combination S1 and S2 both an AU operation and
LU operation are performed. Thus, we need a last selector bit to decide which of the result will be
outputted. Thus, the need of a 2-1 MUX.
S1 Z
0 I0
1 I1
From the table above: Z=1’s (S0) I0 + S0 I1
K MAP OF 2-1 MUX
I1 I0 \S0 0 1
00
01 1
11 1 1
10 1III- COMPARATOR
Since our ALU works in 4-bits, it is expected to output if A<B, A>B or A=B. Let us work
from the idea of creating a two 2-bit comparator and then cascading them to get our 4-bit
comparator.
4 – BIT COMPARATOR
Inputs Outputs
A1 A0 B1 B0 G E L
0 0 0 0 0 1 0
0 0 0 1 0 0 1
0 0 1 0 0 0 1
0 0 1 1 0 0 1
0 1 0 0 1 0 0
0 1 0 1 0 1 0
0 1 1 0 0 0 1
0 1 1 1 0 0 1
1 0 0 0 1 0 0
1 0 0 1 1 0 0
1 0 1 0 0 1 0
1 0 1 1 0 0 1
1 1 0 0 1 0 0
1 1 0 1 1 0 0
1 1 1 0 1 0 0
1 1 1 1 0 1 0
G = ∑ m(4,8,9,12,13,14)
A1 A0\B1 B0 00 01 11 10
00
01 1
11 1 1 1
10 1 1
G=A1 1’s (B1) + A0 1’s (B1) 1’s(B0) + A1A0 1’s(B0)
L= ∑ m (1,2,3,6,7,11)
A1 A0\B1 B0 00 01 11 10
00 1 1 1
01 1 1
11
10 1
L= 1’s (A1) B1 + 1’s (A0) B1 B0 + 1’s(A1) 1’s(A0)
E= ∑ m(0,5,10,15) B0
A1 A0\B1 B0 00 01 11 10
00 1
01 1
11 1
10 1
E = (1’s (A0 B0) ) (1’s(A1 B1) )1 – BIT ALU
Cascading four 1 bit ALU with the Comparator
Finally, our 4 bit ALU managing all the operations is seen on FIGURE 1 Above