# 常见缩略词

### SMI 

* Secure Messaging Integrity 
* 直译`证明的安全消息`,即脚本MAC用途
* mac

### SMC

* Secure Messaging for Confidentiality 
* 直译`保密的安全消息`,即脚本加密用途
* enc

### DAK

### DN

* Dynamic Number 
* 数据鉴定码 动态数字
* MasterCard 万事达卡片使用

### IDN

* Issuer Dynamic Number

### MDK

* Master Divers Key
* 主离散密钥
* 同 IMK

### IMK

* Issuer Master Key
* 发卡行主密钥  

### UDK

* Union Divers Key 
* 唯一离散密钥，即卡片密钥

### KMU KMC

* DES Master Key for Personalization Session Keys
* 一般指芯片主控密钥

### UN

* Unperdictable Number
* 不可预知数
* TAG为 `9F37`

### CVR

* Card Verification Results
* 卡片验证结果
* 联网联合规范中 TAG为 `9F10`

### AC

* Application Cryptogram
* 应用密文
* 联网联合规范中 TAG为 `9F26`
* 不同场景下AC对应的称谓不同可能是下列中的一种
  * ARQC
    * Authorisation Request Cryptogram
    * 认证请求密文
  * TC
    * Transaction Certificate
    * 交易认证
  * AAC
    * Application Authentication Cryptogram
    * 应用认证密文

### ARC

* Authorisation Response Code
* 认证响应码
* 联网联合规范中 TAG为 `91` 的后2字节
* ARPC
  * Authorisation Response Cryptogram
  * 认证响应密文
  * 联网联合规范中 TAG为 `91` 的前8字节
* ATC
  * Application Transaction Counter
  * 交易计数器
  * 联网联合规范中 TAG为 `9F36` 

IC卡交易行为

### ICC 

* Integrated Circuit\(s\) Card
* 集成电路卡

### IAC

*  Issuer Action Codes

### AAC

* Acquirer Action Codes 见 IAC
* Application Authentication Cryptogram 见 AC/AAC

### TAC

* Terminal Action Codes

卡片相关

### PAN

* Primary Account Number
* 主账号
* 多指代完整的卡片账号
* 联网联合规范中 TAG为 `5A`

### SN

* Serial Number
* 序列号/序号
* `5F34`

### CSN

* Chip Serial Number
* 芯片序列号

### PIN

* Personal Identification Number
* 个人身份识别码, 通俗意义的客户密码

卡组织（金融机构）

### ISO 

* International Organization for Standardization
* 国际标准化组织

### EMV

* `Europay`\(2002被万事达合并\) `MasterCard` `Visa`

### JCB

* JAPAN CREDIT BUREAU CARD
* 日本信用卡组织

### AMEX

* American Express
* 美国运通

### PBOC

* The People's Bank Of China
* 中国人民银行

### UnionPay

* 银联\(中国\)

### MasterCard

* [万事达](https://www.mastercard.com.cn/zh-cn.html) 全球第二大信用卡组织

### VISA

* 维萨 全球第一大信用卡组织

### NIST

* National Institute of Standards and Technology
* 美国国家标准与技术研究院

### ANSI

* American National Standards Institute
* 美国国家标准协会

### DEA 

* Data Encryption Algorithm
* 数据加密标准 特指 DES \[参见 TODO\]

### TDEA 

* The Triple Data Encryption Algorithm
* 三重数据加密 特指 DESede 

发卡数据准备（DP Data Preparation）与个人化（Personalization）

### TK

* Transport Key
* 传输\(加密\)密钥,一般指 DP系统与个人化系统协商的密钥

### KEK 

* Key Exchange Key
* 交换密钥密钥, 交换密钥用途的密钥,用于加密/解密密钥

### KDF

* Key Derivation Function
* 密钥导出函数

### PRG 

* pseudo-random generator
* 伪随机数生成器

### DES 

* Data Encryption Standard
* 数据加密标准

### AES 

* Advanced Encryption Standard
* 高级数据加密标准,用于替代DES
* 又名 `Rijdael`

### RC4

* [Rivest Cipher](https://en.wikipedia.org/wiki/RC_algorithm) 4
* RC 2/3/4/5/6
* RC4 一种流加密算法,已被认为是不安全的
* 对称算法

### MD5 

* Message Digest 5
* MD 2/4/5/6
* MD2/MD4/[MD5](https://en.wikipedia.org/wiki/MD5)  已被认为是不安全的
* 消息摘要算法

### SHA

* Secure Hash Algorithm
* SHA-1 已被认为是不安全的
* SHA-2 是目前使用的
* 安全的哈希算法,目前最新为 SHA-3

### RSA 

* Asymmetric cryptographic algorithm invented by Rivest, Shamir, andAdleman 
* 一种非对称算法, 是以三位发明者名字的首字母作为名称

### DH 

* Diffie-Hellman
* 一种非对称算法,以发明者名字命名, 通常主要用于密钥交换,一般会和其他算法融合
* 例如ECDH

### DSA

* Digital Signature Algorithm
* 一种签名算法, NIST 制定的DSS

### DSS

* Digital Signature Standard
* 数字签名标准\(NIST\)

### ECC 

* Elliptic Curve Cryptography
* 椭圆曲线密码
* 一种非对称算法,相对于RSA算法具有密钥短效率高强度高的特点

加密模式

### ECB 

* Electronic Codebook

### CBC

*  Cipher Block Chaining

### CFB

### CTR

### OFB

### GCM

### XTS



