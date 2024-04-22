![image](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/759fd93a-5771-447e-983b-910faf458852)

Source:
```
from Crypto.Cipher import AES


KEY = ?
FLAG = ?


@chal.route('/symmetry/encrypt/<plaintext>/<iv>/')
def encrypt(plaintext, iv):
    plaintext = bytes.fromhex(plaintext)
    iv = bytes.fromhex(iv)
    if len(iv) != 16:
        return {"error": "IV length must be 16"}

    cipher = AES.new(KEY, AES.MODE_OFB, iv)
    encrypted = cipher.encrypt(plaintext)
    ciphertext = encrypted.hex()

    return {"ciphertext": ciphertext}


@chal.route('/symmetry/encrypt_flag/')
def encrypt_flag():
    iv = os.urandom(16)

    cipher = AES.new(KEY, AES.MODE_OFB, iv)
    encrypted = cipher.encrypt(FLAG.encode())
    ciphertext = iv.hex() + encrypted.hex()

    return {"ciphertext": ciphertext}
```
Kiến thức cần biết:
Trước hết, ta cần phải hiểu được cách thức hoạt động của mode OFB, tham khảo tại [đây](https://nguyenquanicd.blogspot.com/2019/10/aes-bai-6-cac-che-o-ma-hoa-va-giai-ma.html)
![image](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/8c9e4056-565d-468a-88b2-765125e0939e)

Trong mode OFB, ở block 1, sẽ dùng thuật toán mã hoá để mã hoá iv, nhận được output1, sau đó output1 này sẽ được xor với plain1 để có được cipher1.
Ở block tiếp theo sẽ lấy output1 để mã hoá (chứ không phải plain1 hay cipher1) và cứ thế đến khi hết plaintext.

Ta nhận thấy rằng trong mode OFB, thuật toán mã hoá chỉ mã hoá iv và các output phái sinh từ nó chứ không mã hoá cipher hay plaintext.

Phân tích:
Trong đề bài lần này, có 2 điểm ta cần chú ý:
- Thứ nhất, đề bài đã cho ta iv
- Thứ hai, trong mode OFB thì: output1 ^ plain1 = cipher1, tức là plain1 = cipher1 ^ output1. Từ đó, ta nhận ra nếu gửi cipher1 làm plaintext thì sẽ nhận được flag.

Lời giải:

Code python:
```
import requests
from Crypto.Cipher import AES
from Crypto.Util.number import *

def received():
	url = 'https://aes.cryptohack.org/symmetry/encrypt_flag/'
	r = requests.get(url)
	return r.json()["ciphertext"]

def send(plain,iv):
	url = 'https://aes.cryptohack.org/symmetry/encrypt/' + plain + '/' + iv
	r = requests.get(url)
	return r.json()

cipher = received()
iv = cipher[:32]
cipher = cipher[32:]

flag = bytes.fromhex(send(cipher, iv)["ciphertext"])
print (flag)
```
Thu được flag: `crypto{0fb_15_5ymm37r1c4l_!!!11!}`




