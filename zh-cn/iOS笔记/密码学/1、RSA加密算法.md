&emsp;&emsp;`密码学`是指研究信息加密，破解密码的技术科学。密码学的起源可追溯到2000年前。而当今的密码学是以数学为基础的。   
&emsp;&emsp;`密码学`的历史大致可以追溯到两千年前，相传古罗马名将凯撒大帝为了防止敌方截获情报，用密码传送情报。凯撒的做法很简单，就是对二十几个罗马字母建立一张对应表。这样，如果不知道密码本，即使截获一段信息也看不懂。从凯撒大帝时代到上世纪70年代这段很长的时间里，密码学的发展非常的缓慢，因为设计者基本上靠经验，没有运用数学原理。   
- **在1976年以前**，所有的加密方法都是同一种模式：**加密、解密使用同一种算法**。在交互数据的时候，彼此通信的双方就必须将规则告诉对方，否则没法解密。那么加密和解密的规则（简称密钥），它保护就显得尤其重要。传递密钥就成为了最大的隐患。这种加密方式被成为`对称加密算法`（symmetric encryption algorithm）  
- **1976**年，两位美国计算机学家 **迪菲（W.Diffie）、赫尔曼（ M.Hellman ）** 提出了一种崭新构思，可以在不直接传递密钥的情况下，完成密钥交换。这被称为`“迪菲赫尔曼密钥交换”`算法。开创了密码学研究的新方向   
- **1977年**，三位麻省理工学院的数学家 **罗纳德·李维斯特（Ron Rivest）、阿迪·萨莫尔（Adi Shamir）和伦纳德·阿德曼（Leonard Adleman）**一起设计了一种算法，可以实现非对称加密。这个算法用他们三个人的名字命名，叫做RSA算法。 

## 2、RSA数学原理
&emsp;&emsp;上世纪70年代产生的一种加密算法。其加密方式比较特殊，需要两个密钥：公开密钥简称公钥(publickey)和私有密钥简称私钥(privatekey)。**公钥加密，私钥解密；私钥加密，公钥解密**。这个加密算法就是伟大的`RSA`。

### 2.1 离散对数问题
思考一：有没有加密容易，破解很难的的数学运算？  
方案：-->离散对数
> 3为17的原根  
> $3^{1} \% 17 = 3$;  $3^{2} \% 17 = 9$;  $3^{3} \% 17 = 10$;  
> $3^{4} \% 17 = 13$;  $3^{5} \% 17 = 5$;  $3^{6} \% 17 = 15$;  
> $3^{7} \% 17 = 11$;  $3^{8} \% 17 = 11$; $3^{9} \% 17 = 14$;  
> $3^{10} \% 17 = 8$;  $3^{11} \% 17 = 7$; $3^{12} \% 17 = 4$;  
> $3^{13} \% 17 = 12$; $3^{14} \% 17 = 2$; $3^{15} \% 17 = 6$;  
> $3^{16} \% 17 = 1$;  
> 结论：正推很简单，反推很难

**模的基本运算** -- 预备知识  
> 加法：$(a+b)\ \% \ m = (a \ \% \ m) + (b \ \% \ m) $
> 减法：$(a-b)\ \% \ m = (a \ \% \ m) - (b \ \% \ m) $
> 乘法：$(a*b)\ \% \ m = (a \ \% \ m) * (b \ \% \ m) $
> 除法：一般不做除法运算，因为可能无法取整
> 幂运算：${(a \ \% \ m)}^b = a^b \ \% \ m $ --->根据乘法公式计算得来

### 2.2 欧拉函数φ
&emsp;&emsp;任意给定正整数n，请问在小于等于n的正整数之中，有多少个与n构成互质关系？ 计算这个值的方式叫做欧拉函数，使用：Φ(n)表示  
- 如:  计算8的欧拉函数，和8互质的 `1`、2、`3`、4、`5`、6、`7`、8    
    - φ(8) = 4  
- 计算7的欧拉函数，和7互质的 `1`、`2`、`3`、`4`、`5`、`6`、7    
    - φ(7) = 6  
