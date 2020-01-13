# 消息鉴别算法

### 

在金融领域以消息鉴别码（Message Authentication Code 简称 MAC）进行消息认证防篡改

基于以上对称算法的 鉴别算法计算时需要考虑以下几点（网络数据流都是高位）

* 当给定的消息后，需要对数据分块计算，如果数据不足以进行加密运算的最小块长度如何进行操作
* 正好足够，是否按照填充规则填充
* 需要填充，规则如何

[_ISO/IEC 9797-1_ Padding](https://en.wikipedia.org/wiki/ISO/IEC_9797-1#Padding) 中常见的如下两种

1. 按照 bit为单位来讲 不足的直接在后面按照bit 位 是 0 来算 也就是 所谓的 填充 0x00
2. 按照 bit为单位来讲 不足的直接在后面先填充 一位 bit 1，不足继续填充 bit 0，也就是所谓的填充 0x80 然后填充 0x00  

    基于商算、国算的金融业务算法填充也就这两种。但基于数据加密的还有PKCS5Padding/PKCS7Padding 关于数据填充的更多规则/标准参见[附录](fu-lu/untitled/shu-ju-tian-chong-gui-ze-biao-zhun.md)

当数据完成填充,可以划分为完整块后,我们还需要关心密钥如何使用

1. 密钥如何使用
   * 直接用于进行数据MAC                                                普通的MAC
   * 密钥进行离散后进行数据MAC                                    PBOC中离散密钥进行MAC
   * 密钥进行离散后再次进行分散，然后进行MAC        IC卡脚本MAC     （EMV PBOC）
2. 基于以上理论，可以根据采用密钥加密的方式，填充规则等归类

MAC实际的标准有很多,如下是常见标准中的摘录

#### X9.9 MAC算法 ANSI-X9.9

> The Financial Institution \(Wholesale\) Message Authentication Standard \(ANSI X9.9-1986\)
>
> 即CBC-MAC \([ISO/IEC 9797-1 Algorithm 1](https://en.wikipedia.org/wiki/ISO/IEC_9797-1#MAC_algorithm_1)\) 但可以指 3DES-CBC-MAC

输入密钥：64bits（奇校验可选）

密钥算法：DES-CBC 密钥长度 64Bits，分组长度 64Bits， 初始化IV 全0

填充规则：不足时填充 bit 0

输出：8Byte 加密结果即为MAC

#### X9.19 MAC算法 ANSI-X9.19

> The Financial Institution \(Retail\) Message Authentication Standard, ANSI X9.19 Optional Procedure 1 \([ISO/IEC 9797-1 Algorithm 3](https://en.wikipedia.org/wiki/ISO/IEC_9797-1#MAC_algorithm_3)\)

输入密钥：128bits（奇校验可选）

密钥算法：DES-CBC 密钥长度 64Bits，分组长度 64Bits， 初始化IV 全0

填充规则：不足时填充 bit 0

算法描述：先取 密钥左半部分 64bits，作为密钥进行DES-CBC MAC，取（结果，密钥右半部分 64bits）再次进行DES解密，取（结果，密钥前 64bits）最后进行DES加密，取结果

输出： 8Byte 加密结果即为 MAC

#### 银联联机MAC算法

> 为保证数据的安全传输，网络中的报文采用了PIN加密和报文来源正确性鉴别\(MAC\)两种加密技术。
>
> 基于DES/3DES算法的PIN、MAC长度为8字节；
>
> 基于SM4算法的PIN为16字节，MAC长度为8字节。
>
> 【中国银联银行卡交换系统技术规范 第4部分 数据安全传输控制规范 Q/CUP 006.4-2016】

* 国际算法

  > 同 X9.9 MAC 规则，但密钥长度支持 双倍、三倍长度

  密钥输入：64Bits 、128Bits、192Bits （奇校验可选）

  密钥算法： DESede-CBC 密钥长度 ，分组长度 64Bits， 初始化IV 全0，

  填充规则： 不足时填充 bit 0

  算法描述： 密钥及算法 DESede-CBC 进行计算 ，取最后一块结果

  输出 8Byte MAC结果

* 国密 128Bits

  > 同 X9.9 MAC 规则，但算法替换为SM4 块大小对应为16Byte

  密钥输入： 128Bits

  密钥算法： SM4-CBC 初始化IV 全0， 分组长度 128Bits，

  填充规则：不足时填充 bit 0

  算法描述：对数据进行SM4-CBC模式分组加密，取最后一块结果

  输出： 16Byte MAC结果（实际）/ 左半部分8Byte MAC结果（规范）

#### 银联商务MAC算法

> 算法说明参见【中国银联POS终端规范 Q/CUP 007-2006】
>
> 算法说明参见【中国银联银联卡受理终端应用规范 第一部分 POS终端规范 Q/CUP 009】

* 国际算法时

  > 将数据分组异或后取最后一块，对最后一块进行 DESede加密

  密钥输入：64Bits 、128Bits、192Bits （奇校验可选）

  密钥算法：DESede-ECB， 分组长度 64Bits，

  填充规则：不足时填充 bit 0

  算法描述：对数据进行分组后进行异或计算，取（结果，密钥）进行DESede加密，取结果

  输出：4Byte MAC结果 即 8Hex MAC结果

* 国密算法时

  暂无，参见【中国银联联机报文MAC算法 Q/CUP 006】或【中国银联银行卡联网联合技术规范V2.1\(金融IC卡国密算法升级\)】

#### 智能卡脚本MAC

> 在PBOC1.0 电子钱包中使用离散子密钥进行MAC计算，PBOC3.0中已经移除了电子钱包

此类多为IC卡（智能卡），需要进行密钥离散，对于卡片/服务器 操作稍有不同

参见IC卡部分的[IC卡脚本MAC](ic-ka-jiao-ben-an-quan-xiang-guan.md#ic-ka-jiao-ben-mac)

