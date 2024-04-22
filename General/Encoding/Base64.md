<img width="705" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/913fd7dd-b100-444e-bf30-8262394738df">

Theo hướng dẫn của đề bài, ta viết chương trình python để giải:


```
!/usr/bin/env python3

import base64

hex = "72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf"
byte = bytes.fromhex(hex)
flag = base64.b64encode(byte)
print(flag)
```
Chạy chương trình vừa viết, thu được flag: `b'crypto/Base+64+Encoding+is+Web+Safe/'`

<img width="249" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/2b60e22f-882e-448a-bd71-4dd2f72433e9">


