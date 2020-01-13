# IC卡脚本安全相关

### 

IC卡

标准 PBOC EMV VISA MasterCard

### 卡片应用密文认证\(ARQC\)

IC卡包含的知识很多，这里只对验卡（交易）阶段做说明

1. 验证卡片（交易）ARQC
2. 卡片验证（交易）ARPC
3. 双向认证 服务器端验证ARQC，响应ARPC

   > EMV 4.1 book2 中对ATC Limit
   >
   > Issuers should be aware that few, if any, cards in normal use will approach the 65,535 transaction limit \(60 per day every day for a 3 year card\) and that cards with a high count may have been subject to attack. If a card with a shorter lifetime is desired, consideration may be given to a lower limit, or to starting the counter at an intermediate value

   ​

#### VISA Cryptogram Version Number 10

> 使用UDK\[ac\]直接计算TC/AAC/ARQC 数据填充 PaddingMethod1
>
> The remaining right-most bits in the last data block shall be zero filled for Cryptogram Version 10.
>
> [Visa Integrated Circuit Card Card Specification, Version 1.4.0](VIS1.4.0) -D.2 Generating the TC, AAC, and ARQC

输入密钥： 128Bits UDK\[ac\]

密钥算法： DESede-CBC 初始化向量 全0，分组 64Bits

填充规则：不足时填充`0x00` 即ISO-9797-1 Padding Method 1

算法描述：直接使用UDK\[ac\] 对填充数据进行X9.9 MAC计算，取MAC即为ARQC

输出： 8Byte 结果

#### VISA Cryptogram Version Number 14

> 使用UDK\[ac\]离散的会话密钥计算TC/AAC/ARQC 数据填充 PaddingMethod2
>
> For Cryptogram Version 14, a mandatory “80” byte is added at the end of the significant data. The smallest number of “00’ bytes are added to right of the “80” so the last block is 8 bytes.
>
> [Visa Integrated Circuit Card Card Specification, Version 1.4.0](VIS1.4.0) -D.2 Generating the TC, AAC, and ARQC

输入密钥：128Bits SK\[ac\]

密钥算法：DESede-CBC 初始化向量 全0，分组 64Bits

填充规则：填充强制 `0x80` 即ISO-9797-1 Padding Method 2

算法描述：使用会话密钥SK\[ac\]对填充数据进行X9.9规则的MAC计算，取MAC即为ARQC

输出： 8Byte 结果

#### MasterCard M/Chip4

输入密钥：128Bits SK\[ac\]

密钥算法：DES-CBC 初始化向量 全0，分组 64Bits

填充规则：填充强制 `0x80` 即ISO-9797-1 Padding Method 2

算法描述：使用会话密钥SK\[ac\]对填充数据进行X9.19规则的MAC计算，取MAC即为ARQC

输出： 8Byte 结果

#### UnionPay  PBOC3.0

* 国际

  _**输入密钥：**_ 128Bits SK\[ac\]

  _**密钥算法：**_ DES-CBC 初始化向量 全0，分组 64Bits

  _**填充规则：**_ 填充强制 `0x80` 即ISO-9797-1 Padding Method 2

  _**算法描述：**_ 使用会话密钥SK\[ac\]对填充数据进行X9.19规则的MAC，取MAC即为ARQC

  _**输出：**_ 8Byte 结果

* 国密

  _**输入密钥：**_ 128Bits SK\[ac\]

  _**密钥算法：**_ SM4-CBC 初始化向量 全0，分组 128Bits

  _**填充规则：**_ 填充强制 `0x80` 即ISO-9797-1 Padding Method 2

  _**算法描述：**_ 使用会话密钥SK\[ac\]对填充数据进行X9.9规则的MAC，取MAC即为ARQC

  _**输出：**_ 8Byte 结果

### 卡片应用密文响应\(ARPC\)

#### EMV 规定了两种计算方式

* EMV4.1 ARPC方式一（EMV4.1 Book2 8.2.1）

  将 计算的 ARQC【8Byte】与 ARC\|\|0【6Byte】异或，使用过程密钥SK\[ac\]加密，结果就是 ARPC

* EMV4.1 ARPC方式二 （EMV4.1 Book2 8.2.2）

  使用到额外的 CSU（卡片状态 Card Status Update）4Byte 以及专用数据（ Proprietary Authentication Data ） 0-8Byte

  将 ARQC、 CSU和PAD 拼接，按照[ISO/IEC 9797-1 Algorithm 3](https://en.wikipedia.org/wiki/ISO/IEC_9797-1#MAC_algorithm_3), and the parameter s is set to 4进行MAC

  输出 4字节结果作为ARPC，然后将ARPC\|\|CSU\|\|PAD 返回

#### UnionPay  PBOC3.0

ARPC由ARQC生成，具体实现方法如下：

* a\) 将应用密文与授权响应密文的响应代码（tag为91子域的后面部分）进行异或。应用密文包括在上传的请求报文tag为9F26的子域域中，通常是ARQC，在一些特殊情况下是AAC。授权响应密文的响应代码在执行异或前左对齐后面补6个字节0x00。

  该方式同EMV4.1 ARPC方式一

* b\) SM4算法模式：上述异或的结果是一个8字节的数据块D1，将数据块D1扩充成16字节，不足16字节的右边补0x00,对D1用过程密钥采用SM4密钥计算得到16个字节的HK。取16字节的HK的左边8字节HKL，作为8字节ARPC。

> IC卡部分数据填充多以 ISO 9797-1 Padding Method 2 为主

### IC卡脚本加密

GP规范中要求加密需支持两种模式

1. ECB 该模式下，如果不足时填充 `0x00` 即执行 Padding Method 1
2. CBC 该模式下，IV 默认为全0 `0x00` ， 强制填充 `0x80` 即执行 Padding Method 2

> 特别的，应用系统在解密/转加密时不需要去除填充，解决对块数据操作。（去）填充操作由IC卡完成

EMV 等规范都遵循GP规范

### IC卡脚本MAC

GP个人化规范中规定MAC长度： 用于数据准备时：4Byte ，用于个人化时：8Byte

EMV中MAC 长度 4-8Bytes，且数据填充方式为 强制`0x80` \(Padding Method 2\)

EMV 标准中提供两种计算方法

> 1. ISO/IEC 9797-1 Algorithm 1  等同于 X9.9（但支持多倍密钥）**（填充规则不同）**
> 2. ISO/IEC 9797-1 Algorithm 3  等同于 X9.19（双倍密钥）**（填充规则不同）**

不同规范中 用于计算MAC的key的生成方式有所不同，参见对应规范的密钥离散方式

* VISA规范中  使用 X9.19 即EMV方法2
* MasterCard  M/Chip4 使用X9.9 即EMV方法1
* 银联卡 PBOC2.0 使用X9.9 即EMV方法1   
* 银联卡 PBOC3.0 国密 使用X9.9 但算法更换为SM4

  ARQC 8字节 是由结果的 16Byte分成左右两部分并异或的结果

### IC卡PIN修改 PIN Change

