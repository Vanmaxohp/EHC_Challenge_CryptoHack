<img width="680" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/532ee614-323b-4cd2-858b-1310bb270091">

Vẫn là mã hoá XOR, thế nhưng đề bài lần này không cho ta gợi ý để đoán key nữa, vì vậy ta phải dùng cách khác để đoán key.
Biết rằng với tính chất của xor: plain ^ key = cipher thì plain ^ cipher = key. Ta sẽ lợi dụng đặc điểm của flag để tìm ra key.
Flag có dạng `crypto{...}` nên khi xor 7 byte đầu tiên của dãy hex với `crypto{` và xor byte cuối của dãy hex đã cho với `}` ta sẽ thu được key.

Viết chương trình python để thực hiện:
```
#!/usr/bin/env python3

from pwn import xor

enc = bytes.fromhex('0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104')
key = xor(enc[:7], 'crypto{') + xor(enc[-1], '}')
print(xor(enc, key))
```

<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/85c1bdec-6a30-4a8c-80c3-f08dcbe026d5">

Chạy đoạn code vừa viết, thu được flag: `crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}`