- 计算56的欧拉函数    
    - φ(56) =  φ(8) *  φ(7) = 4 * 6 = 24 

**欧拉函数特点**
- 1、当n是`质数`的时候，φ(n)=n-1。
- 2、如果n可以分解成`两个互质`的整数之积
    - 如n=A*B则：  
        - φ(A*B)=φ(A)* φ(B)
        - φ(56) =  φ(8) *  φ(7) = 4 * 6 = 24   
- 3、根据以上两点得到：如果N是两个质数P1 和 P2的乘积则 `（两个质数必须互质）` 
    - φ(N)=φ(P1)* φ(P2)=(P1-1)*(P2-1) 
    - φ(21) = φ(3) * φ(7) = 2*6 = 12
    - `1` `2` 3 `4` `5` 6 7 `8` 9 `10` `11` 12 `13` 14 15 `16` `17` 18 `19` `20` 共12个

### 2.3 欧拉定理
&emsp;&emsp;如果两个正整数m和n互质，那么m的φ(n)次方减去1，可以被n整除。   
- **公式:** $m^{φ(n)} \%n = 1$
验证：m=3，n=4，则：φ(n) = φ(4) = 2  
结果：$3^2 \ \% \ 4 = 1 $   

### 2.4 费马小定理
&emsp;&emsp;欧拉定理的特殊情况：如果两个正整数m和n互质，而且n为质数！那么φ(n)结果就是n-1。  

- **公式：**$m^{φ(n)} \ \%\ n = m^{(n-1)} \ \%\ n = 1$
验证：m=3，n=5，则：φ(n) = φ(5) = 5-1 = 4  
结果：$3^4 \  \% \ 5 = 1$   

### 2.5 公式换算  

- **公式：**$m^{φ(n)} \%\ n = 1$，正整数m和n互质  
验证：m=3，n=5，则：φ(n) = φ(5) = 4  
结果：$3^4 \ \% \ 4 = 1$

- **公式2：**$m^{k*φ(n)} \%\ n = 1$，正整数m和n互质  
验证：m=3，n=5，则：φ(n) = φ(5) = 4，假设k=2  
结果：`$3^8 \ \% \ 4 = 1$`

- **公式3：**$m^{k*φ(n)+1} \%\ n = m$，正整数m和n互质,且m<n  
验证：m=3，n=5，则：φ(n) = φ(5) = 4，假设k=1  
结果：$3^5 \ \% \ 4 = 3$

### 2.6 模反元素 
&emsp;&emsp;如果两个正整数e和x互质，那么一定可以找到整数d，使得 e\*d-1 被x整除。那么d就是e对于x的“模反元素”  
 
- 1、$e*d \ \% \ x = 1$;  
验证：e=3，x=5   
找到结果：d=2或d=7或 ……，3*2 % 5 = 1

- 2、e\*d = k\*x + 1  
当x=φ(n)时，$m^{k*φ(n)+1}\ \%\ n = m$ --> $m^{e*d}\ \%\ n = m$  
验证：m=3，n=15, x=φ(n)=φ(15)=φ(3)*φ(5) = 2*4 = 8   
接着：取e=3与x=8互质，找到模反元素d=3或11或***  
结果：$m^{e*d}\ \%\ n = 3^{3*3}\ \%\ 15 = 3$

 
- 3、以上结果做一点延伸：  
> $m^{e*d}\ \%\ n = 3^{3*3}\ \%\ 15 = 3$  
> $m^{e*d}\ \%\ n = 4^{3*3}\ \%\ 15 = 4$  
> $m^{e*d}\ \%\ n = 5^{3*3}\ \%\ 15 = 5$  
> $m^{e*d}\ \%\ n = 6^{3*3}\ \%\ 15 = 6$  
> $m^{e*d}\ \%\ n = 7^{3*3}\ \%\ 15 = 7$  
> $m^{e*d}\ \%\ n = 8^{3*3}\ \%\ 15 = 8$  
> $m^{e*d}\ \%\ n = 9^{3*3}\ \%\ 15 = 9$  
> $m^{e*d}\ \%\ n = 10^{3*3}\ \%\ 15 = 10$  
> $m^{e*d}\ \%\ n = 11^{3*3}\ \%\ 15 = 11$  
> $m^{e*d}\ \%\ n = 12^{3*3}\ \%\ 15 = 12$  
> $m^{e*d}\ \%\ n = 13^{3*3}\ \%\ 15 = 13$  
> $m^{e*d}\ \%\ n = 14^{3*3}\ \%\ 15 = 14$   
> 
> **结论：**m<n时，$m^{e*d}\ \%\ n = m$


