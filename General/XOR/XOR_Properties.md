<img width="799" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/a5bd1416-b81a-4548-a0d2-f314ebc43fbe">

Đề bài giới thiệu cho ta sơ qua về lý thuyết và các tính chất của phép xor, cùng yêu cầu hãy tìm ra flag.

Ta biết rằng xor có tính chất:

nếu a xor b = c thì a xor c = b

Chứng minh:

có a xor b = c => a xor c = a xor (a xor b)
                          = (a xor a) xor b
                          = 0 xor b
                          = b

Theo đó, ta có hướng giải bài này như sau:

Có:
 
  KEY1 = a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313
  KEY2 ^ KEY1 = 37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e
  KEY2 ^ KEY3 = c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1
  FLAG ^ KEY1 ^ KEY3 ^ KEY2 = 04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf

Theo tính chất của phép xor, có:

FLAG = (FLAG ^ KEY1 ^ KEY3 ^ KEY2) ^ KEY1 ^ (KEY2 ^ KEY3)

Ta sẽ viết một chương trình python để tính ra FLAG dựa theo phép tính trên, sau đó chuyển dạng FLAG từ hex sang ASCII:
```
#!/usr/bin/env python3

ka = bytes.fromhex("a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313")
kb = bytes.fromhex("c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1")
kc = bytes.fromhex("04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf")
flag = [a ^ b ^ c for a, b, c in zip(ka, kb, kc)]
print("".join(chr(o) for o in flag))
```


Chạy code python ta thu dc flag: `crypto{x0r_i5_ass0c1at1v3}`


