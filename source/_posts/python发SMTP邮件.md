---
title: python发SMTP附件邮件
date: 2017-05-04 11:31:08
categories: "Python"
tags: ["Python","轮子"]
---

遇到个小需求，需要写个小脚本来自动发附件邮件，直接上code。希望能给有需要的朋友一点提示。

### import
主要用到两个模块，email模块构建邮件，smtplib模块发送邮件。
import部分
<!--more-->
```
# -*- coding: UTF-8 -*-
from email import encoders
from email.header import Header                 # 头信息相关
from email.mime.text import MIMEText            # 邮件正文相关
from email.MIMEMultipart import MIMEMultipart   # 邮件对象
from email.MIMEBase import MIMEBase             # 附件设置
from email.utils import parseaddr, formataddr   # 邮件地址
import smtplib                                  # 发送邮件
```

### 处理邮件地址
这里提供了一个方法用来处理邮件的发送和接收地址
```
def _format_addr(s):
    name, addr = parseaddr(s)
    return formataddr(( \
           Header(name, 'utf-8').encode(), \
           addr.encode('utf-8') if isinstance(addr, unicode) else addr))                       
```

### 配置发送和接收方
我们日常使用的邮箱一般都有自己的smtp服务，比我我这里用的163邮箱，对应的smtp_server是'smtp.163.com'。
```
# 发送接收方信息
我们日常使用的邮箱一般都有自己的smtp服务，比我这里用的163邮箱，对应的smtp_server是'smtp.163.com'。
from_addr = 'm15913143122@163.com'
password = 'XXXXXXXXX'
to_addr = '506255560@qq.com'
smtp_server = 'smtp.163.com'                       
```

### 邮件对象
这只发送方和接收方
```
msg = MIMEMultipart()
msg['From'] = _format_addr(u'xuym <%s>' % from_addr)      # 发送方
msg['To'] = _format_addr(u'xiaoxian <%s>' % to_addr)      # 接收方
msg['Subject'] = Header(u'标题', 'utf-8').encode()  # 邮件标题                    
```
接下来是邮件正文
```
# 邮件正文是MIMEText:
msg.attach(MIMEText('正文', 'plain', 'utf-8'))
```
邮件附件（有附件的话）
我的例子是发送zip压缩文件的附件邮件，这里应该还会涉及利用python压缩文件，这里就不多说啦。
```
with open('/Users/xuym/Desktop/python/qa.zip', 'rb') as f:    # 附件的路径
     # 设置附件的MIME和文件名，这里是zip类型:
     mime = MIMEBase('zip', 'zip', filename='test.zip')       # 附件类型，文件名
     # 加上必要的头信息:
     mime.add_header('Content-Disposition', 'attachment', filename='test.zip')
     mime.add_header('Content-ID', '<0>')
     mime.add_header('X-Attachment-Id', '0')
     # 把附件的内容读进来:
     mime.set_payload(f.read())
     # 用Base64编码:
     encoders.encode_base64(mime)
     # 添加到MIMEMultipart:
     msg.attach(mime)
```

### 发送邮件
```
# 发送邮件
server = smtplib.SMTP(smtp_server, 25)
server.set_debuglevel(1)                                # 这里可以打印出和SMTP服务器交互的所有信息
server.login(from_addr, password)
server.sendmail(from_addr, [to_addr], msg.as_string())
server.quit()
```