## 3、 RSA加密原理：
### 3.1 迪菲赫尔曼密钥交换
&emsp;&emsp;原始需求：不直接用密钥进行传输，保证数据传输更加安全，交换的目的是为了获取到10这个值。  
&emsp;&emsp;实际应用可能是为了获得对称加密的秘钥![image](https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/rac_1.png) 

### 3.2 RSA加密原理：

1、$m^{e}\%\ n = c$  -->加密
2、$c^d\% n = (m^e\%n)^d \%\ n =m^{e*d} \%\ n =m$  -->解密
> - 加密公式：$m^{e} \%\ n = c$
> - 解密公式：$c^{d} \%\ n = m$
>   - 公钥：n和e  
>   - 私钥：n和d 
>   - 明文:  m 
>   - 密文:  c 
** 特别说明 **
>  - 1、n会非常大，长度一般为1024个二进制位。（目前人类已经分解的最大整数，232个十进制位，768个二进制位） 
> - 2、由于需要求出φ(n)，所以根据欧拉函数是特点，最简单的方式n由两个质数相乘得到: 质数：p1、p2 
> Φ(n) = (p1 -1) * (p2 - 1) 
> - 3、最终由φ(n)得到e 和 d 。 总共生成6个数字：p1、p2、n、φ(n)、e、d

### 3.3 关于RSA的安全

> 除了公钥用到了n和e其余的4个数字是不公开的。 
> 目前破解RSA得到d的方式如下：  
> 1、要想求出私钥d。由于e*d = φ(n)*k + 1。要知道e和φ(n); 
> 2、e是知道的，但是要得到 φ(n)，必须知道p1 和 p2。 
> 3、由于 n=p1*p2。只有将n因数分解才能算出。--->穷尽法

### 3.4 RSA加密的特点：
> 1、只能加密小数据，加密一些核心的小数据
> 2、加密效率不高
> 3、主要用于核心数据加密，如对称加密的key，签名等

## 4、RSA终端命令 
Mac的终端可以直接使用OpenSSL进行RSA的命令运行。 
由于Mac系统内置OpenSSL(开源加密库),所以我们可以直接在终端上使用命令来玩RSA. OpenSSL中RSA算法常用指令主要有三个： 

![image](https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/rac_2.png) 
<!-- <img src="https://xudusheng.github.io/document/zh-cn/Flutter笔记/2个小时快速入门/images/4.1.png" width="49%"> -->
<!-- <img src="https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/rac_2.png" width="49%"> -->

