# Easy Peasy
> Description: A one-time pad is unbreakable, but can you manage to recover the flag? (Wrap with picoCTF{})\
Point: 40 points\
Author: madStacks

```
nc mercury.picoctf.net 64260
```

### File: otp.py
```python
#!/usr/bin/python3 -u
import os.path

KEY_FILE = "key"
KEY_LEN = 50000
FLAG_FILE = "flag"


def startup(key_location):
	flag = open(FLAG_FILE).read()
	kf = open(KEY_FILE, "rb").read()

	start = key_location
	stop = key_location + len(flag)

	key = kf[start:stop]
	key_location = stop

	result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), flag, key))
	print("This is the encrypted flag!\n{}\n".format("".join(result)))

	return key_location

def encrypt(key_location):
	ui = input("What data would you like to encrypt? ").rstrip()
	if len(ui) == 0 or len(ui) > KEY_LEN:
		return -1

	start = key_location
	stop = key_location + len(ui)

	kf = open(KEY_FILE, "rb").read()

	if stop >= KEY_LEN:
		stop = stop % KEY_LEN
		key = kf[start:] + kf[:stop]
	else:
		key = kf[start:stop]
	key_location = stop

	result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), ui, key))

	print("Here ya go!\n{}\n".format("".join(result)))

	return key_location

print("******************Welcome to our OTP implementation!******************")
c = startup(0)
while c >= 0:
	c = encrypt(c)

```

Sau khi chạy lệnh netcat ta nhận được kết quả như sau:\
![2022-04-19_12-18](https://user-images.githubusercontent.com/77691959/164049574-2553ed12-2e34-4d05-b827-cf3cd41b048d.png)

Flag cần tìm đã bị encrypt qua hàm startup với tham số truyền vào là 0. Vậy nên chuỗi hex chúng ta nhận được khi chạy netcat chính là XOR char by char của flag và key. 

Và cũng cho chúng ta biết flag cần tìm dài 256bits.

Hàm này trả về của hàm startup() là độ dài của độ dài của flag, bằng 32. Đây cũng là tham số truyền vào của hàm encrypt() sau đó.

Bây giờ ta chỉ cần tìm được 32 kí tự đầu tiên của key_file sau đó XOR với chuỗi hex ở netcat thì đó chính là flag mình cần tìm.

Ở netcat, nó cho mình nhâp chuỗi bất kì để thực hiện hàm encrypt() với tham số đầu vào là 32. Chúng ta có key_file = 50000

```python
if stop >= KEY_LEN:
		stop = stop % KEY_LEN
		key = kf[start:] + kf[:stop]
	else:
		key = kf[start:stop]
```
Ở đây cho chúng ta biết rằng nếu nhập quá 50000 kí tự, thì key sẽ lấy lại từ đầu thêm vào để đủ lượng kí tự XOR với input. Vậy chúng ta có thể nhập vào 49968 kí tự để hàm encrypt() trả về 0 và dùng nó làm tham số truyền vào cho hàm encrypt() tiếp theo, sau đó nhập tiếp 32 kí tự để kết quả trả về của hàm encrypt() chính là kết quả trả về của hàm startup() với XOR flag và key. 

Vậy nên input là chuỗi các giá trị "\x00" và output nhận được chính là 32 kí tự đầu của key_file\
![2022-04-19_12-49](https://user-images.githubusercontent.com/77691959/164055106-2d80ee9a-b99e-4b03-9f48-6b0bb901e978.png)

Sau đó chúng ta chỉ cần XOR output này với chuỗi hex chúng ta có được ở netcat thì chúng ta đã có được flag.

![2022-04-19_12-50](https://user-images.githubusercontent.com/77691959/164055090-89f85bc0-d2f3-4f03-bb3d-a28543d8348d.png)

Commandline tham khảo: 
```
python3 -c "print('\x00'*(50000-32)+'\n'+'\x00'*32)" | nc mercury.picoctf.net 64260
```
'\n' ở đây là để nhập 32 kí tự ở lệnh tiếp theo đảm bảo cho hàm encrypt() đã trả về 0.

### Flag: picoCTF{3a16944dad432717ccc3945d3d96421a}.
