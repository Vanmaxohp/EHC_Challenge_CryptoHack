<img width="760" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/9c10bb8c-09ec-4dff-92ba-3d13d23ffcd9">

Đề bài yêu cầu ta tính ước chung lớn nhất của 2 số bằng thuật toán Euclid.
Viết chương tình python:
```                                                                      
#!/usr/bin/env python3

def gcd(a, b):
    if (b == 0):
        return a;
    return gcd(b, a % b);

a = int(input("a = "))
b = int(input("b = "))
print("GCD = ", gcd(a, b))
```


<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/74c93f5a-7f83-406c-beb8-adb6ee0ca000">
Chạy chương trình:

<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/44452732-90d3-4a95-887e-d6d968f5822a">
