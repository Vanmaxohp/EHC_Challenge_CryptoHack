<img width="738" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/e38bb997-049e-4952-808d-30ab2756f74f">

Đề bài cho một flag được mã hoá thành một dãy hex, theo hướng dẫn của đề bài, ta viết chương trình python để giải mã:
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/e5a79f49-83d1-4f4b-9c5f-e94adf6a0db8">

```
#!/usr/bin/env python3

cipher = "63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d"
bytes = bytes.fromhex(cipher)
print(bytes)
```
Chạy chương trình vừa viết, ta thu được flag: `b'crypto{You_will_be_working_with_hex_strings_a_lot}'`
<img width="272" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/b2fdaeb2-224b-4c51-ad3e-fb5ef803bd83">

