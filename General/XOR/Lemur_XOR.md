<img width="701" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/9d72fe9f-d6b0-486b-9b8d-f9f247b0fd77">

Đề bài cho ta hai ảnh `lemur.png` và `flag.png` đã được XOR với cùng một key, cùng yêu cầu là xor byte rgb của 2 ảnh.

Vì cả hai đều được xor với cùng một key, nên nếu ta xor 2 ảnh này với nhau thì key sẽ biến mất và ta sẽ thu được tấm ảnh ẩn.
Trước hết, ta sẽ tải 2 ảnh về máy:



Viết chương trình python để thực hiện:
```
  GNU nano 6.2                                                                          dcxor5.py *                                                                                  
#!/usr/bin/env python3

from PIL import Image  #Thu vien hinh anh
from pwn import *

lemur = Image.open("lemur.png")
flag = Image.open("flag.png")

plain_b = xor(lemur.tobytes(), flag.tobytes())
plain = Image.frombytes(flag.mode, flag.size, plain_b)

plain.save('plain.png')
```


Chạy file python vừa viết, thu được ảnh:


flag là `crypto{X0Rly_n0t!}`
