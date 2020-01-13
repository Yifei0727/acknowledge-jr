# 金融业务常用算法说明



### 从算法来看

1. 磁条卡鉴别
2. IC卡鉴别
3. 报文鉴别 MAC
4. 数据保护（机密数据加密）
5. 密码保护（PIN保护--PIN传输、PIN存储、PIN校验）
6. 手机支付 闪付 NFC-[HCE](https://en.wikipedia.org/wiki/Host_card_emulation) [Samsung-MST](https://en.wikipedia.org/wiki/Magnetic_secure_transmission) 各种Pay（Apple Pay HuaweiPay MiPay 。。。）
7. 支付宝、微信的二维码支付、银联二维码支付 目前个人推断 二者原理不大相同
8. 6和7都属于标记化支付手段
9. 其他支付形式（指纹支付、虹膜支付、扫脸支付、声纹支付等等）不属于本文的范畴，参见[这里](http://blog.csdn.net/starryheavens/article/details/50590485)扫盲
10. 联机服务
11. 发卡服务

关于 TOKEN支付技术以及ETOKEN以及签名验签

1. 计算 - 产生 都是一种正向结果，即输入必需信息输出一个结果
2. 验证 - 校验 都是一种确认结果，根据参数进行1 的操作，然后将给定预期结果与1的结果对比，相同为真，不同为假

