# REVERSE
## 1. Hide and Seek  

- Mình dủng lệnh strings file "dropFile.exe"  
![Screenshot 2023-12-17 094124](https://github.com/AnVinh07/WriteUp_CTF_TTV_KCSC/assets/131764804/c12a81ed-6dbf-4d97-94bf-116093cb89b0)  

- Mình bắt đầu soi từng ngốc ngếch, sau cùng thì mình tìm được đoạn này:  
![Screenshot 2023-12-16 155520](https://github.com/AnVinh07/WriteUp_CTF_TTV_KCSC/assets/131764804/6bb05c92-9128-4c11-b92f-d8b806a8de52)

- Thấy có hàm showFlag(), nên mình đã copy nguyên đoạn đó ra và sửa chữa 1 xíu rồi chạy thử thì kết quả ra flag=))  

### FLAG: KCSC{~(^._.)=^._.^=(._.^)~}  

## 2. Awg Mah Back  

- Đề bài cho ta 1 đoạn mã python và 1 file "output.txt"
![image](https://github.com/AnVinh07/WriteUp_CTF_TTV_KCSC/assets/131764804/2cde1a57-2e0c-4138-8762-9a26d53f17a8)
- Sau khi đọc qua, thì mình hiểu là file chứa flag có tên là "flag.txt" và sau một loại các thao tác XOR thì kết quả được cho vào file "output.txt"
- Nên mình đã đảo ngược code lại.
```
from pwn import *

with open('output.txt', 'rb') as f:
    enc = f.read()

# Split the encrypted content into three parts
a = enc[0:len(enc) // 3]
b = enc[len(enc) // 3:2 * len(enc) // 3]
c = enc[2 * len(enc) // 3:]

# Reverse the XOR operations to decrypt
c = xor(c, int(str(len(enc))[0]) * int(str(len(enc))[1]))
b = xor(a, b)
a = xor(c, a)
c = xor(b, c)
b = xor(a, b)
a = xor(a, int(str(len(enc))[0]) + int(str(len(enc))[1]))

# Combine the decrypted parts to get the original flag
decrypted_flag = a + b + c

# Save the decrypted flag to a file
with open('decrypted_flag.txt', 'wb') as f:
    f.write(decrypted_flag)

print("Decrypted flag:", decrypted_flag)

```
- Nhưng nó ra 1 chuỗi gì, mình cũng không biết, nên mình hỏi chatGPT và nó đã cho mình một đoạn code để giải mã tiếp.
```
import base64

encoded_string = "VXpCT1ZGRXpjelJPUjA1TVdETlJkMWd3U21oUk1IUm1Wa2M1WmxGcVVtcGhNVGxaVFVoS1oxaDZVblZTUmpnMFRtcFNabUl3TUhwYWVsSk5aRlY0T1E9PQ=="

# Decode base64 string to bytes
decoded_bytes = base64.b64decode(encoded_string)

# Decode bytes to ASCII
decoded_string = decoded_bytes.decode('ascii')

print("Decoded string:", decoded_string)
```
- Sau khi giải mã ra một đoạn chuỗi, thì mình vẫn chưa nhận ra nó là mã gì nên mình đã đưa nó vào CyberCher để mò thử, thì thật may mắn nó đã ra flag=))
![Screenshot 2023-12-16 150839](https://github.com/AnVinh07/WriteUp_CTF_TTV_KCSC/assets/131764804/db90d13b-9c10-4d09-85f6-c9710b9fbc17)

### FLAG: KCSC{84cK_t0_BaCK_To_B4ck_X0r`_4nD_864_oM3g4LuL}  

# PWN  
## A Gift For Pwners  
- Khi nhận được đề bài, mình liên đưa vào IDA để phân tích và vào view chọn strings để tìm  
![Screenshot 2023-12-17 093149](https://github.com/AnVinh07/WriteUp_CTF_TTV_KCSC/assets/131764804/7c2a492c-f293-4db6-85f6-3058b1b000d0)
- Vừa vào thì thấy flag chia làm 3 phần và nằm cả ở đây=))
![image](https://github.com/AnVinh07/WriteUp_CTF_TTV_KCSC/assets/131764804/b9c62616-8c22-41c9-8002-26cf8499bd54)

### FLAG: KCSC{A_gift_for_the_pwners_0xdeadbeef}



