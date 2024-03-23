<img width="679" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/97830304-4639-4807-9da8-e1228bc682e2">

Đề bài cho ta đoạn code python rsa:
```
#!/usr/bin/env python3

from Crypto.Util.number import getPrime, inverse, bytes_to_long, long_to_bytes, GCD

e = 3

# n will be 8 * (100 + 100) = 1600 bits strong which is pretty good
while True:
    p = getPrime(100)
    q = getPrime(100)
    phi = (p - 1) * (q - 1)
    d = inverse(e, phi)
    if d != -1 and GCD(e, phi) == 1:
        break

n = p * q

flag = b"XXXXXXXXXXXXXXXXXXXXXXX"
pt = bytes_to_long(flag)
ct = pow(pt, e, n)

print(f"n = {n}")
print(f"e = {e}")
print(f"ct = {ct}")

pt = pow(ct, d, n)
decrypted = long_to_bytes(pt)
assert decrypted == flag
```
Cùng ciphertext:
```
n = 742449129124467073921545687640895127535705902454369756401331
e = 3
ct = 39207274348578481322317340648475596807303160111338236677373
```
Sử dụng tool [factordb](http://factordb.com/index.php?query=) để tìm p và q, ta được:
p = 752708788837165590355094155871
q = 986369682585281993933185289261
Viết chương trình giải mã:
```
from Crypto.Util.number import *
n = 742449129124467073921545687640895127535705902454369756401331
e = 3 
ct = 39207274348578481322317340648475596807303160111338236677373
p =  752708788837165590355094155871
q =  986369682585281993933185289261
phi = (p - 1) * (q - 1)
d = pow(e, -1, phi)

plain = pow(ct,d,n)

print(long_to_bytes(plain))
```
Flag: `crypto{N33d_b1g_pR1m35}`
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/0030d518-770b-4cf5-b61e-d23c8912301e">

