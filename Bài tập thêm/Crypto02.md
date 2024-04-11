Ciphertext: `50416610a7cb3f51c4c24b6e1401e8652671465aa7cb3f51c4c24b6e1401e865603c141da7cb3f51c4c24b6e1401e86526561519a7cb3f51c4c24b6e1401e8654a381034a7cb3f51c4c24b6e1401e8657b391234a7cb3f51c4c24b6e1401e865203a461ea7cb3f51c4c24b6e1401e865673a7a58a7cb3f51c4c24b6e1401e8657b395052a7cb3f51c4c24b6e1401e8657d285866a6ca3e50c5c34a6f1500e964`

Source:
```

from Crypto.Util.Padding import pad
import os

FLAG = b"EHC{REDACTED}"
KEY = os.urandom(16)

flag = []
for i in range(0, len(FLAG), 4):
    flag.append(pad(FLAG[i:i+4], 16))
FLAG = b''.join(flag)

def encrypt_flag(flag, key):
    res = []
    for i in range(len(flag)):
        res.append(flag[i] ^ key[i % len(key)])

    hex_flag = ''.join(format(c, '02x') for c in res)

    return hex_flag

encrypt = encrypt_flag(FLAG, KEY)

print(encrypt)
```

Phân tích: 

Đây là một dạng mã hoá đơn giản, đầu tiên tách flag thành các phần tử 4 ký tự, sau đó pad lên thành block 16 bytes rồi encrypt bằng cái xor với key. Ciphertext được chuyển đổi sang dạng hex rồi in ra.

Để bẻ khoá bài này, ta sẽ lợi dụng đặc tính của phần padding. Theo đó, phương thức pad mặc định của hàm pad trong thư viên Crypto là [pkcs](https://www.ibm.com/docs/en/zos/2.4.0?topic=rules-pkcs-padding-method)
Từ đó ta có thể dự đoán được block dữ liệu đầu tiên được đưa vào mã hoá là 'EHC{\x0c\x0c\x0c\x0c\x0c\x0c\x0c\x0c\x0c\x0c\x0c\x0c'

Theo tính chất của phép xor, ta có: 'EHC{\x0c\x0c\x0c\x0c\x0c\x0c\x0c\x0c\x0c\x0c\x0c\x0c' ^ ciphertext = key

Từ đó ta dễ dàng tính được key = '50416610a7cb3f51c4c24b6e1401e865' ^ '4548437b0c0c0c0c0c0c0c0c0c0c0c0c' = '1509256babc7335dc8ce4762180de469'

Lời giải:

Code python:
```
from Crypto.Util.Padding import unpad
from pwn import xor

key = bytes.fromhex('1509256babc7335dc8ce4762180de469')
flag = bytes.fromhex('50416610a7cb3f51c4c24b6e1401e8652671465aa7cb3f51c4c24b6e1401e865603c141da7cb3f51c4c24b6e1401e86526561519a7cb3f51c4c24b6e1401e8654a381034a7cb3f51c4c24b6e1401e8657b391234a7cb3f51c4c24b6e1401e865203a461ea7cb3f51c4c24b6e1401e865673a7a58a7cb3f51c4c24b6e1401e8657b395052a7cb3f51c4c24b6e1401e8657d285866a6ca3e50c5c34a6f1500e964')

encrypt = xor(flag, key)
plain = b''
for i in range (0, len(encrypt), 16):
    plain += unpad(encrypt[i:i+16], 16)
#flag = unpad_bytes_array(encrypt)
print(plain)
```
Thu được flag: `EHC{3xc1u51v3_0r_15_n07_53cur3_3n0u9h!}`

![image](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/319bc093-ff6a-499c-a71c-36ab3b62f8b8)
