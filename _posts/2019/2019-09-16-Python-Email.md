---
layout: post
title: Python Email
category: python
tags: [python,email]
---

# Python Email #

## 发邮件 ##

SMTP是发送邮件的协议，Python内置对SMTP的支持，可以发送纯文本邮件、HTML邮件以及带附件的邮件。

Python对SMTP支持有smtplib和email两个模块，email负责构造邮件，smtplib负责发送邮件。

简单的发送邮件可以包括，发送文本、带附件发送、发送html

### 样例代码 ###

```python 
# -*- coding utf-8 -*-

from email import encoders
from email.header import Header
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.base import MIMEBase
from email.utils import parseaddr, formataddr

import smtplib


def _format_addr(s):
    name, addr = parseaddr(s)
    return formataddr((Header(name, 'utf-8').encode(), addr))

# 普通文本
def _text_msg():
    return MIMEText('hello , send by python ', 'plain', 'utf-8');

# 带附件
def _attachment_msg():
    msg = MIMEMultipart()
    msg.attach(MIMEText('send with file...', 'plain', 'utf-8'))
    # 添加附件就是加上一个MIMEBase，从本地读取一个图片:
    with open('C:/Users/vincent.fung/Desktop/2017-03-08-235233.jpg', 'rb') as f:
        # 设置附件的MIME和文件名，这里是png类型:
        mime = MIMEBase('image', 'png', filename='test.jpg')
        # 加上必要的头信息:
        mime.add_header('Content-Disposition', 'attachment', filename='test.jpg')
        mime.add_header('Content-ID', '<0>')
        mime.add_header('X-Attachment-Id', '0')
        # 把附件的内容读进来:
        mime.set_payload(f.read())
        # 用Base64编码:
        encoders.encode_base64(mime)
        # 添加到MIMEMultipart:
        msg.attach(mime)
    return msg;

# 带HTML
def _html_msg():
    msg = MIMEMultipart('alternative');
    msg.attach(MIMEText('hello', 'plain', 'utf-8'))
    msg.attach(MIMEText('<html><body><h1>Hello</h1></body></html>', 'html', 'utf-8'))
    return msg


from_addr = input('From:')
password = input('Password:')
to_addr = input('To : ')
smtp_server = input('SMTP server: ')

msg = _html_msg();
msg['FROM'] = _format_addr('Python Sender <%s>' % from_addr)
msg['To'] = _format_addr('Manager <%s>' % to_addr)
msg['Subject'] = Header('From SMTP say hello ', 'utf-8').encode()

server = smtplib.SMTP(smtp_server, 25)
server.set_debuglevel(1)
server.login(from_addr, password)
server.sendmail(from_addr, [to_addr], msg.as_string())
server.quit()


```
msg['To']接收的是字符串而不是list，如果有多个邮件地址，用,分隔即可。

记得，smtp.163/qq.com.


## 收邮件 ##

Python编写收邮件以POP3 为主，不能使用IMAP 。 当今，IMAP 为主流。 建议先了解一下 POP3 和 IMAP 的区别。

其实工作十多年，我还是真没遇到过写收邮件的场景。

可以参考廖老师的学习内容：
https://www.liaoxuefeng.com/wiki/1016959663602400/1017800447489504


