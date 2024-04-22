<img width="759" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/65ba133f-28ff-4fe5-bd94-933f48a8ac17">

Đề bài yêu cầu ta phải chuyển từng ký tự trong string `label` thành số nguyên rồi xor với 13, sau đó chuyển lại thành string rồi in ra màn hình.
Code python theo yêu cầu:

```
#!/usr/bin/env python3

cipher = 'label'
num = 13
encoded = [ord(c) for c in cipher]
xor = [num ^ e for e in encoded]
plain = "".join(chr(x) for x in xor)
flag = "crypto{" + plain + "}"
print(flag)
```
Chạy code và ta có dc flag: `crypto{aloha}`

