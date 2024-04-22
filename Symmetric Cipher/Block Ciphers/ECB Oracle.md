![image](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/aae5b5aa-4147-41d4-930b-636e06d4e2a3)

Source: 
```
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad


KEY = ?
FLAG = ?


@chal.route('/ecb_oracle/encrypt/<plaintext>/')
def encrypt(plaintext):
    plaintext = bytes.fromhex(plaintext)

    padded = pad(plaintext + FLAG.encode(), 16)
    cipher = AES.new(KEY, AES.MODE_ECB)
    try:
        encrypted = cipher.encrypt(padded)
    except ValueError as e:
        return {"error": str(e)}

    return {"ciphertext": encrypted.hex()}
```
Giải: 
Điều chúng ta cần chú ý là dòng: `padded = pad(plaintext + FLAG.encode(), 16)`
Server sẽ cộng đầu vào với flag, sau đó mã hoá AES ECB với mỗi block gồm 16 bytes.
Vì mỗi block của ECB được mã hoá theo cùng một cách, nên ta sẽ brute force từng ký tự một của flag, ví dụ:

Ta gửi đi input là "000000000000000' (15 bytes '0')
Server sẽ cộng nó với flag: "00000000000000c" "rypto{something}"
Lấy 16 bytes đầu của output lưu vào 1 biến received1, sau đó gửi đi đoạn 15 bytes "0" + 1 ký tự ta biết trước (bruteforce), ví dụ "000000000000000a"
Lấy output này lưu vào biến tạm received2.

Khi so sánh received1 và received2, ta có thể biết được bytes cuối trong input mình vừa gửi có phải là ký tự trong flag hay không, lặp lại tương tự với các byte tiếp theo, ví dụ ở lần thử tiếp theo, ta gửi đi "00000000000000" (14 bytes "0") và "00000000000000ca" rồi cứ tiếp tục brite force như thế sẽ lần lượt có được các ký tự của flag.

Thế nhưng khi code chạy xong, ta chỉ nhận được một phần của flag: `crypto{p3n6u1n5_` chứng tỏ flag còn dài hơn, ta đổi tham số 16 thành 32 để tiếp tục, và nhận được flag hoàn chỉnh: `crypto{p3n6u1n5_h473_3cb}`
Code python:
```
import requests
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad

flag = 'crypto{'

def received(send):
    url = "http://aes.cryptohack.org/ecb_oracle/encrypt/" + send.encode().hex() + '/'
    r = requests.get(url)
    js = r.json()
    return bytes.fromhex(js["ciphertext"])

while flag[-1:] != '}':
    send = ""
    send += "0" * (31 - len(flag))
    print(send)
    received1 = received(send)[:32]
    send += flag
    for i in range(33, 128):   #Khoảng từ ký tự '!'(33) đến ký tự ' '(127) trong bảng ascii
        send = send[:31]
        send += chr(i)
        print(chr(i))
        
        received2 = received(send)[:32]
        if received1 == received2:
            flag += chr(i)
            print(flag)
            break
```


