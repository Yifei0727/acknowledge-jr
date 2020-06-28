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

#### 关于 PKCS5 和 PKCS7 

两者都是对数据进行填充，但是实际规则有一定区别的，不过多数（算法库）实现都会忽略这种差异性，在实现时将两者作为同一个规则标识

PKCS5 中本来限定了算法为 DES/DESede 由于DES 是 64bit 即 8字节 为一块的，所以 PKCS5 隐式的其实限定死了只能对数据以 8字节为分块进行填充，这种情况下 某些实现 就会表现出差异性造成“不兼容”的问题；PKCS7则 是动态， 一般地限定 分块可以是 1-256（均不含） ，PKCS7 根据实际算法块长决定 即对于 DES 同 PKCS5 为8字节，对于AES/SM4则 为16  字节

例如 对 `FF FF FF FF FF FF` 进行填充 

> 当采用DES 作为算法时， PKCS5、PKCS7 填充结果都是 `FF FF FF FF FF FF 02 02`

> 当采用 SM4 /AES作为算法时， 则会表现出差异  
>
> > 有的实现 都是 以算法 对应块长 作为标准的 此时 均为 `FF FF FF FF FF FF 0A 0A 0A 0A 0A 0A 0A 0A 0A 0A`
>
> > 有的实现按照PKCS5规范取 8 作为了块长 作为标准 
> >
> > 对于1-7  字节的数据 则可能存在问题
> >
> > * PKCS5 `FF FF FF FF FF FF 02 02` 
> > * PKCS7 `FF FF FF FF FF FF 0A 0A 0A 0A 0A 0A 0A 0A 0A 0A` 
> >
> > 对于 8-15 字节的数据 例如8字节数据 均为 `FF FF FF FF FF FF FF FF 08 08 08 08 08 08 08 08`

             

那些PKCS5 以定长作为标准的可能存在差异性（但这种反而才是依照文档规范实现的 \[滑稽\]）

> 数据长度：原始数据长度模 16 的结果
>
> 算法： DES、DESede 为 8 字节分组， AES、SM4以16字节分组
>
> PKCS5 /PKCS7： 针对所在行算法和数据长度，对应实现取的 块长度
>
> 差异性： 对应实现填充后，是否可能存在兼容性问题

| 数据长度 | 算法 | PKCS5 | PKCS7 | 差异性 |
| :--- | :--- | :--- | :--- | :---: |
| 1-7 | DES | 8 | 8 | 无 |
| 8-15 | DES | 8 | 8 | 无 |
| 1-7 | AES/SM4 | \*  8 | 16 | 有 |
| 1-7 | AES/SM4 | \* 16 | 16 | 无 |
| 8-15 | AES/SM4 | 8 | 16 | 无 |
| 8-15 | AES/SM4 | 16 | 16 | 无 |



