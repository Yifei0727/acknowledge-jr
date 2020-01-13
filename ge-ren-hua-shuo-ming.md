# 个人化说明

> \[EMV Card Personalization Specification 2003\]
>
> \[GlobalPlatform Load and Personalization Interface Specification\]

通常的,加密时对密钥 使用ECB模式,不填充\(不足时填充0x00\);对数据使用CBC模式,强制填充 0x80

CBC无初始化向量,或者通常可以认为初始化向量为全 0x00

KMC  

CSN Chip Serial Number

KEYDATA := KMC\_id\(6Byte\) \|\| CSN \(4Byte\)

GeneLeft := KEYDATA\[2:8\] \|\| PAD\_LEFT

GeneRight := KEYDATA\[2:8\] \|\| PAD\_RIGHT

* ENC
  * LEFT:PAD = "F001"
  * RIGHT:PAD = "0F01"
* MAC
  * LEFT:PAD = "F002"
  * RIGHT:PAD = "0F02"
* DEK
  * LEFT:PAD = "F003"
  * RIGHT:PAD = "0F03" 

ENC 即 K\_ENC 用于 IC卡认证,解密StoreData 数据

计算过程密钥

> Derivation Data for Session Keys  \(DES CBC mode\)
>
> Gene\_SKU := PAD\(2Byte\)\|\|  初始化时卡片产生的随机数sequence counter\(2Byte\) \|\| 000000...0000\(12Byte\)
>
> K\_XXK  -Gene\_SKU-&gt; SKU\_XXK

* ENC
  * PAD = "0182"
  * SKU\_ENC 使用CBC模式 加密APDU指令数据
* MAC
  * PAD = "0101"
* DEK
  * PAD = "0181"
  * SKU\_DEK使用ECB模式 加密密钥和敏感数据 eg PINBlock/ICC\_privateKey/UDK

ℹ 个人化设备使用TK\_from\_DP 转加密为 SKU\_XXX ,将敏感数据交付给卡片

