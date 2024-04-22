![image](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/f3bcfa4d-ef06-4ab4-acbd-e4609305f661)

Source:
```
from Crypto.Cipher import AES


KEY = ?


class StepUpCounter(object):
    def __init__(self, step_up=False):
        self.value = os.urandom(16).hex()
        self.step = 1
        self.stup = step_up

    def increment(self):
        if self.stup:
            self.newIV = hex(int(self.value, 16) + self.step)
        else:
            self.newIV = hex(int(self.value, 16) - self.stup)
        self.value = self.newIV[2:len(self.newIV)]
        return bytes.fromhex(self.value.zfill(32))

    def __repr__(self):
        self.increment()
        return self.value



@chal.route('/bean_counter/encrypt/')
def encrypt():
    cipher = AES.new(KEY, AES.MODE_ECB)
    ctr = StepUpCounter()

    out = []
    with open("challenge_files/bean_flag.png", 'rb') as f:
        block = f.read(16)
        while block:
            keystream = cipher.encrypt(ctr.increment())
            xored = [a^b for a, b in zip(block, keystream)]
            out.append(bytes(xored).hex())
            block = f.read(16)

    return {"encrypted": ''.join(out)}
```

Kiến thức:

Đọc thêm về CTR tại [đây](https://nguyenquanicd.blogspot.com/2019/10/aes-bai-6-cac-che-o-ma-hoa-va-giai-ma.html)

Phân tích:
- Ở phần Counter, do `step_up=False` => step_up = 0 => self.stup do đó phân bộ đếm giảm sẽ là -0 và cả hệ thống mã hoá sẽ dùng cùng 1 keystream từ đầu chí cuối.
- Vì plaintext ^ keystream = ciphertext => plaintext = ciphertext ^ keystream, hay nói cách khác, chúng ta cần tìm đươcn keystream để tìm ra flag.
- Google thêm về file PNG (do flag là file png) ta có https://en.wikipedia.org/wiki/PNG
  ![image](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/3be95897-6937-4bd8-bf3f-5cc4502d37eb)

Ta thấy 16 bytes đầu của png sẽ thường là "89504E470D0A1A0A0000000D49484452"
Suy ra: keystream = "89504E470D0A1A0A0000000D49484452" ^ ciphertext

Code python:
```
import requests
from pwn import xor

def encrypt():
    url = 'https://aes.cryptohack.org/bean_counter/encrypt'
    r = requests.get(url)
    return r.json()["encrypted"]

png_sign = bytes.fromhex('89504E470D0A1A0A0000000D49484452')
cipher = bytes.fromhex(encrypt())

keystream = xor(png_sign, cipher[:16])
if len(keystream) == 16:
    plain = xor(keystream, cipher)
    image = open('flag.png', 'wb').write(plain)
```
Sau đó mở file flag.png lên là có flag: `crypto{hex_bytes_beans}`
![flag](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/c2416182-4e4a-4dae-b58e-1323b9f493fd)

