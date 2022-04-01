# Basic-mod2
> Description: We have so much faith in RSA we give you not just the product of the primes, but their sum as well!
* gen.py
* output.txt

#### Đầu tiên chúng ta tải về 2 file và xem nội dung của nó: 
### output.txt: là một file chứa các giá trị dạng hex x, n, e.
> x = 1c5d833516f25a832a331f349d2931d1577b3171d689da0391608dea7bbd9cda413d836db2f5c79da05755225c41af1cffbfaf1777b64abb521ac63e09d6101fe16fa7b98a647b94eccef0601681c34d4aa0ac9ded573f14460dc5dc5337d24bdf1f69325689346795adb0f9159cb2779463dce6e084adf861b61bd76bb160132\
n = c6c7c7879953a678d8e6d2ab85248f19b3f7c4b1e0c4c3f5bc1b63946abcac0cc19523386a08bbb5bd09321b9023df091f162dd0e9b100da1b5d15f78523ea6d7c6d7c7cd8b5c287fcd5d91defc53a32885c0a6f16f3b13221bbd4b5254bb9dbfe79244d343841485ad38fb139abfa3c3bd50e4787b1e882d21ada914989c1497774bdaa046ad2366028bd31f9277c39f58fe6fc78c247c4159b8879eaa7e15301ce937a7491e7727e5ae6e7852df6f9fd3367e5bb178c7013805a16ee68f6cdf8f5f72b2fbc159c38244082b1c47f5814a494ac7b310c37fe68a85e4448885d0de8f93d21106121ff74c0c6452ff697b2d2660483af13ce82ebdc0293b24dad\
c = 101ef1af3fd07a28858d5102e2448f29fd995f63df13b6e6a98d077e2330722af3374cd30652943fd1de006118024a4c86a23eae960b872e8d6c5735d73a05c40d039b6779b78f0fb90daf5011de05636b35a47416cb91712df3ca62f32bd2799b24d3b267a6140f98b07dfbb9e333bc71170776ce794f34674c232544df18e719698614958bbda4e371e58e22df63c2284f0f748af6ea0465f520ed8a70ba8d12307900216645b820c29a6297c1754a703a7caa1747ecf4d4bee49163366686ff15961db87f08007c302bde64c3e4dc165604a856b036c891ef4b0dd1fd9aec79f2a7d2d017c880c1a523d1d46868a99ee2b0046cacebe65da9a3ce3b7c9683

### gen.py: source code python cho chúng ta biết cách mà x, n, c được tạo ra như thế nào.
``` python
#!/usr/bin/python
from binascii import hexlify
from gmpy2 import mpz_urandomb, next_prime, random_state
import math
import os
import sys
if sys.version_info < (3, 9):
    import gmpy2
    math.gcd = gmpy2.gcd
    math.lcm = gmpy2.lcm
FLAG  = open('flag.txt').read().strip()
FLAG  = int(hexlify(FLAG.encode()), 16)
SEED  = int(hexlify(os.urandom(32)).decode(), 16)
STATE = random_state(SEED)
def get_prime(bits):
    return next_prime(mpz_urandomb(STATE, bits) | (1 << (bits - 1)))
p = get_prime(1024)
q = get_prime(1024)
x = p + q
n = p * q
e = 65537
m = math.lcm(p - 1, q - 1)
d = pow(e, -1, m)
c = pow(FLAG, e, n)
print(f'x = {x:x}')
print(f'n = {n:x}')
print(f'c = {c:x}')
```

#### Từ source code này chúng ta có thể thấy rằng p và q là 2 số nguyên tố được tạo ngẫu nhiên.
``` python
def get_prime(bits):
    return next_prime(mpz_urandomb(STATE, bits) | (1 << (bits - 1)))
p = get_prime(1024)
q = get_prime(1024)
```

* x là tổng của p và q: x = p + q
* n là tích của p và q: n = p * q
* e = 65537

