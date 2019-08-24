# Bit-Stuffing-Math
The Math Behind Bit Stuffing

## Problem
We are encoding a packet data as a series of bits, for example: `0101000101`. We want to denote the end of the packet with a "flag" of size J in the form of 0 followed by J 1s followed by a 0. So a flag with a flag size of 3 would be: 01110. To make sure no false flags appear in the packet, we insert a 0 after every J-1 occurences of a 1. To decode the packet after every J-1 occurences of a 1, we remove the following 0.

Our task is to find the optimal flag length if the expected size of a packet is `E[K]`

## Solution
We assume `P(0) = 1/2` and `P(1) = 1/2`

The probability of any bit being preceded by a 0 and being the first 1 in a sequence of `j-1` 1s is `P(0)P(1)^(j - 1)=(1/2)^j`. This does not apply to the very first bit (because it cannot be preceded by anything) and the final j-1 bits (they cannot be followed by `j-1` 1s), but as `j` is relatively small compared to `E[K]`, we'll consider the probabilistic differences caused by these bits to be negligable.


Due to indepence between bits, the expected number of stuffed bits is `E[Stuffed Bits] ~= E[K]P(0)P(1)^(j - 1)`, so 
`E[Stuffed Bits and Ending Flag] ~= E[K]P(0)P(1)^(j - 1) + j + 2`

To find the smallest optimum flag length
```
E[Stuffed Bits and Ending Flag] ~= E[K](1/2)^j + j + 2
Now we compare a flag of length j to that of j+1. We want to find the smallest flag size where increasing the flag size increases the total bits.
E[K](1/2)^j + j + 2 < E[K](1/2)^(j + 1) + (j+1) + 2
E[K](1/2)^j < E[K](1/2)^(j + 1) + 1
E[K](1/2)^j - E[K](1/2)^(j + 1) <  1
2E[K](1/2)^(j+1) - E[K](1/2)^(j + 1) < 1
E[K](1/2)^(j + 1) <  1
(1/2)^(j + 1) < 1/E[K]
log((1/2)^(j + 1)) < log(1/E[K])
-(j+1) < log(1/E[K])
-(j+1) < -log(E[K])
j > log(E[K]) - 1
```

As increasing the flag size increases the total bits, we want to minimize the flag size `j`. Therefore `log(E[k]) - 1` is the optimum flag size.

## What about Calculus?

This is much harder to do with calculus. Consider this: we want to minimize the expected total number of bits. So we want to minimize the function `E[K](1/2)^j + j + 2` with respect to j. The derivative of the function would be `E[K]log(1/2)(1/2)^j + j + 2`. Setting that equal to 0, we get a weird, yucky expression that we don't actually want to solve (and solving it would give yucky results). So, the first solution is easier and more intuitive than the calculus one.
