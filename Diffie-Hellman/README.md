# picoCTF2021: Diffie hellman

![Contest Date: 15.03.2022](https://img.shields.io/badge/Contest%20Date-15.03.2022-lightgrey.svg)
![Solve Moment: During The Contest](https://img.shields.io/badge/Solve%20Moment-During%20The%20Contest-brightgreen.svg)
![Score: 200](https://img.shields.io/badge/Score-200-brightgreen.svg)

## Description

> Alice and Bob wanted to exchange information secretly. The two of them agreed
to use the Diffie-Hellman key exchange algorithm, using p = 13 and g = 5. They
both chose numbers secretly where Alice chose 7 and Bob chose 3. Then, Alice
sent Bob some encoded text (with both letters and digits) using the generated
key as the shift amount for a Caesar cipher over the alphabet and the decimal
digits. Can you figure out the contents of the message?


## Attached Files

- message.txt

## Summary

We research about the Diffie-Hellman key exchange algorithm, and get our generated secret. Then we do a Caeser Cipher shift over the alphabet and the decimal digits with the shift value of the secret to get the flag.

## Flag

```
picoCTF{C4354R_C1PH3R_15_4_817_0U7D473D_84AA1DA8}
```

## Detailed Solution

Searching up the Diffie-Hellman key exchange algorithm in Google, one of the many first results we get is [Diffie-Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange). After seeing this, we see that what we're looking for is the value $s$. In this challenge, we are given $(g, p, a, b)$. We can start by computing $B$ which is $g ^ b$ *mod p* . Then our secret $s = B ^ a$ *mod p*. Now we can use this value as the shift in our Caeser Cipher. We can also use [dcode.fr](https://www.dcode.fr/shift-cipher) to do this. <p align="center">
![logo](https://github.com/Thinker28/picoCTF2021/blob/main/Diffie-Hellman/Screen%20Shot%202022-03-29%20at%205.28.56%20PM.png "Raspberry pi")
</p>
