<img width="680" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/3fa1c1a4-869d-4f73-b7f4-960e57a0478c">

Đề bài yêu cầu tìm số nguyên tố p,q biết:
```
N = p*q
c1 = (2*p + 3*q)**e1 mod N
c2 = (5*p + 7*q)**e2 mod N
```
Theo những gì google được, ta sẽ chứng minh cách tìm q (còn p thì p = N //q)

Có:
  
  $c_{1} = (a_{1} * p + b_{1} * q)^{e_{1}} \mod N$

  $c_{2} = (a_{2} * p + b_{2} * q)^{e_{2}} \mod N$

Suy ra:

  $c_{1}^{e_{2}} = (a_{1}p)^{e_{1}e_{2}} + (b_{1}q)^{e_{1}e_{2}} \mod N$
 
  $a_{1}^{-e_{1}e_{2}} * c_{1}^{e_{2}} = a_{1}^{-e_{1}e_{2}} * (a_{1}p)^{e_{1}e_{2}} + a_{1}^{-e_{1}e_{2}} * (b_{1}q)^{e_{1}e_{2}} \mod N$
  $a_{1}^{-e_{1}e_{2}} * c_{1}^{e_{2}} = p_{1}^{e_{1}e_{2}} + a_{1}^{-e_{1}e_{2} * (b_{1}q)^{e_{1}e_{2}}$
