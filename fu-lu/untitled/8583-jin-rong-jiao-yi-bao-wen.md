# 8583金融交易报文



### ISO 8583 金融交易消息格式

> Financial Transaction Message Format

bitmap 方式，有64bit 和 128bit两种

涉及交易认证的关键元素位（MAC需要用的大部分数据域，这里不做说明）

* 2 Primary account number \(PAN\)     n ..19                           存放主账号
* 23 Card sequence number                n ..3                             存放卡序列号
* 35 Track 2 data                                     z ..37                           存放二磁道信息
* 52 Personal identification number \(PIN\) data     b ..16       存放PINBLOCK
* 55  ISO保留【银联规范中规定 Q/CUP 006.2  6.44】    b ..255    CUPS系统 IC卡数据域 
* 64 Message authentication code \(MAC\) field      b ..16        存放MAC值
* 128 Message authentication code \(MAC\) field    b ..16   【如果有】



