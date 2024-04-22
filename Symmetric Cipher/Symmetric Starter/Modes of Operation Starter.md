<img width="677" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/77779392-65d2-45b6-874c-60483d0ed61a">

Đề bài nói qua về các mode - các cách khác nhau để aes mã hoá một đoạn tin dài, sau đó cho ta một [website](https://aes.cryptohack.org/block_cipher_starter/) để thực hiện thử thách.

Nhìn qua một lượt, ta thấy website cung cấp cho ta suộc code cho viêc encrypt và decrypt, công cụ xor và côn g cụ chuyển đổi từ hex sang text và ngược lại.
Đầu tiên, ta bấm nút in ra ciphertext, nhận được: 
`{"ciphertext":"ecc66eb0f6359eb85cda93a63ad91a7852f6002466e25c62eaa82f576aea95de"}`
Vì đề bài đã cho sẵn phần code để decrypt, ta sử dụng luôn và nhận được một đoạn hex:
`{"plaintext":"63727970746f7b626c30636b5f633170683372355f3472335f663435375f217d"}`
Dán đoạn hex này vào tool chuyển thành text, ta có flag:
`crypto{bl0ck_c1ph3r5_4r3_f457_!}`