### 4.1 生成RSA私钥(密钥长度为1024bit)
```Shell
//生成RSA私钥(密钥长度为1024bit)
RSA hmily$ openssl genrsa -out private.pem 1024
Generating RSA private key, 1024 bit long modulus
....................++++++
.++++++
e is 65537 (0x10001)

//输出私钥内容
RSA hmily$ cat private.pem
-----BEGIN RSA PRIVATE KEY-----
MIICXAIBAAKBgQDoGwSluU1rcrCuPjLuXhrslgLIwXl+vOnEHhCOlq9/BrM4yrdc
WyiEe59sjFP0ucKd55KjVxr0yMVBuY/4sYORB0DUjjm7+ndVSiGUgbk7E0gUkAi4
7mcSfMWFtnpIBKCD4lMgqllZXITusfmADTRTjou9fUudR4UuV5CA/ikN5wIDAQAB
AoGAPskQOMQnbSlZIckxfcl2/wiVODkd5Gq10ZdQY0HftzzYvkQX1aPTEgNe3L4Y
99pICu7Ze9XUNOMaeOz5RQy/ybd9dUtqSwSHWl8CKbGECMzBdQ+qyGNBtnWzen01
mj3pZ4+D3NZ6l+ztOFllLJDm7hKnLPtdSsdAPYzvGWCfWgECQQD692fJpAzS8Hd7
i/R/PvWjHTHmlGn3HQ0L7aYbZi7ojJtmZn9FNAQI6q51ttvBCXbfxctX/hEI3ULj
VkywkviLAkEA7MLFXJ6aJWnFOWDSF/9vtVVfLhPYd5r6eXrCQLziw02XQf9l3eBF
+Aide5pagX0hKk67Hq9sS295lrOk07LPlQJAGPLNU4NGbxXOmu6P0LJ+kseNNWHd
ot41dNEcKS8gTKflrulTj5qbKBPEYhlagTcipR4xl76/DMWKJ7VljEwf/wJBAKse
jtTFUPXvf3tcDh0IIq31+SftcgvoOFZqslFl86NixgsOU4rMmOWPHHuEcRub28ef
RcEE2wmelUulpWDYoQ0CQHwossxox5obfnc+tKQThmbandaylqwQNDSIxyqPy8Gz
+x68mC5X6m3JnkU+QaMPEiW5JIhw/hKeWPNsp5gaPoA=
-----END RSA PRIVATE KEY-----
```

### 4.2 从私钥中提取公钥
```Shell
//从私钥中提取公钥
RSA hmily$ openssl rsa -in private.pem -pubout -out public.pem
writing RSA key
//输出公钥内容
RSA hmily$ cat public.pem
-----BEGIN PUBLIC KEY-----
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDoGwSluU1rcrCuPjLuXhrslgLI
wXl+vOnEHhCOlq9/BrM4yrdcWyiEe59sjFP0ucKd55KjVxr0yMVBuY/4sYORB0DU
jjm7+ndVSiGUgbk7E0gUkAi47mcSfMWFtnpIBKCD4lMgqllZXITusfmADTRTjou9
fUudR4UuV5CA/ikN5wIDAQAB
-----END PUBLIC KEY-----
```

**4.1、4.2生成以下两个文件**
![image](https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/rac_2.png) 

### 4.3 将私钥转换成为明文
```Shell
//将私钥转换成为明文
RSA hmily$ openssl rsa -in private.pem -text -out private.txt
writing RSA key
```

### 4.4 通过公钥加密数据，私钥解密数据 
```Shell
//生成明文文件
RSA hmily$ vi message.txt
//查看文件内容
RSA hmily$ cat message.txt
密码:123456
//通过公钥进行加密
RSA hmily$ openssl rsautl -encrypt -in message.txt -inkey public.pem -pubin -out enc.txt
//通过私钥进行解密
RSA hmily$ openssl rsautl -decrypt -in enc.txt -inkey private.pem -out dec.txt
```

### 4.5 通过私钥加密数据，公钥解密数据 
```Shell
//通过私钥加密数据
RSA hmily$ openssl rsautl -sign -in message.txt -inkey private.pem  -out enc_p.txt
//公钥解密数据
RSA hmily$ openssl rsautl -verify -in enc_p.txt -inkey public.pem -pubin -out dec_p.txt
```
### 4.6 颁发证书请求证书（生成csr文件）
&emsp;&emsp;   csr，证书签名请求(certificate signing request，也称为CSR或certificate request)是申请人向证书颁发机构发送的一条消息，用于申请数字身份证书。它通常包含需要被颁发证书的公钥、标识信息(例如域名)和完整性保护(例如数字签名)。
```Shell
//通过私钥生成csr文件，用于请求证书
//同“钥匙串->证书助理->从机构颁发证书请求证书”的操作类似
RSA hmily$ openssl req -new -key private.pem -out rsacert.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:CN
State or Province Name (full name) []:FUJIAN
Locality Name (eg, city) []:XIAMEN
Organization Name (eg, company) []:BISINUOLAN
Organizational Unit Name (eg, section) []:BISINUOLAN
Common Name (eg, fully qualified host name) []:bisinuolan.com
Email Address []:xudusheng@bisinuolan.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
```
### 4.7 颁发签名证书（生成crt文件）
&emsp;&emsp;有CSR必定有KEY，是成对的，CSR最终变成为证书crt，和私钥key配对使用。
&emsp;&emsp;证书下发后，CSR就没有用了，只是在提交的时候需要。 机构签名的证书收费，也可自签名。