## Solution:
#### Đầu tiên chúng ta cần chuyển x, n, c về dạng integer
``` python
decin = int(str(n), 16)
decix = int(str(x), 16)
decic = int(str(c), 16)
```
#### Sau đó tìm phi n: 
```python
phiN = decin-decix+1
```
#### Và cuối cùng là tìm d bằng cách tìm "modular inverse" của e và phiN

#### Và đây là source code: 
``` python
x = '1c5d833516f25a832a331f349d2931d1577b3171d689da0391608dea7bbd9cda413d836db2f5c79da05755225c41af1cffbfaf1777b64abb521ac63e09d6101fe16fa7b98a647b94eccef0601681c34d4aa0ac9ded573f14460dc5dc5337d24bdf1f69325689346795adb0f9159cb2779463dce6e084adf861b61bd76bb160132'
n = 'c6c7c7879953a678d8e6d2ab85248f19b3f7c4b1e0c4c3f5bc1b63946abcac0cc19523386a08bbb5bd09321b9023df091f162dd0e9b100da1b5d15f78523ea6d7c6d7c7cd8b5c287fcd5d91defc53a32885c0a6f16f3b13221bbd4b5254bb9dbfe79244d343841485ad38fb139abfa3c3bd50e4787b1e882d21ada914989c1497774bdaa046ad2366028bd31f9277c39f58fe6fc78c247c4159b8879eaa7e15301ce937a7491e7727e5ae6e7852df6f9fd3367e5bb178c7013805a16ee68f6cdf8f5f72b2fbc159c38244082b1c47f5814a494ac7b310c37fe68a85e4448885d0de8f93d21106121ff74c0c6452ff697b2d2660483af13ce82ebdc0293b24dad'
c = '101ef1af3fd07a28858d5102e2448f29fd995f63df13b6e6a98d077e2330722af3374cd30652943fd1de006118024a4c86a23eae960b872e8d6c5735d73a05c40d039b6779b78f0fb90daf5011de05636b35a47416cb91712df3ca62f32bd2799b24d3b267a6140f98b07dfbb9e333bc71170776ce794f34674c232544df18e719698614958bbda4e371e58e22df63c2284f0f748af6ea0465f520ed8a70ba8d12307900216645b820c29a6297c1754a703a7caa1747ecf4d4bee49163366686ff15961db87f08007c302bde64c3e4dc165604a856b036c891ef4b0dd1fd9aec79f2a7d2d017c880c1a523d1d46868a99ee2b0046cacebe65da9a3ce3b7c9683'


decin = int(str(n), 16)
decix = int(str(x), 16)
decic = int(str(c), 16)

e = 65537

phiN = decin-decix+1

def ExtendedEuclidAlgo(a, b):
	if a == 0 :
		return b, 0, 1
	gcd, x1, y1 = ExtendedEuclidAlgo(b % a, a)
	x = y1 - (b // a) * x1
	y = x1
	return gcd, x, y
	
def linearCongruence(A, B, N):
	A = A % N
	B = B % N
	u = 0
	v = 0
	d, u, v = ExtendedEuclidAlgo(A, N)
	if (B % d != 0):
		print(-1)
		return
	x0 = (u * (B // d)) % N
	if (x0 < 0):
		x0 += N
	for i in range(d):
		return ((x0 + i * (N // d)) % N)

d = linearCongruence(e, 1, phiN)
```

![2022-04-01_11-00](https://user-images.githubusercontent.com/77691959/161290106-0e4525f8-3579-481f-b583-4e0903da6d30.png)

#### Sau tôi dùng tool X-RSA để  attack rsa bằng n, c, e, phiN, d đã tìm được ở trên
![2022-04-01_11-01](https://user-images.githubusercontent.com/77691959/161290096-6f600dca-57fd-4734-b916-b1f7b2491cf8.png)

### Flag: picoCTF{92fe3557}.
