# 个人识别码\(PIN\)校验

### 

#### PVV \(PIN Verification Value\)【VISA】

> 参见 VISA【Payment Technology Standards Manual 】的第6章节

实际上与银联PVN算法一致

#### PVN \(PIN Verification Number\)【银联】

> 用于银联代授权业务，此时银联将代替银行执行验证PIN的行为

* 国际

  > 【中国银联PIN校验码算法标准 Q/CUP 032-2008】

  密钥：128Bits

  算法：DESede

  输入：

  * PAN，右对齐去除最后一位校验位的 11 位数字，不足时左侧补 0
  * PVK 索引号 取值 1-F （0保留）
  * 客户PIN的最左 4位数字密码

  输出：4位数字 PVN

* 国密

  > 【中国银联银行卡联网联合技术规范V2.1（SM4算法试点技术指引）】附录A.1

  密钥： 128Bits

  算法：SM4 块大小 128Bits

  输入：

  * PAN，右对齐去除最后一位校验位的 11 位数字，不足时左侧补 0
  * PVK 索引号 取值 1-F （0保留）
  * 客户PIN的最左 4位数字密码

  描述：

  1. 将 11位的账号、 1位的PVK索引号、 4位的客户PIN依次拼接，构成一个32个（HEX）位的块，不足时，填充 0， 该块大小实际为一个 16Byte的明文块
  2. 使用SM4算法，对明文块加密  SM4\(PVK, Block\)，得密文块，展开为32个（HEX）位的块 
  3. 对2的结果块从左到右提取数字，依次写入到缓存 
  4. 对2的结果块从左到右提取HEX，并将HEX -10 即 A -&gt; 0, B -&gt; 1 .... F -&gt; 5，并一次写入到3的缓存
  5. 产生了一个 32 位纯数字的缓存块
  6. 提取前 4 位数字即为PVN

  输出： 4位的数字即 PVN

#### 3624 PIN Verification Algorithm 【[IBM](https://www.ibm.com/support/knowledgecenter/en/SSLTBW_2.2.0/com.ibm.zos.v2r2.csfb400/csfb4933.htm) 】

> IBM官网有描述，也可参见 VISA的【Payment Technology Standards Manual 】的6.7章节 IBM 3624 PIN Offset Processing。章节中描述为PIN支持 1-16位。但下列中依旧按照ISO标准 要求PIN为 4-12位

**IBM主机系统使用的PIN产生算法**

密钥： DESede 128Bits

输入：

* 账号PAN（12-16） 转换为 64Bits数据 一般的是对PAN去除校验位，右对齐然后左补零
* PIN长度 4-12 以Hex半字节表示 `0x4`-`0xC`，
* 16进制转换表即如何将

`0123456789ABCDEF` 对应成相同字节的 诸如`0123456789012345`这样的对应关系

规则表述：

1. 使用 密钥对 PAN块 加密DESede后，产生对应的结果，
2. 使用16进制转换表将结果中的数据进行替换，生成纯数字的表
3. 左对齐，从左开始截取 PIN长度个数字即为 PIN

> 这个算法计算出来的这个PIN 可以说是原始PIN，随着账号和密钥的固定终身不变。这个值一旦泄漏除非修改密钥，否则对与PIN Offset无法保证安全

**IBM主机系统使用的PIN Offset算法**

密钥： DESede 128Bits

输入：

* 账号PAN（12-16） 转换为 64Bits数据 一般的是对PAN去除校验位，右对齐然后左补零
* PIN长度 4-12 以Hex半字节表示 `0x4`-`0xC`，
* 16进制转换表即如何将`0123456789ABCDEF` 对应成相同字节的 诸如`0123456789012345`这样的对应关系
* 客户设置的私人密码 PIN

规则表述：

1. 使用 密钥对 PAN块 加密DESede后，产生对应的结果，
2. 使用16进制转换表将结果中的数据进行替换，生成纯数字的表
3. 左对齐，从左开始截取 PIN长度个数字                                                       
4. 将 3的PIN与用户设置的PIN对齐，逐位相加并 模10 
5. 结果即为对应的PINoffset ，这个offset的长度就是 PIN check length

> 业务系统存储的PIN就是这个密文的PIN Offset，依此保障客户密码不会被业务系统人员知晓

**IBM主机系统使用的PIN校验算法**

密钥： DESede 128Bits

输入：

* 账号PAN（12-16） 转换为 64Bits PAN块 一般的是对PAN去除校验位，右对齐然后左补零
* PIN长度 4-12 以Hex半字节表示 `0x4`-`0xC`，
* 16进制转换表即如何将`0123456789ABCDEF` 对应成相同字节的 诸如`0123456789012345`这样的对应关系
* 业务系统存储的PIN Offset数据（存储的加密的客户密码）
* 待验证的明文PIN

规则表述：

1. 使用 密钥对 PAN块 加密DESede后，产生对应的结果，
2. 使用16进制转换表将结果中的数据进行替换，生成纯数字的表
3. 左对齐，从左开始截取 需要校验的PIN长度个数字
4. 左对齐，从做开始截取PIN Offset 需要校验的 PIN长度个数字
5. 将3和4进行逐位相加模10
6. 对比5 和客户输入的 PIN是否一致

输出：校验通过或者不通过

> 算法优点： 当客户设置6位密码时，可以仅验证前4位。

### 

