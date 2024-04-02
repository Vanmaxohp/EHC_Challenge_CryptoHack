<img width="678" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/f1350496-fe14-44f5-bb3c-d8f63a29eed5">

Đề bài tiếp tục cung cấp cho ta một website để chơi, lần này đề bài nói qua về việc gen key.Trong bài lần này, key chính là mật khẩu được hash, khiến cho nó có thể bị tấn công.
Nhiệm vụ của ta là tấn công và lấy được flag.

Sử dụng nút in ra ciphertext, có:
`{"ciphertext":"c92b7734070205bdf6c0087a751466ec13ae15e6f1bcdd3f3a535ec0f4bbae66"}`
Source code:
```
from Crypto.Cipher import AES
import hashlib
import random


# /usr/share/dict/words from
# https://gist.githubusercontent.com/wchargin/8927565/raw/d9783627c731268fb2935a731a618aa8e95cf465/words
with open("/usr/share/dict/words") as f:
    words = [w.strip() for w in f.readlines()]
keyword = random.choice(words)

KEY = hashlib.md5(keyword.encode()).digest()
FLAG = ?


@chal.route('/passwords_as_keys/decrypt/<ciphertext>/<password_hash>/')
def decrypt(ciphertext, password_hash):
    ciphertext = bytes.fromhex(ciphertext)
    key = bytes.fromhex(password_hash)

    cipher = AES.new(key, AES.MODE_ECB)
    try:
        decrypted = cipher.decrypt(ciphertext)
    except ValueError as e:
        return {"error": str(e)}

    return {"plaintext": decrypted.hex()}


@chal.route('/passwords_as_keys/encrypt_flag/')
def encrypt_flag():
    cipher = AES.new(KEY, AES.MODE_ECB)
    encrypted = cipher.encrypt(FLAG.encode())

    return {"ciphertext": encrypted.hex()}
```

Đề bài sử dụng một mật khẩu đơn giản từ wordlist trên "https://gist.githubusercontent.com/wchargin/8927565/raw/d9783627c731268fb2935a731a618aa8e95cf465/words", để mã hoá. Tất nhiên ta không thể đoán được đề bài đã dùng password gì nên chỉ có thể bruteforce.
Đầu tiên ta tải file words về:
![image](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/95cfa569-b8ef-4ce4-87f5-ef5b66041ad2)

Tiếp theo, ta viết chương trình giải mã giữa trên chương trình mã hoá đề bài đã cho:
```
#!/usr/bin/env python3

from Crypto.Cipher import AES
import hashlib
import random

with open("words") as f:
    words = [w.strip() for w in f.readlines()]
keyword = random.choice(words)

KEY = hashlib.md5(keyword.encode()).digest()

l = len(words)

def decrypt(ciphertext, password_hash):
    ciphertext = bytes.fromhex(ciphertext)
    key = password_hash

    cipher = AES.new(key, AES.MODE_ECB)
    try:
        decrypted = cipher.decrypt(ciphertext)
    except ValueError as e:
        return {"error": str(e)}

    return decrypted

for i in range(l):
    KEY = hashlib.md5(words[i].encode()).digest()
    decrypted = decrypt("c92b7734070205bdf6c0087a751466ec13ae15e6f1bcdd3f3a535ec0f4bbae66", KEY)
    if b'crypto' in decrypted:
        print(decrypted)
```
Chạy chương trình vừa viết, thu được flag:
`crypto{k3y5__r__n07__p455w0rdz?}`

![image](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/5cc4fd2a-c103-40c9-b7aa-6f7ccea0a8e5)
