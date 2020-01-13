# IC卡密钥分散

### 密钥分散\(Key Derivation）：

主要涵盖了 EMV/PBOC/VIS/MChip 等规范中对 IC卡卡片密钥计算流程

#### 卡片应用密钥及其会话密钥

**发卡行主密钥生成唯一卡片应用密钥**

> MDK\[ac\] -&gt; UDK \[ac\]
>
> 输入 128Bits 主密钥MDK\[ac\]、PAN（卡片账号）、PANSN（卡片序号）
>
> 输出 128Bits 唯一卡片密钥UDK\[ac\]

* EMV 4.1 OptionA 【 EMV 4.1 Book2 A1.4 】

  PAN\(number format\) SN\(number format\) 构成16位

  （VIS1.4.0） 如果长于16 保留16，如果短于16则左补0

  DESede\(MDK, PAN\|\|SN\) \|\|DESede\(MDK, PAN\|\|SN XOR FFFFFFFFFFFFFFFF\)

  适用于 **VIS1.4** 、**M/Chip4** 、**PBOC2.0**

* EMV 4.1 OptionB 【 EMV 4.1 Book2 A1.4 】

  当PAN长度长于 16时 进行SHA-1然后提取出16位账号，进行OptionA

* PBOC3.0 国密 QCUPV2.1 国密

  SM4\(MDK, \(PAN\|\|SN\) \|\|\(PAN\|\|SN XOR FFFFFFFFFFFFFFFF\)\)

**卡片唯一密钥生成会话密钥**

> UDK\[ac\] -&gt; SessionKey\[ac\]
>
> 输入 128Bits 卡片主密钥、16Bits ATC
>
> 输出 128Bits 会话密钥

* EMV2000 会话密钥 EMV 4.0 Book2 Annex A1.3.1

  16Byte UDK\[ac\]，2Byte ATC, 输出 16Byte SessionKey

  离散函数需要两个参数 b H

  H 即 The Height of tree ，可以理解为 几级密钥

  b 即 The branch factor ，一个密钥可以离散出多少个子密钥

  0 ≤ i ≤ H 第 i 级 密钥有 b\*\*i 个子密钥

  bH 常用的值

  * b:2  H:16
  * b:4  H:8     EMV 推荐

* M/Chip 4

  > 额外需要 4Byte UN\(Unpredictable Number\)
  >
  > 输出需要对KEY做 奇校验

​ SK\[ac\] := DESede\(UDK, ATC\|\|0F00\|\| UN\) \|\| DESede\(UDK, ATC\|\|F000\|\| UN\)

* VIS 1.4.0
  * CVN10 中直接采用UDK计算 无需离散

    SK\[ac\] := UDK\[ac\]

  * CVN14 中使用EMV2000 中的会话密钥离散方式
* PBOC2.0 Q/CUP v2.0 v2.1

  > 计算得到的过程密钥必须满足奇校验要求。

  SK\[ac\] := DESede\(UDK, 000000000000\|\|ATC\) \|\| DESede\(UDK, 000000000000\|\|\(ATC XOR FFFF\)\)

* PBOC3.0 Q/CUP v2.1国密

  SK\[ac\] := SM4\(UDK, 000000000000\|\|ATC\) \|\| DESede\(UDK, 000000000000\|\|\(ATC XOR FFFF\)\)

#### 卡片脚本MAC密钥及其会话密钥

**发卡行密钥生成唯一卡片MAC密钥**

> MDK\[mac\] -&gt; UDK\[mac\]

参见对应UDK\[ac\]的计算方式，只是Key对应为MAC的Key

**唯一卡片密钥生成会话密钥**

> UDK\[smi\] -&gt; SessionKey\[smi\]

* M/Chip 4

  > * The Triple-DES Encryption of X = “the input with the third byte replaced by ‘F0‘ ” 
  > * The Triple-DES Encryption of Y = “the input with the third byte replaced by ‘0F’ ”

  SK\[smi\] := DES3\(MK\[smi\],\[X\]\) \|\| DES3\(MK\[smi\],\[Y\]\)

  对 8Byte AC作为 X，Y，将X的第3字节替换为 `0xF0`，将Y的第3字节替换为`0x0F`

  然后进行加密即为 会话密钥 SK\[mac\]

* VIS 1.4.0

  提供了两种方法

  1. 使用UDK\[smi\]和ATC产生

     SK\[smi\_left\] := UDK\[mac\_left\] XOR \(000000000000 \|\| ATC\)

     SK\[smi\_right\] := UDK\[mac\_right\] XOR \(00000000 \|\| \(ATC XOR FFFF\)\)

     SK\[smi\] := SK\[smi\_left\] \|\| SK\[smi\_right\]

  2. 参见EMV 4.0 Book2 Annex A1.3.1 也就是 同VIS1.4.0 中的 CVN14方式

* PBOC3.0 国际 即PBOC2.0

  参见同PBOC2.0 中的 SessionKey\[ac\]生成方式，但ac需替换为smi

* PBOC3.0 国密

  参见同PBOC3.0 中的 SessionKey\[ac\]生成方式，但ac需替换为smi

  ​

#### 卡片脚本加密密钥及其会话密钥

**发卡行密钥生成唯一卡片加密密钥**

> MDK\[smc\] -&gt;UDK\[smc\]

同AC生成方式

**唯一卡片密钥生成会话密钥**

> UDK\[smc\] -&gt; SessionKey\[smc\]

* M/Chip 4

  > * The Triple-DES Encryption of X = “the input with the third byte replaced by ‘F0‘ ” 
  > * The Triple-DES Encryption of Y = “the input with the third byte replaced by ‘0F’ ”

  SK\[smc\] = DES3\(MK\[smc\]\)\[X\] \|\| DES3\(MK\[smc\]\)\[Y\]:

  对 8Byte AC作为 X，Y，将X的第3字节替换为 `0xF0`，将Y的第3字节替换为`0x0F`

  然后进行加密即为 会话密钥 SK\[smc\]

* VIS 1.4.0
* PBOC3.0 国际 即PBOC2.0

  参见同PBOC2.0 中的 SessionKey\[smi\]生成方式，但smi需替换为smc

* PBOC3.0 国密

  参见同PBOC3.0 中的 SessionKey\[smi\]生成方式，但smi需替换为smc

### 

### 

