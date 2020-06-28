---
description: 个人化(Personalization) 是将数据/应用写入到IC卡并让IC卡装载应用的过程
---

# 个人化说明

> \[EMV Card Personalization Specification 2003\]
>
> \[GlobalPlatform Load and Personalization Interface Specification\]

通常的,加密时对密钥 使用ECB模式,不填充\(不足时填充0x00\);对数据使用CBC模式,强制填充 0x80

CBC无初始化向量,或者通常可以认为初始化向量为全 0x00

| 名称 | 说明 |
| :--- | :--- |
| KMC | 卡片主控密钥 |
| K | 卡片密钥, 由 KMC 分散得到 |
| SKU | 卡片会话密钥/过程密钥,由K 离散得到 |
| ENC | 加密用途\(报文数据/通信数据\) |
| MAC | 鉴别用途\(报文鉴别\) |
| DEK | 加密用途\(敏感数据\) |

### 计算卡片密钥\(通信\)

KEYDATA := KMC\_id\(6Byte\) \|\| [CSN](chang-jian-suo-lve-ci.md#csn) \(4Byte\)

* ENC
  * 用于 IC卡认证,解密StoreData 数据
  * PAD\_L := "F001"
  * PAD\_R := "0F01"
* MAC
  * PAD\_L :="F002"
  * PAD\_R := "0F02"
* DEK
  * PAD\_L := "F003"
  * PAD\_R := "0F03"

分散因子 `K_GENE_L` := KEYDATA\[2:8\] \|\| PAD\_L

分散因子 `K_GENE_R` := KEYDATA\[2:8\] \|\| PAD\_R

使用`卡片主控密钥`加密`分散因子`,即得到对应的`卡片密钥(通信)` 三把,分别标记为 `K_ENC` `K_MAC` `K_DEK`

### 计算过程密钥

> Derivation Data for Session Keys  \(DES CBC mode\)

* ENC
  * PAD = "0182"
  * SKU\_ENC 使用CBC模式 加密APDU指令数据
* MAC
  * PAD = "0101"
* DEK
  * PAD = "0181"
  * SKU\_DEK使用ECB模式 加密密钥和敏感数据 eg PINBlock/ICC\_privateKey/UDK

分散因子 SKU := PAD\(2Byte\) \|\| 初始化时卡片产生的随机数sequence counter\(2Byte\) \|\| 000000...0000\(12Byte\)

使用 `K` 对于加密`分散因子` 即可对应的`过程密钥`,三把 分别标记为 `SKU_ENC` `SKU_MAC` `SKU_DEK`

ℹ 个人化设备使用[TK](chang-jian-suo-lve-ci.md#tk)\_from\_DP 转加密为 SKU\_XXX ,将敏感数据从DP交付给卡片

