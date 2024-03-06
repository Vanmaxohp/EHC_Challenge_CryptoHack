<img width="760" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/72feb05c-afd2-4570-8cc7-63aaa7512ec9">

Tiếp theo đến phần Round Keys, đề bài lần này cho ta thông tin chi tiếp về cách các bytes được xor, yêu cầu lần này là hoàn thành code python để xor, sau đó dùng function matrix2byte ở bài trước để lấy flag.

Code python:
```
#!/usr/bin/env python3

from pwn import xor

state = [
    [206, 243, 61, 34],
    [171, 11, 93, 31],
    [16, 200, 91, 108],
    [150, 3, 194, 51],
]

round_key = [
    [173, 129, 68, 82],
    [223, 100, 38, 109],
    [32, 189, 53, 8],
    [253, 48, 187, 78],
]


def add_round_key(s, k):
   return [[s2^k2 for s2, k2 in zip(s1, k1)] for s1, k1 in zip(s, k)]

def matrix2bytes(matrix):
    """ Converts a 4x4 matrix into a 16-byte array.  """
    text = ''
    for i in range(len(matrix)):
        for j in range(4):
            text += chr(matrix[i][j])
    return text

print(matrix2bytes(add_round_key(state, round_key)))
```

Chạy chương trình, thu được flag: `crypto{r0undk3y}`

<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/1ef53116-52e2-4359-b341-a60945194f2f">
