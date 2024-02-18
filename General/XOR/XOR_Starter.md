<img width="759" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/65ba133f-28ff-4fe5-bd94-933f48a8ac17">

Đề bài yêu cầu ta phải chuyển từng ký tự trong string `label` thành số nguyên rồi xor với 13, sau đó chuyển lại thành string rồi in ra màn hình.
Code python theo yêu cầu:
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/5f03c9ac-e33d-4fed-8134-726df3604ee8">
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
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/7f3436a6-f12d-4c20-b7ca-7fd77fafed5d">
