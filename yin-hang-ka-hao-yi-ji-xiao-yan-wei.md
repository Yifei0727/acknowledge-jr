# 银行卡号以及校验位

### 

> 【银行磁条卡磁道格式和使用规范 JR/T 0009-2000】
>
> 【银行卡磁条信息格式和使用规范 GB/T 19584-2004】
>
> 【银联卡发卡行标识代码及卡号 Q/CUP 002-2006】

[Luhn](https://baike.baidu.com/item/Luhn%E7%AE%97%E6%B3%95/22799984?fr=aladdin) 算法

#### 卡片磁道信息

> 磁道信息，参见【银行卡磁条信息格式和使用规范 GB/T 19584-2004】 包含 了3磁道
>
> 在发卡阶段需要计算CVN和CVN2 用于制卡（磁道信息以及卡片信息）

1. 第一磁道 最大 79 字节信息，包含（这里不关注顺序）
   * 主账号（13-19位银行账号数字）
   * 失效日期 4位数字（YYMM）
   * 服务代码 3位数字
   * 持卡人姓名（2-26）
2. 第二磁道 最大40字节信息， 用于磁卡交易包含
   * 主账号（13-19位银行账号数字）
   * 失效日期 4位数字（YYMM）
   * 服务代码 3位数字
   * 附件数据 3位数字（即CVN）
3. 第三磁道 最大 107字节，

   ```text
    主要包含交易状态等信息，这里不做过多说明。

    卡片CVN计算方法在附录中也有说明，核心元素为 二磁道的4个元素。
   ```

由于磁条卡中的磁道信息都直接存储在磁道中，如果有一个读取磁道的设备就可以将这些信息都读取出来，同时某个不法分子动下心思，将这些信息留存然后写入到另一张空白的磁条卡，那么他将有一个一模一样的卡片这样就存在很大的隐患，新闻上也有不少此类报道（POS设备、ATM设备）都是一些不法之徒利用一些媒介先获取词条的信息然后利用远程摄像头等设备记录输入的PIN密码然后利用伪造的卡片和记录的密码窃取用户的钱财。正是磁条卡的不安全性，所以目前各大卡组织都在推广（强制要求使用）IC卡（智能芯片卡）作为持卡人身份鉴别卡片。

