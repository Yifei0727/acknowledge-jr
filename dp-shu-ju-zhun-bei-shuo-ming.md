# DP 数据准备说明

DP阶段

```text
 加密 DES、RSA密钥、PIN密文必需使用TDES的ECB模式
```

​ 个人化设备 使用SKU\[dek\] ECB模式进行加密数据和密钥

外部认证指令 需要MAC和加密时 使用 CBC模式，IV 全0 ，强制填充 `0x80`

SKU\[enc\] 

