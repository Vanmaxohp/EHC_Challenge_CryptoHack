<img width="802" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/4c188cfc-8eb7-4c40-91e8-d4803dbe30b0">

Đề bài cho ta biết cách mà AES hoạt động. Yêu cầu lần này là chuyển một ma trận về lại plaintext ban đầu, ta chỉ cần chuyển đổi theo ascii từng phần tử của ma trận là xong:
Sửa code python đề bài đã cho:
```
#!/usr/bin/env python3

def bytes2matrix(text):
    """ Converts a 16-byte array into a 4x4 matrix.  """
    return [list(text[i:i+4]) for i in range(0, len(text), 4)]

def matrix2bytes(matrix):
    """ Converts a 4x4 matrix into a 16-byte array.  """
    text = ''
    for i in range(len(matrix)):
        for j in range(4):
            text += chr(matrix[i][j])
    return text

matrix = [
    [99, 114, 121, 112],
    [116, 111, 123, 105],
    [110, 109, 97, 116],
    [114, 105, 120, 125],
]

print(matrix2bytes(matrix))
```

Chạy chương trình vừa viết, thu được flag: `crypto{inmatrix}`

<img width="960" alt="image" src="https://github.com/Vanmaxohp/EHC_Challenge_CryptoHack/assets/90485791/ba25290c-0174-4779-b8ef-f86740567c8f">
