<img width="801" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/a178a422-8a47-42b3-ba73-8729657aca19">

Đề bài cho ta kiến thức sơ bộ về [thặng dư bậc hai](https://tailieukcblog.files.wordpress.com/2017/09/thang-du-binh-phuong1.pdf) và yêu cầu ta tìm thặng dư bậc hai của module 29 trong tập hợp các số: 14, 6, 11.
Dựa vào dữ kiện của bài toán, ta viết chương trình python để giải:
```
#!/usr/bin/env python3

from Crypto.Util.number import *

p = 29
ints = [14, 6, 11]
root = []
for i in range(1, p):
    if (i ** 2) % p in ints:
        root.append(i)

print(min(root))
```
Chạy chương trình vừa viết, thu được số cần tìm theo đề bài: `8`
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/a4a1b23e-c493-4fbb-9cea-5983648d4781">
