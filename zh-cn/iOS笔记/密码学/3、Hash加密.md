## 1、Hash概述
&emsp;&emsp;`Hash`，一般翻译做散列、杂凑，或音译为哈希，是把任意长度的输入（又叫做预映射pre-image）通过散列算法变换成固定长度的输出，该输出就是散列值。这种转换是一种压缩映射，也就是，散列值的空间通常远小于输入的空间，不同的输入可能会散列成相同的输出，所以不可能从散列值来确定唯一的输入值。简单的说就是一种将任意长度的消息压缩到某一固定长度的消息摘要的函数。 

&emsp;&emsp;常见的`MD5`加密，`SHA1`加密，`SHA256`加密等都是`Hash`加密的一种。以下是MD5加密的终端命令行。
```bash
# 对字符串加密  md5 -s "文本"
~ % md5 -s "123"
MD5 ("123") = 202cb962ac59075b964b07152d234b70

# 对文件加密  md5 文件名.后缀
~ % md5 DSC_5572.JPG 
MD5 (DSC_5572.JPG) = 83f54f3744fb47b71b6bde03ce2028bf
```

## 2、Hash加密的特点

- 算法是公开的（怎么算的不重要，不在我们讨论范围内）；

- 单向不可逆：从hash值不可以反向推导出原始的数据，即**只有加密过程，没有解密过程**；

- 抗冲突性：**对相同数据运算，得到的结果是一样的**，不同的数据加密得到的结果是不一样的。

- 易压缩：对不同数据运算得到的**密文长度是固定的且很短**，如MD5得到的结果默认是128位，32个字符（16进制标识）；

- 信息摘要/指纹：基于Hash值的不可逆性和抗冲突性，Hash加密一般用于做信息摘要或者信息“指纹”，是用来做`数据识别`的。

## 3、Hash加密的作用
### 3.1 用户密码加密
&emsp;&emsp;为了保护用户密码的安全性，**服务端一般不会直接存储用户输入的明文密码**（明文存储情况下如果发生密码泄漏，公司会有连带责任），而是由客户端进行适当的加密之后传给服务器存储（一般使用MD5进行加密）。这样可以保证用户真实的密码永远不会被存储下来，即使服务端数据泄漏，也无法通过逆向算法推出用户的真实密码。
![image](https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/hash_1.jpg) 
&emsp;&emsp;但是，如果仅仅使用Hash算法对用户密码进行加密，依然无法无法保证用户密码的安全，详见[用户密码安全防护](https://)

### 搜索引擎

&emsp;&emsp;简单说一下搜索引擎与Hash的关系.  
&emsp;&emsp;比如百度搜`大爷 吃饭 妹子` 和`吃饭 妹子 大爷` 这三个关键词。出现的结果如果没有`大爷 吃饭 妹子`直接关联的内容，那么搜索结果是一样的。  
&emsp;&emsp;因为搜索时引擎会拆词，关键字会被拆成`大爷`、`吃饭`和`妹子`这三个词，然后得到三个词的Hash值，然后在对位相加。无论顺序是怎么样的，对位相加后的结果都一样！所以不论顺序怎么边，搜索结果都是一样的。

### 3.2 数据校验
&emsp;&emsp;在讲Hash加密的特点的部分提到，Hash加密可以用来做`数据识别`。 
#### 3.2.1 版权校验 
&emsp;&emsp;当你用拍了一段视频，并成功上传到某视频网站，此时**视频网站就会记录下这段视频的Hash值，并认定这段视频的版权归属于你**。别人如果上传了相同的视频，则会提示该视频已存在等类似的提示。
#### 3.2.2 大文件重复校验
&emsp;&emsp;同样的，在上传文件到云盘时，服务器会记录下这个文件的Hash值，如果别人上传相同的文件，**服务器会先取这份文件的Hash值与现存文件的Hash进行比对**，如果匹配到，说明服务器上有相同的数据，服务器只要在你的账号里加了一条数据，而无需重新上传这份文件。所以会感觉`秒传`。
下面我们来看看几种文件操作是否会改变Hash值。

- 修改文件名，修改前的Hash值与修改后的Hash值是否一致 -- Hash值一致
<img src="https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/hash_3_1.jpg" width="49%">
<img src="https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/hash_3_2.png" width="49%">
<img src="https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/hash_3_3.jpg" width="49%">
<img src="https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/hash_3_4.png" width="49%">

- 复制文件，原文件的Hash值与副本的Hash值是否一致 -- Hash值一致
<img src="https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/hash_3_5.jpg" width="49%">
<img src="https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/hash_3_6.png" width="49%">

- 压缩文件，原文件的Hash值与压缩文件的Hash值是否一致 -- Hash值变了
<img src="https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/hash_3_7.jpg" width="49%">
<img src="https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/hash_3_8.png" width="49%">

### 3.3 数字签名
&emsp;&emsp;客户端在向想服务器发起请求的过程中，黑客完全有机会**拦截请求并篡改数据**之后重新发起请求，造成数据安全隐藏。这就需要对网络请求的进行数据签名。
1、对数据做Hash加密；  
2、对Hash值进行RSA加密；  
3、服务端对RSA密文进行解密获取客户端的Hash值；  
4、服务端对数据进行Hash加密；  
5、对两个Hash值进行比对，如果一致说明数据没有被篡改，如果不一致，说明数据被篡改。   
我们把用RSA包装Hash值的数据称为`数字签名`。
![image](https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/hash_3_9.jpg) 

### 3.4 其他
- 负载均衡
- 服务器缩容
- 服务器扩容
- 虚拟节点
来源：https://www.zhihu.com/question/26762707


## 4、Hash加密的安全与防护
从理论上说，Hash只有加密过程，没有解密过程。那么是不是使用了Hash加密后的字符串来存密码就万无一失了呢，理想总是很丰满，而现实总是很骨感的。
一起来看看这个密文反向查询网站：[cmd5.com](https://www.cmd5.com/)
以下是网站的介绍
```
本站针对md5、sha1等全球通用公开的加密算法进行反向查询，通过穷举字符组合的方式，
创建了明文密文对应查询数据库，创建的记录约90万亿条，占用硬盘超过500TB，查询成功率95%
以上，很多复杂密文只有本站才可查询。自2006年已稳定运行十余年，国内外享有盛誉。 
```
![image](https://xudusheng.github.io/document/zh-cn/iOS笔记/密码学/images/hash_4_1.jpg) 

## 5、Hmac加密方案



