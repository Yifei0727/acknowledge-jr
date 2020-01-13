# 数据填充规则\(标准\)



#### ANSI X9.23 X.923 数据填充格式

> cipher block chaining

```text
 填充至符合块大小的整数倍，填充值最后一个字节为填充的数量数，其他字节填 0
 原始：FF FF FF FF FF FF FF FF FF
 填充：FF FF FF FF FF FF FF FF FF 00 00 00 00 00 00 07
```

#### ISO 10126  数据填充格式

> 银行消息加密处理

```text
 填充至符合块大小的整数倍，填充值最后一个字节为填充的数量数，其他字节随机处理
 原始：FF FF FF FF FF FF FF FF FF
 填充：FF FF FF FF FF FF FF FF FF 3F 7A B4 09 14 36 07
```

#### ISO 9797-1 数据填充格式：

#### ISO/IEC 9797-1 Padding Method 1

[Padding method 1](https://en.wikipedia.org/wiki/ISO/IEC_9797-1#Padding) 必要时填充 bit0 ，也就是 不足时填充 0x00

```text
 以DES举例 块大小为 8Byte
 原始：FF FF FF FF FF FF FF FF 
 填充：FF FF FF FF FF FF FF FF 
 原始：FF FF FF FF FF FF FF FF FF
 填充：FF FF FF FF FF FF FF FF FF 00 00 00 00 00 00 00
```

#### ISO/IEC 9797-1 Padding Method 2

[Padding method 2](https://en.wikipedia.org/wiki/ISO/IEC_9797-1#Padding) 先填充bit 1，然后必要时填充 bit 0，也就是 强制填充 0x80，不足时填充 0x00

```text
 以DES举例 块大小为 8Byte
 原始：FF FF FF FF FF FF FF FF 
 填充：FF FF FF FF FF FF FF FF 80 00 00 00 00 00 00 00
 原始：FF FF FF FF FF FF FF FF FF
 填充：FF FF FF FF FF FF FF FF FF 80 00 00 00 00 00 00
```

#### ISO/IEC 9797-1 Padding Method 3

极少使用

#### PKCS5 数据填充格式

> [RFC2898](https://tools.ietf.org/html/rfc2898) PKCS \#5: Password-Based Cryptography Specification

```text
 PKCS5 使用DES/3DES算法 块大小固定是8，与PKCS7块为8时 相同，但是PKCS7最大支持块大小为 255 即 0xFF
 填充至符合块大小的整数倍，填充值为填充数量数
 原始：FF FF FF FF FF FF FF FF FF
 填充：FF FF FF FF FF FF FF FF FF 07 07 07 07 07 07 07
 原始：FF FF FF FF FF FF FF 
 填充：FF FF FF FF FF FF FF 01
```

#### PKCS7 数据填充格式

> [RFC2315](https://tools.ietf.org/html/rfc2315) PKCS \#7: Cryptographic Message Syntax
>
> [RFC5652](https://tools.ietf.org/html/rfc5652) Cryptographic Message Syntax \(CMS\)

```text
 填充至符合块大小的整数倍，填充值为填充数量数 
 如果算法为DES/3DES则与PKCS5相同，如果为AES 或者 SM4等块大小为 16Byte 则如下
 原始：FF FF FF FF FF FF FF FF FF
 填充：FF FF FF FF FF FF FF FF FF 07 07 07 07 07 07 07
 原始：FF FF FF FF FF FF FF
 填充：FF FF FF FF FF FF FF 09 09 09 09 09 09 09 09 09
```

