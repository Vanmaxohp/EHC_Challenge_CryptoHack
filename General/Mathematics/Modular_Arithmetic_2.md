<img width="799" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/fa51ed8a-87a0-467f-a80c-21238711e6b7">

Bài này yêu cầu ta giải quyết bài toán [Tính (a^b) % c](https://vnoi.info/wiki/translate/he/Number-Theory-3.md)
Từ những gì google được, ta viết được chương trình python giải bài toán trên:
```
#!/usr/bin/env python3

def sqr(x):
        return (x*x)
def powMod(a, b, c):
        if (b == 0):
                return (1 % c)
        else:
                if (b % 2 == 0):
                        return (sqr(a ** (b/2)) % c)
                else:
                        return (sqr(a ** (b/2)) % c)

a = int(input('a = '))
b = int(input('b = '))
c = int(input('c = '))
x = powMod(a, b, c)
print ("x = {}".format(x))
```
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/81aba22c-8565-4373-917f-e00aab2a5dee">

Chạy thử chương trình, nhâp vào các số theo đề bài hướng dẫn:
<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/95ff64f3-d724-448d-8721-42da0f4690e4">

Dễ thấy: a ** p mod p = a và a ** (p - 1) mod p = 1
Xem thêm: [Định lý nhỏ Fermat](https://vi.wikipedia.org/wiki/Định_lý_nhỏ_Fermat)
Kết quả bài toán là `1`
