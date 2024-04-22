<img width="799" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/54a589f6-a302-48aa-a49a-2257e86ceaa7">

Đề bài yêu cầu ta giải bài toán [thặng dư trung hoa](https://vi.wikipedia.org/wiki/Định_lý_số_dư_Trung_Quốc)
Từ những gì tìm hiểu được, bài toán trên có nghiệm duy nhất theo modulo M = m1 * m2 *... mn
là x = a1 * M1 * y1 + a2 * M2 * y2 + ... + an * Mn * yn
Trong đó:
Mi = M / mi
yi = nghịch đảo (Mi) modulo mi  hay  yi * Mi = 1 mod mi

Từ cách giải trên, ta viết chương trình python để tính kết quả:
```
#!/usr/bin/env python3

from Crypto.Util.number import *

a = [2, 3, 5]
m = [5, 11, 17]
M = []
Mod = 1
x = 0
y = []

for i in range (3):
	Mod *= m[i]
print (Mod)
for i in range (3):
	M.append(Mod // m[i])
	y.append(inverse(M[i], m[i]))
print (M)
print (y)
for i in range (3):
	x += a[i] * M[i] * y[i]
print (x % Mod)
```
Chạy chương trình vừa viết, thu được kết quả: `872`

