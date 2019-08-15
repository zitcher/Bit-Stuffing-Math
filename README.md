# Bit-Stuffing-Math
The Math Behind Bit Stuffing

## Problem
We are encoding a packet data as a series of bits, for example: `0101000101`. We want to denote the end of the packet with a "flag" of size J in the form of 0 followed by J 1s followed by a 0. So a flag with a flag size of 3 would be: 01110. To make sure no false flags appear in the packet, we insert a 0 after every J-1 occurences of a 1. To decode the packet after every J-1 occurences of a 1, we remove the following 0.

Our task is to find the optimal flag length if the expected size of a packet is `E[K]`

## Solution
We assume `P(0) = 1/2` and `P(1) = 1/2`

Ignoring the first `K-1` bits The probability of any bit being the final 1 in a sequence of `j-1` 1s is `P(1)^(j - 1)`

Due to indepence between bits, the expected number of stuffed bits is `E[Stuffed Bits] ~= E[K]P(1)^(j)`

`E[Stuffed Bits and Ending Flag] ~= E[K]P(1)^(j) + j + 2`

To find the smallest optimum flag length
```
E[Extra Bits] ~= E[K](1/2)^j + j + 2
E[K](1/2)^j + j + 2 < E[K](1/2)^(j + 1) + (j+1) + 2
E[K](1/2)^j < E[K](1/2)^(j + 1) + 1 (edited) 
E[K](1/2)^j - E[K](1/2)^(j + 1) <  1
2E[K](1/2)^(j+1) - E[K](1/2)^(j + 1) <  1 (edited) 
E[K](1/2)^(j + 1) <  1
(1/2)^(j + 1) < 1/E[K]
log((1/2)^(j + 1)) < log(1/E[K])
-(j+1) < log(1/E[k])
-(j+1) < -log(E[k])
j > log(E[k]) - 1
```
