<img width="762" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/139d2862-c2ea-47bd-818a-575341e2b7d5">

Đề bài yêu cầu ta sử dụng [giải thuật Euclid mở rộng](https://vnoi.info/wiki/translate/he/So-hoc-Phan-1-Modulo-gcd.md) để tìm x và y.
Nghiên cứu giải thuật Euclid, ta viết được đoạn code python để giải bài như sau:
```
#!/usr/bin/env python3

def extendedEuclid(p,q):
        if q == 0:
                return (p, 1, 0)
        else:
                (gcd, x, y) = extendedEuclid(q, p%q)
                return (gcd, y, x - (p // q)*y)
p = 26513
q = 32321

gcd, x, y = extendedEuclid(p, q)
print ("gcd = {}".format(gcd))
print ("x = {}".format(x))
print ("y = {}".format(y))
```
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/a66d029d-2b09-4d7f-a380-eea129ba7247">

Chạy đoạn code vừa viết, thu được hai số đề bài yêu cầu:
x = 10245
y = -8404
Nhập số nhỏ hơn là `-8404` theo yêu cầu
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/98703feb-9177-47ad-ae79-fc6657ba50f8">
