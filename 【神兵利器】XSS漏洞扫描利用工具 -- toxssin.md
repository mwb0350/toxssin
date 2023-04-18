# 【神兵利器】XSS漏洞扫描利用工具 -- toxssin

2023-03-03本文共 612 字阅读完需 3 分钟

**0x01 工具介绍**

toxssin 是一种开源渗透测试工具，可自动执行跨站脚本 (XSS) 漏洞利用过程。它由一个 https 服务器组成，它充当为该工具 (toxin.js) 提供动力的恶意 JavaScript 有效负载生成的流量的解释器。

**0x02 安装与使用**

1、安装需要的依赖库

```bash
git clone https://github.com/t3l3machus/toxssin
cd ./toxssin
pip3 install -r requirements.txt
```

2、要启动 toxssin.py，您需要提供 ssl 证书和私钥文件。

如果您不拥有具有受信任证书的域，则可以使用以下命令颁发和使用自签名证书（尽管这不会让您走得太远）：

```apache
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365
```

3、强烈建议使用受信任的证书运行 toxssin（请参阅本文档中的如何获取有效证书）。也就是说，您可以像这样启动 toxssin 服务器：

```awk
python3 toxssin.py -u https://your.domain.com -c /your/certificate.pem -k /your/privkey.pem
```

4、运行效果

![img](https://img.nsg.cn/img/article/2023/04/e081b43b-e83d-4f40-bf32-f6350b018878.png) 



**0x03 下载地址**

项目地址：https://github.com/t3l3machus/toxssin