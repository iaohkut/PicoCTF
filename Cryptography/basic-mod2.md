# Basic-mod2
> Description: A new modular challenge! \
The message here: 104 290 356 313 262 337 354 229 146 297 118 373 221 359 338 321 288 79 214 277 131 190 377.\
Take each number mod 41 and find the modular inverse for the result. Then map to the following character set: 1-26 are the alphabet, 27-36 are the decimal digits, and 37 is an underscore. \
Wrap your decrypted message in the picoCTF flag format (i.e. picoCTF{decrypted_message})


#### Đầu tiên chúng ta có thể thấy đây là một chuỗi decimal cần phải mod để lấy được giá trị cần thiết sau đó chúng ta sẽ số sánh chuỗi vừa được chuyển với định dạng đã được cho ở đầu bài.
* 1-26 is the alphabet (uppercase)
* 27-36 are the decimal digits
* 37 is an underscore

#### Bởi vì chuỗi được đánh số từ 0 nên sau khi tìm được "modular inverse" thì chúng ta lần lượt lấy kết quả trừ 1:
```python
flag+=valid[list2[i] - 1]
```

#### Và đây là source code: 
``` python
import  string

valid = string.ascii_lowercase + string.digits + "_"

def modInverse(a, m):
     
    for x in range(1, m):
        if (((a%m) * (x%m)) % m == 1):
            return x
    return -1

tmp="104 290 356 313 262 337 354 229 146 297 118 373 221 359 338 321 288 79 214 277 131 190 377"
list1 = list(map(int, tmp.split()))

list2 = []
for i in range(len(list1)):
    module = list1[i] % 41
    list2.append(modInverse(module, 41))

print(list2)
flag = ""
for i in range(len(list2)):
    flag+=valid[list2[i] - 1]
    
print("picoCTF{" + flag + "}")
```

### Flag: picoCTF{1nv3r53ly_h4rd_8a05d939}.
