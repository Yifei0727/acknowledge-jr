# 常见缩略词

卡组织（金融机构）

### ISO

* International Organization for Standardization
* 国际标准化组织

### EMV

* `Europay`\(2002被万事达合并\) [`MasterCard`](chang-jian-suo-lve-ci.md#mastercard) [`Vis`](chang-jian-suo-lve-ci.md#visa)
* 三家卡组织的缩写,本文中特指由这三家卡组织制定的一种金融IC卡计数标准

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

### CSU

* Card Status Update
* 卡片状态更新
* EMV/M Chip 使用

### PAD

* Proprietary Authentication Data 
* 专用认证数据
* EMV/M Chip 使用

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
* TAG为 `9F10`

### AC

* Application Cryptogram
* 应用密文
* TAG为 `9F26`
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
* TAG为 `91` 的后2字节

### ARPC

* Authorisation Response Cryptogram
* 认证响应密文
* TAG为 `91` 的前8字节

### ATC

* Application Transaction Counter
* 交易计数器
* TAG为 `9F36` 

IC卡交易行为

### ICC 

* Integrated Circuit\(s\) Card
* 集成电路卡

### IAC

*  Issuer Action Codes

### AAC

* Acquirer Action Codes 见 [IAC](chang-jian-suo-lve-ci.md#iac)
* Application Authentication Cryptogram 见 [AC](chang-jian-suo-lve-ci.md#ac)/AAC

### TAC

* Terminal Action Codes

卡片相关

### PAN

* Primary Account Number
* 主账号
* 多指代完整的卡片账号
* TAG为 `5A`

### SN

* Serial Number
* 序列号/序号
* TAG为 `5F34`

### CSN

* Chip Serial Number
* 芯片序列号

### PIN

* Personal Identification Number
* 个人身份识别码, 通俗意义的客户密码

### TEE

* Trusted Execution Environment
* 可信执行环境

### SE

* Secure Element 多指安全原件, 与[TEE](chang-jian-suo-lve-ci.md#tee)搭配 
* Secure Environment 安全环境



### DEA 

* Data Encryption Algorithm
* 数据加密标准 特指 [DES](chang-jian-suo-lve-ci.md#des)
* 参见\[ ANSI XXX 标准\] TODO

### TDEA 

* The Triple Data Encryption Algorithm
* 三重数据加密 Triple DES algorithm特指 [DESede](chang-jian-suo-lve-ci.md#desede) 
* 参见\[ ANSI XXX 标准\] TODO

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
* 另见 HKDF\(Hash based KDF\)

### PRG 

* pseudo-random generator
* 伪随机数生成器

### DES 

* Data Encryption Standard
* 数据加密标准
* 一种分组/对称算法,块大小为 `64bit` 密钥为`64bit(56bit有效位+8bit校验位)`

### DESede

* 使用DES进行 Encrypt-&gt;Decrypt-&gt;Encrypt 的三重加密运算方式 所以称为 DES\_e\_d\_e
* 因为DES算法密钥过短安全性有限,而使用的一种增强性手段,块大小仍为DES块大小为 `64bit` 但密钥拓展为`128it(112bit有效位+16bit校验位)` 或者 `192bit(168bit有效位+24bit校验位)` 在对数据加密时,密钥划分为\(128bit\)两段/\(192bit\)三段,称为 `LMR` 即`左侧64bit`,`中间64bit`以及`右侧64bit` 而两段密钥由于缺少`R`,使用L取代即 `LML`

### AES 

* Advanced Encryption Standard
* 高级数据加密标准,用于替代DES
* 又名 `Rijdael`
* 一种分组/对称算法,块大小为 `128bit`, 密钥支持 `128bit` `192bit` 和`256bit`

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
* 一种签名算法, NIST 制定的[DSS](chang-jian-suo-lve-ci.md#dss)
* 参见 NIST XXX文档 TODO

### DSS

* Digital Signature Standard
* 数字签名标准

### ECC 

* Elliptic Curve Cryptography
* 椭圆曲线密码
* 一种非对称算法,相对于RSA算法具有密钥短效率高强度高的特点

### IV

* Initialization Vector
* 初始化向量
* 一般用于分组算法,大小等于分组算法块大小
* 安全敏感度上,一般认为是非敏感数据,可公开获取的
* 一般意义上 IV 应为随机产生,每次对数据进行加密不应相同
* 本文中,大部分规范一般不使用IV,或者说 IV 为全 `0x00`

加密模式

### ECB 

* Electronic Codebook
* 电子密码本模式
* 普通的块运算模式

### CBC

* Cipher Block Chaining
* 密文块链\[接\]模式
* 加密的数据块与数据块之间有相关联关系,会相互影响
* 而相对的[ECB](chang-jian-suo-lve-ci.md#ecb)的 每个数据块 之间则相互独立,互不影响

### CFB

* The Cipher Feedback
* 密文反馈
* 
### CTR

* The Counter
* 计数

### OFB

### GCM

### XTS



