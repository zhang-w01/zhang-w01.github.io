---
layout: post
title: PUF密钥管理论文
categories: [PUF]
description: some word here
keywords: C++
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false


---

# Software Implementation of a SRAM PUF-Based Password Manager

**解决问题：** 提出一种密钥管理协议，减少网络物理系统的攻击，例如ID-密码。

**方法** ：哈希函数+可寻址PUF生成器(APG)

方案一：

哈希函数解决方案：

![](C:\Users\zw\AppData\Roaming\marktext\images\2024-08-06-21-24-01-image.png)

 地址：$h(UID⊕PW)$ 

值：$h(PW)$ 

**缺点** ：数据库泄露之后，存在暴力破解的可能。

方案二：

将密码hash，将哈希值输入APG，puf的输出存储在数据库。

![](C:\Users\zw\AppData\Roaming\marktext\images\2024-08-06-21-37-15-image.png)

**缺点：** 如果知道ID的话，可以测试常用的密码，与ID进行异或，查找数据库中是否有相同的地址。

方案三：将地址也用puf的输出

![](C:\Users\zw\AppData\Roaming\marktext\images\2024-08-06-21-42-04-image.png)

**优点** ：黑客即使访问到数据库，也无法读取或理解数据库中的信息。

因为sram puf不稳定

如果是值发生翻转，错误率在6%之内，则接受响应。

如果是地址值发生翻转，检查每一位发生翻转的可能。

![](C:\Users\zw\AppData\Roaming\marktext\images\2024-08-06-22-19-33-image.png)
