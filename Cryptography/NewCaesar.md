# New Caesar
> Description: We found a brand new type of encryption, can you break the secret code? (Wrap with picoCTF{})\
Point: 60 points\
Author: madStacks

```
Ciphertext: mlnklfnknljflfmhjimkmhjhmljhjomhmmjkjpmmjmjkjpjojgjmjpjojojnjojmmkmlmijimhjmmj
```

#### File: new_caesar.py
```python
import string

LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]

def b16_encode(plain):
	enc = ""
	for c in plain:
		binary = "{0:08b}".format(ord(c))
		enc += ALPHABET[int(binary[:4], 2)]
		enc += ALPHABET[int(binary[4:], 2)]
	return enc

def shift(c, k):
	t1 = ord(c) - LOWERCASE_OFFSET
	t2 = ord(k) - LOWERCASE_OFFSET
	return ALPHABET[(t1 + t2) % len(ALPHABET)]

flag = "redacted"
key = "redacted"
assert all([k in ALPHABET for k in key])
assert len(key) == 1

b16 = b16_encode(flag)
enc = ""
for i, c in enumerate(b16):
	enc += shift(c, key[i % len(key)])
print(enc)
```

Hàm đầu tiên, b16_encode():
```python
def b16_encode(plain):
	enc = ""
	for c in plain:
		binary = "{0:08b}".format(ord(c))
		enc += ALPHABET[int(binary[:4], 2)]
		enc += ALPHABET[int(binary[4:], 2)]
	return enc
```
Hàm này chuyển đổi từng kí tự sang dạng binary, sau đó chia thành 2 phần 4bits rồi chuyển 2 phần đó thành decimal. Và lấy kí tự trong ALPHABET theo vị trí vừa được chuyển đổi.

Và các kí tự trong chuỗi chuyển đổi này sẽ không quá kí tự 'p' theo alphabet.

Hàm thứ hai này, shift():
```python
def shift(c, k):
	t1 = ord(c) - LOWERCASE_OFFSET
	t2 = ord(k) - LOWERCASE_OFFSET
	return ALPHABET[(t1 + t2) % len(ALPHABET)]
```
nhận từng character được trả về từ hàm b16_encode() cùng với từng kí tự trong key rồi trả về một kí tự trong chuỗi ALPHABET.
Đầu tiên lấy vị trí của kí tự của b16_encode() theo bảng mã ASCII từ đi vị trí của 'a' theo ASCII là 97, tương tự như vậy với các kí tự trong key, chia dư với độ dài của chuỗi ALPHABET(len=16), lấy giá trị trong ALPHABET theo giá trị được trả về.

Để xử lý hàm này cần phải cho chạy một vòng for để tìm được giá trị sao cho vị trí của nó trong ASCII trừ đi 97 cộng vị trí của key trong ASCII trừ 97, chia dư cho 16 thì được vị trí của kí tự cipher theo ALPHABET.

```python
assert all([k in ALPHABET for k in key])
assert len(key) == 1
```
Chúng ta biết được key có độ dài là 1 và là một trong các kí tự thuộc ALPHABET.

Để xử lý vẫn đề này chúng ta cần một bruteforce attack.

Công việc của chúng ta bây giờ là xử lý ngược lại công việc của tất cả các hàm.

### Source code tham khảo:
```python
import string
import numpy as np

LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]

enc = "mlnklfnknljflfmhjimkmhjhmljhjomhmmjkjpmmjmjkjpjojgjmjpjojojnjojmmkmlmijimhjmmj"

def inv_shift(enc, key):
    tmp = ""
    for i in range(len(enc)):
        indexs = ALPHABET.index(enc[i])
        indexs_key = ord(key[i % len(key)]) - LOWERCASE_OFFSET
        for i in range(16):
            if (indexs_key + i) % 16 == indexs:
                tmp += ALPHABET[i]
    return tmp

def inv_b16_encode(key):
    result = ""
    for j in range(0, len(enc), 2):
        bin_half = ""
        for i in range(2):
            num = ALPHABET.index(key[i + j])
            bin_half += np.binary_repr(num, width=4)
        result += chr(int(bin_half, 2))
    return result


for i in range(len(ALPHABET)):
    strings = inv_shift(enc, ALPHABET[i])
    print(inv_b16_encode(strings))
```

### Flag: picoCTF{et_tu?_a2da1e18af49f649806988786deb2a6c}.
