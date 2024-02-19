<img width="683" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/b01ef020-fc3d-4053-b906-6eb42b6e3092">

Đề bài cho ta một dãy hex với gợi ý rằng, flag đã được xor với một byte duy nhất để ra dãy hex này.
Do đó ta sẽ phải xor dãy hex trên với lần lượt các số có kích thước 1 byte (1 đến 16) để tìm ra flag.
Viết chương trình python:
```
#!/usr/bin/env python3

cipher = bytes.fromhex("73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d")
for i in range(17):
        flag = [c ^ i for c in cipher]
        print("".join(chr(o) for o in flag)); print("\n")
```
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/2359655b-5121-42b4-a83f-3230b0530e6c">

Chạy code vừa viết, thu được flag: `crypto{0x10_15_my_f4v0ur173_by7e}`
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/58d6721f-8b7b-4451-a7e6-3f0a2a61ca1f">
