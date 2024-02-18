<img width="759" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/5c0a1ed1-6d0d-42a9-b947-1af753fee061">

Sau khi cài các thư viện cần thiết như trong hướng dẫn, ta viết chương trình python để decode:
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/509a8f23-1190-43d8-9cb6-922eb0aed951">

```
!/usr/bin/env python3

from Crypto.Util.number import *

cipher = long_to_bytes(11515195063862318899931685488813747395775516287289682636499965282714637259206269)
print(cipher)
```
Chạy chương trình vừa viếtm thu được flag: `b'crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}'`
<img width="251" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/cda72897-8eaa-4233-8127-ce80b6a2310d">


