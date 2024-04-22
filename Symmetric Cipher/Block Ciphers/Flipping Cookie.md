![image](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/5e2092d8-e0fb-4dd6-9e1a-4bae4e2a5bc0)

Source:
```
from Crypto.Cipher import AES
import os
from Crypto.Util.Padding import pad, unpad
from datetime import datetime, timedelta


KEY = ?
FLAG = ?


@chal.route('/flipping_cookie/check_admin/<cookie>/<iv>/')
def check_admin(cookie, iv):
    cookie = bytes.fromhex(cookie)
    iv = bytes.fromhex(iv)

    try:
        cipher = AES.new(KEY, AES.MODE_CBC, iv)
        decrypted = cipher.decrypt(cookie)
        unpadded = unpad(decrypted, 16)
    except ValueError as e:
        return {"error": str(e)}

    if b"admin=True" in unpadded.split(b";"):
        return {"flag": FLAG}
    else:
        return {"error": "Only admin can read the flag"}


@chal.route('/flipping_cookie/get_cookie/')
def get_cookie():
    expires_at = (datetime.today() + timedelta(days=1)).strftime("%s")
    cookie = f"admin=False;expiry={expires_at}".encode()

    iv = os.urandom(16)
    padded = pad(cookie, 16)
    cipher = AES.new(KEY, AES.MODE_CBC, iv)
    encrypted = cipher.encrypt(padded)
    ciphertext = iv.hex() + encrypted.hex()

    return {"cookie": ciphertext}
```
Phân tích: 

- Đầu tiên, ta nhận thấy được cách hoạt động của web lần này là: mã hoá cookie có chứa "admin=False" với iv được tạo ngầu nhiên bằng AES mode CBC, điều kiện để lấy flag là gửi lại cookie và iv phù hợp để khi decrypt sẽ nhận được "admin=True", web sẽ trả về flag.
- Do ciphertext ban đầu có chứa cả iv: `ciphertext = iv.hex() + encrypted.hex()`, nên ta nghĩ đến việc lợi dụng iv này để tạo ra một input mới với đoạn "admin=True"
- Block đầu tiên của mode CBC sẽ xor 16 bytes đầu của cookie "admin=False;expi" với iv ban đầu (tạm gọi là iv0), nên ta chỉ cần gửi về web phần data gồm iv mới (sao cho khi xor với block đầu tiên sẽ ra "admin=True;expir" thay vì "admin=False;expi") và phần còn lại của ciphertext (từ byte thứ 17 trở đi)

Lời giải:

Tạm gọi plain ban đầu là plain0 = ""admin=False;expi", plain cần có là plain1 = "admin=True;expir"

Có: plain0 ^ iv0 => block 0 của ciphertext mà ta cần một iv mới iv1 sao cho khi decrypt block 0 => plain1 ^ iv1

hay: plain0 ^ iv0 = plain1 ^ iv1

=> iv1 = plain 0 ^ iv0 ^ plain1

Code python giải mã:
```import requests
import json
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
from Crypto.Util.number import *
from pwn import xor

def received():
	url = 'https://aes.cryptohack.org/flipping_cookie/get_cookie/'
	r = requests.get(url)
	return r.json()["cookie"]

def send(cookie,iv):
	url = 'https://aes.cryptohack.org/flipping_cookie/check_admin/' + cookie + '/' + iv.hex()
	r = requests.get(url)
	return r.json()

cipher = received()
iv0 = bytes.fromhex(cipher[:32])     #Vì mỗi bytes gồm 1 ký tự hex, chuyển về cùng dạng bytes để xor với plain

plain0 = b'admin=False;expi'
plain1 = b'admin=True;expir'

iv1 = xor(xor(iv0, plain0), plain1)

print (send(cipher[32:], iv1))
```
Nhận được flag: `crypto{4u7h3n71c4710n_15_3553n714l}`


