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
picoCTF{e032a664}
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

```python
from Crypto.Util.number import *
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
c = 0x4790c71b682f70a3e8aeaeb62b7b5c7381b27ab013d806631efd826da0bfc4ea7f343ad33ea0abdd14762acf5fcdf02b3e44646b8df7b09345ec2c43614a15e4e38bda58bf0b08f643e521d04f4d1eb06a4521351533b4140df785f12fa085db1e14dba803f00a25208167b359045d4491a49463f2423894dc69d92fc814229bf3d439b0d552732363af89605fc5bc035612b68c49d01c5ec185028d3d036332f6d5d7bccc1e65c7fe13aefb3c8a4ebeb8006092cb714b9040ec3147c0ec784cb6e6cae2456999afdc8fcacd3f3d2502d29b59be9f47e5ff192512ff6a37cf12837f3da1a1905de2d5a4ae7eea353c1b0c15c764bb10a45a21cdb84c3bf948ef

num = n

ans = []

p = pollard(num)

q = int(num//p)

print("[*]Found factors p={}, q={}".format(p, q))

print(long_to_bytes(discrete_log(c, Mod(3, p))))
#[*]Found factors p=99755582215898641407852705728849845011216465185285211890507480631690828127706976150193361900607547572612649004926900810814622928574610545242732025536653312012118816651110903126840980322976744546241025457578454651121668690556783678825279039346489911822502647155696586387159134782652895389723477462451243655239, q=145188107204395996941237224511021728827449781357154531339825069878361330960402058326626961666006203200118414609080899168979077514608109257635499315648089844975963420428126473405468291778331429276352521506412236447510500004803301358005971579603665229996826267172950505836678077264366200199161972745420872759627
#b'picoCTF{e032a664}'
```