```Shell
//证书请求证书和私钥生成私有签名证书
RSA hmily$ openssl x509 -req -days 3650 -in rsacert.csr -signkey private.pem -out rsacer.crt
Signature ok
subject=/C=CN/ST=FUJIAN/L=XIAMEN/O=BISINUOLAN/OU
=BISINUOLAN/CN=bisinuolan.com/emailAddress=xudusheng@bisinuolan.com
Getting Private key
```
&emsp;&emsp;在密码学中，X.509是定义公钥证书格式的标准。X.509证书用于许多Internet协议，包括TLS/SSL，它是HTTPS(用于浏览web的安全协议)的基础。它们也用于离线应用程序，比如电子签名。一个X.509证书包含一个公钥和一个标识(主机名、组织或个人)，由证书颁发机构签名或自签名。当证书由受信任的证书颁发机构签名时，或者通过其他方法进行验证时，持有该证书的人可以依赖于它包含的公钥来与另一方建立安全通信，或者验证由相应私钥数字签名的文档。


### 4.8 导出p12文件
```Shell
//利用私钥和签名证书，导出p12文件
RSA hmily$ openssl pkcs12 -export -out p.p12 -inkey private.pem -in rsacer.crt
Enter Export Password:
Verifying - Enter Export Password:
```

## 5、RSA代码演示 

```shell
 # 生成证书
 $ openssl genrsa -out ca.key 1024
 # 创建证书请求
 $ openssl req -new -key ca.key -out rsacert.csr
 # 生成证书并签名
 $ openssl x509 -req -days 3650 -in rsacert.csr -signkey ca.key -out rsacert.crt
 # 转换格式
 $ openssl x509 -outform der -in rsacert.crt -out rsacert.der
 @endcode
```

### 5.1 生成公钥
```Shell
 # 生成证书
 $ openssl genrsa -out ca.key 1024
 # 创建证书请求
 $ openssl req -new -key ca.key -out rsacert.csr
 # 生成证书并签名
 $ openssl x509 -req -days 3650 -in rsacert.csr -signkey ca.key -out rsacert.crt
 # 转换格式
 $ openssl x509 -outform der -in rsacert.crt -out rsacert.der
 ```
 
 ### 5.2 生成私钥
 ```Shell
 # crt文件转成p12文件
  openssl pkcs12 -export -out p.p12 -inkey ca.key -in rsacert.crt
 ```
### 5.3 RSA加密代码
```ObjC
- (void)viewDidLoad {
    [super viewDidLoad];
    
    //1.加载公钥
    [[RSACryptor sharedRSACryptor] loadPublicKey: [[NSBundle mainBundle] pathForResource:@"rsacert.der" ofType:nil]];
    //2.加载私钥
    [[RSACryptor sharedRSACryptor] loadPrivateKey:[[NSBundle mainBundle] pathForResource:@"p.p12" ofType:nil] password:@"123456"];
}

- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    //1.加密
    NSData * result = [[RSACryptor sharedRSACryptor] encryptData:[@"hello" dataUsingEncoding:NSUTF8StringEncoding]];
    NSLog(@"加密的结果是:%@",[result base64EncodedStringWithOptions:0]);
    
    //2.解密
    NSData * jiemi = [[RSACryptor sharedRSACryptor] decryptData:result];
    NSLog(@"解密的结果：%@",[[NSString alloc] initWithData:jiemi encoding:NSUTF8StringEncoding]);
}
```