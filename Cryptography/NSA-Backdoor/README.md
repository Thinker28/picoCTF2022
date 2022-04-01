# picoCTF2022: NSA-Backdoor

![Contest Date: 15.03.2022](https://img.shields.io/badge/Contest%20Date-15.03.2022-lightgrey.svg)
![Solve Moment: During The Contest](https://img.shields.io/badge/Solve%20Moment-During%20The%20Contest-brightgreen.svg)
![Score: 500](https://img.shields.io/badge/Score-500-brightgreen.svg)

## Description

> I heard someone has been sneakily installing backdoors in open-source implementations of Diffie-Hellman... I wonder who it could be... ;)


## Attached Files

- gen.py
- output.txt

## Summary

We use Pollard p-1 to factor our modulus *n*, then we can take the discrete log in the order of p(It's prime factor).

## Flag

```

```

## Detailed Solution

In gen.py, we see that prmies are getting generated in the same was as the Very Smooth challenge, hence we can use Pollard p-1 to factorize N.

```python
import math

import sympy

def pollard(n):
        a = 2
        i = 2
        while(True):
                a = pow(a,i,n)
                if i % 10000 == 0:
                        print(i)
                d = math.gcd((a-1), n)
                if (d > 1):
                        return d
                        break
                i += 1

n = 0x72bae3105c52d6ca470aa6d21b1a8a9f2208951ca6cd71d1b484e38095e0558b32d9db2f926771dc4a93b6deebaf64d2978f0f4efc8f49db5571959e214c900a4bed54fa235ee72cec66c85bca819ea3fb1b4e3dd70e940d9067eb3d0a6a4abf6c152d7d1a19d0833532048ec84754c95eb8055b7e3817e65aea897e3e2a29764af08589a6271721c863df2386ceb9eea4f208ed8f45f0628d5ec3afcc416ab3dda4071a9fca2166e87f14a9475b1711a0b4ccdefab041a7e2a7b418155aed4a1bbc343a0c1a8d9af479ff7e62765bfb5f1762aa66c4b06ce44b5681977e027428b32811c8c539f0c631178ed60a863176cdd1fd73ee9cbe14eaa5e7010443cd

num = n

ans = []

p = pollard(num)

ans.append(p)

q = int(num//p)

ans.append(q)
```
Now after doing some research, we get [Backdoor](https://crypto.stackexchange.com/questions/32415/how-does-a-non-prime-modulus-for-diffie-hellman-allow-for-a-backdoor) as a result. Now this article tells us that we can instead of doing discrete log in the field of N, we can do it in the field of a factor of N(Aka P or Q). Turns out, this will be the flag! So now we can utilize the discrete_log() function in sage for our advantage and convenience.
