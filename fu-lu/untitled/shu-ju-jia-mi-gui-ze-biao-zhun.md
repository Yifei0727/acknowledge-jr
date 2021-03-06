---
description: 常见加密模式
---

# 数据加密规则\(标准\)

金融领域常见标准中至少实现的

* ECB
* CBC

关于这些常见的模式,有人在github 上做了汇总, 可以[参考这里](https://github.com/GmSSL/documents/blob/master/standards/)

文中有提到 [`FIPS`](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standards) `(Federal Information Processing Standard)` 算是一种计数标准集, 某些方面可以类比于 `GM/T 国密标准` 某些方面,会提到 `FIPS认证` 也是基于 FIPS 某一公开标准检测通过而来,在国内取而代之的是 `国密认证` 

对这些常见模式进行简单说明之前,有必要提下`对称算法` 前面我们提到 

> 对称算法: 加密方和解密方使用相同的密钥进行运算进行信息的加密/还原
>
> 非对称算法: 加密方使用 公钥\(可以公开发布的密钥\)进行信息加密, 解密方使用私钥\(只有解密方自身持有的密钥\) 进行信息还原

这是从密钥角度来看的,  这也是为什么`对称算法` 也在某些书/文章中称为`私钥算法` 的原因.

对称算法 也叫分组算法 是因为,现在的基础算法都是对消息进行分块然后加密的,例如 使用`DES算法和`密钥 `K`加密 一段 `Hello World!` 文本, 由于 DES 每次只处理 `64bit` 大小的数据\(称之为`块`\) 所以这段文本其实需要进行分组然后执行运算

```text
Hello Wo| 48 65 6C 6C 6F 20 77 6F
rld!    | 72 6C 64 21        
```

然后DES加密 第一块数据,再加密第二块数据. 特别说明的时通常的块加密算法要求送入的块数据是正好与算法的块一致的 称为为 `块大小 (BlockSize)`  如果数据分组不满足块大小,则需要进行_数据填充_ 如何对数据填充以及填充并加密后,解密方如何对填充的消息去除填充也有许多不同的方法,后面会提到[ 数据填充规则](shu-ju-tian-chong-gui-ze-biao-zhun.md)

这里使用最简单文本加密填充规则,不足的 按照 0x00 也就是 C语言中字符串的结束标志 `\0`  即

```text
Hello Wo| 48 65 6C 6C 6F 20 77 6F
rld!    | 72 6C 64 21 00 00 00 00     
```

这样消息原文被分成了 2 组 记作 `B1`,`B2`, 即2个待加密的消息`块` 然后就可以使用DES对这些块进行加密操作了.

如果每个块都是被密钥`K`加密, 加密后的块对应记作`B1'` `B2'`按照原来的顺序存放, 这种方式就称为 [`ECB`](../../chang-jian-suo-lve-ci.md#ecb) 模式,这是最常见最简单的模式,但 ECB 有个[最致命的问题](https://blog.filippo.io/the-ecb-penguin/) 所以在对大数据/文件加密时一般并不推荐使用ECB模式.

有一种称为 [CBC](../../chang-jian-suo-lve-ci.md#cbc) 的加密模式, 同时 引入了 [`IV`](../../chang-jian-suo-lve-ci.md#iv) ,过程大致如下

1. 数据分组后,消息被划分为 B1,B2,B3....Bn
2. 在初次加密前, 使用 IV 与 B1 进行 [异或运算](https://en.wikipedia.org/wiki/Exclusive_or) 得到 临时块 记作 `Bi1`
3. 使用密钥`K` 对`Bi1` 加密得到 `Bi1'` 
4. 使用 `Bi1'` 与 B2 进行 异或运算 得到临时块,记作 `Bi2`
5. 使用密钥 `K` 对`Bi2` 加密得到 `Bi2'`
6. 循环这种操作 到最后一块

可以看到, 块与块之间 现在有了联系,正如其名 `Cipher Block Chaining` 上次加密得到到 `密文块` 参与本次加密,这种块与块之间的联系不是`链` 是什么?

解密时如何操作

1. 密文数据分组\( 加密后的数据必定成组 Block\) `Bi1'` `Bi2'` ... `Bin'`
2. 使用密钥`K` 对 `Bi1'` 解密 得到 `Bi1` 
3. 使用`Bi1` 与 IV 进行 异或运算 得到 原文 `B1`
4. 使用密钥`K` 对 `Bi2'` 解密得到 `Bi2`
5. 使用 `Bi2` 与 `Bi1'` 进行异或运算 得到 原文 `B2`
6. 重复
7. 使用密钥 `K` 对 `Bin'` 解密得到 `Bin`
8. 使用 `Bin` 与 `Bin-1` 进行 异或运算 得到 原文 `Bn`
9. 原文排序 得到 原始数据

然后按照最初分组前的填充规则去除填充

思考: 如果解密时 某位数据错误 会怎样?

* ECB  所在块解密对应的出错, 但不影响其他块
* CBC  所在块解密对应的出错, 因为下一块解密使用本块,导致下一块解密也出错,但不影响更多块



