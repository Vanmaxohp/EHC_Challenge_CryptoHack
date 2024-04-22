![image](https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/aac29c04-fc31-4d4a-ac10-c5940059c4fa)

Encoder:
```
from Crypto.Cipher import AES


KEY = ?
FLAG = ?


@chal.route('/ecbcbcwtf/decrypt/<ciphertext>/')
def decrypt(ciphertext):
    ciphertext = bytes.fromhex(ciphertext)

    cipher = AES.new(KEY, AES.MODE_ECB)
    try:
        decrypted = cipher.decrypt(ciphertext)
    except ValueError as e:
        return {"error": str(e)}

    return {"plaintext": decrypted.hex()}


@chal.route('/ecbcbcwtf/encrypt_flag/')
def encrypt_flag():
    iv = os.urandom(16)

    cipher = AES.new(KEY, AES.MODE_CBC, iv)
    encrypted = cipher.encrypt(FLAG.encode())
    ciphertext = iv.hex() + encrypted.hex()

    return {"ciphertext": ciphertext}
```
Mấu chốt của bài này là hiểu được cách hoạt động của CBC mode.
Trong CBC mode, vector khởi tạo (Initialization vector - iv) sẽ được xor với block đầu tiên (block 0) của plaintext rồi mới mã hoá, sau đó ciphertext của block 0 sẽ được xor với block 1 của plaintext rồi cứ thế...
Ta để ý thấy đề bài có cho luôn iv trong phần ciphertext: 
`ciphertext = iv.hex() + encrypted.hex()`
Nên bài này trở nên khá đơn giản, ta chỉ cần tách riêng ciphertext thành các phần, mỗi phần 16 bytes, phần đầu sẽ là iv, các phần sau lần lượt là các block của ciphertext, sau đó xử lý riêng từng block với ECB decrypt, rồi xor ngược lại là ra các block của plaintext.
Cuối cùng, ghép các block lại ta có được flag: `crypto{3cb_5uck5_4v01d_17_!!!!!}`

Code python:
```
import requests
from pwn import xor
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
from Crypto.Util.number import long_to_bytes, bytes_to_long

def decrypt(send):
	url = "http://aes.cryptohack.org/ecbcbcwtf/decrypt/" + send.hex() + '/'
	r = requests.get(url)
	js = r.json()
	return bytes.fromhex(js["plaintext"])

def encrypt():
	url = "http://aes.cryptohack.org/ecbcbcwtf/encrypt_flag/"
	r = requests.get(url)
	js = r.json()
	return bytes.fromhex(js["ciphertext"])


cipher = encrypt()

iv = cipher[:16]
cipher_block0 = cipher[16:32]
cipher_block1 = cipher[32:]

plain_block0 = xor(decrypt(cipher_block0), iv)
plain_block1 = xor(decrypt(cipher_block1), cipher_block1)
print(plain_block0 + plain_block1)
```
