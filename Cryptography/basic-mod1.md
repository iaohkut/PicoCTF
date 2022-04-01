# Basic-mod1
> Description: We found this weird message being passed around on the servers, we think we have a working decrpytion scheme.
The message here: 91 322 57 124 40 406 272 147 239 285 353 272 77 110 296 262 299 323 255 337 150 102.
Take each number mod 37 and map it to the following character set: 0-25 is the alphabet (uppercase), 26-35 are the decimal digits, and 36 is an underscore.
> Wrap your decrypted message in the picoCTF flag format (i.e. picoCTF{decrypted_message})

#### Đầu tiên chúng ta có thể thấy đây là một chuỗi decimal cần phải mod để lấy được giá trị cần thiết sau đó chúng ta sẽ số sánh chuỗi vừa được chuyển với định dạng đã được cho ở đầu bài.
* 0-25 is the alphabet (uppercase)
* 26-35 are the decimal digits
* 36 is an underscore

#### Và đây là source code: 
``` python
import  string
tmp="91 322 57 124 40 406 272 147 239 285 353 272 77 110 296 262 299 323 255 337 150 102"
list1 = list(map(int, tmp.split()))

list2 = []
for i in range(len(list1)):
    list2.append(list1[i] % 37)

flag = ""
for i in range(len(list2)):
    if 26 <= list2[i] <= 35: 
        number=str(list2[i]-26)
        flag+=number
    if list2[i] == 36:
        flag+='_'
    if 0 <= list2[i] <= 25:
        flag+=string.ascii_lowercase[list2[i]]

print("picoCTF{" + flag + "}")
```

### Flag: picoCTF{r0und_n_r0und_add17ec2}.
