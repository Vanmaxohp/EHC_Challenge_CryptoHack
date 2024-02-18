<img width="702" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/b4302886-8514-466d-9b2b-6b93b4b84b4b">

Theo đề bài, ta cần phải giải mã đoạn mã được mã hoá bằng file `13377.py` và ta được biết phần đầu của lời giải trong file `pwntools_example.py`
Tải cả 2 về xem thử
<img width="799" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/72137fb1-00a7-44a6-b156-97becdb0cdf8">
Đầu tiên là file `13377.py`
```
#!/usr/bin/env python3

from Crypto.Util.number import bytes_to_long, long_to_bytes
from utils import listener # this is cryptohack's server-side module and not part of python
import base64
import codecs
import random

FLAG = "crypto{????????????????????}"
ENCODINGS = [
    "base64",
    "hex",
    "rot13",
    "bigint",
    "utf-8",
]
with open('/usr/share/dict/words') as f:
    WORDS = [line.strip().replace("'", "") for line in f.readlines()]


class Challenge():
    def __init__(self):
        self.challenge_words = ""
        self.stage = 0

    def create_level(self):
        self.stage += 1
        self.challenge_words = "_".join(random.choices(WORDS, k=3))
        encoding = random.choice(ENCODINGS)

        if encoding == "base64":
            encoded = base64.b64encode(self.challenge_words.encode()).decode() # wow so encode
        elif encoding == "hex":
            encoded = self.challenge_words.encode().hex()
        elif encoding == "rot13":
            encoded = codecs.encode(self.challenge_words, 'rot_13')
        elif encoding == "bigint":
            encoded = hex(bytes_to_long(self.challenge_words.encode()))
        elif encoding == "utf-8":
            encoded = [ord(b) for b in self.challenge_words]

        return {"type": encoding, "encoded": encoded}

    #
    # This challenge function is called on your input, which must be JSON
    # encoded
    #
    def challenge(self, your_input):
        if self.stage == 0:
            return self.create_level()
        elif self.stage == 100:
            self.exit = True
            return {"flag": FLAG}

        if self.challenge_words == your_input["decoded"]:
            return self.create_level()

        return {"error": "Decoding fail"}


listener.start_server(port=13377)

```
Đọc thử source thì thấy rằng, thử thách này gồm 100 level, mỗi level sẽ là 1 string ngẫu nhiên được mã hoá bởi 1 trong 5 dạng base64, hex, rot13, bigint và utf-8, nhiệm vụ của ta là viết một chương trình python tự động giải mã những string được gửi về với dạng mã cho trước, sau đó gửi lại lên server để đế với level tiếp theo, khi giải được level 100 sẽ nhận được flag.

Tiếp theo mở file `pwntools_example.py` ra xem
```
from pwn import * # pip install pwntools
import json

r = remote('socket.cryptohack.org', 13377, level = 'debug')

def json_recv():
    line = r.recvline()
    return json.loads(line.decode())

def json_send(hsh):
    request = json.dumps(hsh).encode()
    r.sendline(request)


received = json_recv()

print("Received type: ")
print(received["type"])
print("Received encoded value: ")
print(received["encoded"])

to_send = {
    "decoded": "changeme"
}
json_send(to_send)

json_recv()
```
Đề đã cho ta code mẫu để nhận và gửi string, giờ ta sẽ viết chèn vào phần code để giải mã và in ra flag.
Trước hết, ta chạy thử file example để xem kết quả có dạng như thế nào:
<img width="443" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/e9fef306-104c-49fd-aea6-ebf14d89f83e">

Viết chèn đoạn code để giải mã vào trong file `pwntools_example.py`
```
#Gọi thêm các thư viện cần thiết
from pwn import * # pip install pwntools
import json
from Crypto.Util.number import bytes_to_long, long_to_bytes
import base64
import codecs
import random
from binascii import unhexlify


r = remote('socket.cryptohack.org', 13377, level = 'debug')

def json_recv():
    line = r.recvline()
    return json.loads(line.decode())

def json_send(hsh):
    request = json.dumps(hsh).encode()
    r.sendline(request)

def list_to_string(s):
    output = ""
    return(output.join(s))

#Viết code giải mã, cho lặp đến khi ra flag
while 1:
    received = json_recv()
    if "flag" in received:
        print("\n[*] FLAG: {}".format(received["flag"]))
        break

    cipher = received["encoded"]
    type = received["type"]

    if type == "base64":
        plain = base64.b64decode(cipher).decode('utf8').replace("'", '"')
    elif type == "hex":
        plain = (unhexlify(cipher)).decode('utf8').replace("'", '"')
    elif type == "rot13":
        plain = codecs.decode(cipher, 'rot_13')
    elif type == "bigint":
        plain = unhexlify(cipher.replace("0x", "")).decode('utf8').replace("'", '"')
    elif type == "utf-8":
        plain = "".join(chr(b) for b in cipher)

#Sửa change me thành biến chứa plaintext
    to_send = {
        "decoded": plain
    }

    json_send(to_send)
#xoá phần json_recv vì đã gán vào biến phía trên
```
Chạy bằng lệnh `python3 pwntools_example.py`
Thu được flag: `crypto{3nc0d3_d3c0d3_3nc0d3}`
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/7f2576b0-5977-4a66-96cc-ae4d3ab637ba">


![image](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/530d55e8-fb6e-4d8f-b830-5cba9edc9dc1)
