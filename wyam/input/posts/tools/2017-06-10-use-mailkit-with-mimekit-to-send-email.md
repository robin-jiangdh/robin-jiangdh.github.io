---
title: 使用mailkit 和mimekit来发送邮件
Published: 10/24/2019
tags: ['mailkit ','mimekit','smtp','Pop3','工具库'] 
---

## [mailkit](https://github.com/jstedfast/MailKit)和[mimekit](https://github.com/jstedfast/MimeKit) 是什么

> ## What is MailKit?

> MailKit is a cross-platform mail client library built on top of [MimeKit](https://github.com/jstedfast/MimeKit).

> ## What is MimeKit?

> MimeKit is a C# library which may be used for the creation and parsing of messages using the Multipurpose Internet Mail Extension (MIME), as defined by [numerous IETF specifications](https://github.com/jstedfast/MimeKit/blob/master/RFCs.md).

## 怎么用
分为发信和收信功能

### 发信
```csharp
 
	var message = new MimeMessage ();
	message.From.Add (new MailboxAddress ("Joey Tribbiani", "joey@friends.com"));
	message.To.Add (new MailboxAddress ("Mrs. Chanandler Bong", "chandler@friends.com"));
	message.Subject = "How you doin'?";
    message.Body = new TextPart ("plain") {
				Text = @"Hey Chandler,
I just wanted to let you know that
Monica and I were going to go play some paintball, you in?
-- Joey"
			};

	using (var client = new SmtpClient ()) {
		// For demo-purposes, 
        //accept all SSL certificates (in case the server supports STARTTLS)
		client.ServerCertificateValidationCallback = (s,c,h,e) => true;

		client.Connect ("smtp.friends.com", 587, false);

		// Note: only needed if the SMTP server requires authentication
		client.Authenticate ("joey", "password");

		client.Send (message);
		client.Disconnect (true);
	}
```

### 收信
```csharp
using (var client = new Pop3Client ()) {
	// For demo-purposes, 
    //accept all SSL certificates (in case the server supports STARTTLS)
		client.ServerCertificateValidationCallback = (s,c,h,e) => true;

		client.Connect ("pop.friends.com", 110, false);

		client.Authenticate ("joey", "password");

		for (int i = 0; i < client.Count; i++) {
			var message = client.GetMessage (i);
			   Console.WriteLine ("Subject: {0}", message.Subject);
			}
           client.Disconnect (true);
			}
```

## 常见邮件服务提供商及端口

【sina.com】
POP3服务器地址:pop3.sina.com.cn（端口：110）
SMTP服务器地址:smtp.sina.com.cn（端口：25） 

 

【sinaVIP】

POP3服务器:pop3.vip.sina.com （端口：110）

SMTP服务器:smtp.vip.sina.com （端口：25）

 

【sohu.com】

POP3服务器地址:pop3.sohu.com（端口：110）
SMTP服务器地址:smtp.sohu.com（端口：25）

 

【126邮箱】

POP3服务器地址:pop.126.com（端口：110）

SMTP服务器地址:smtp.126.com（端口：25）

 

【139邮箱】

POP3服务器地址：POP.139.com（端口：110）

SMTP服务器地址：SMTP.139.com(端口：25)


【163.com】

POP3服务器地址:pop.163.com（端口：110）
SMTP服务器地址:smtp.163.com（端口：25）

 

【QQ邮箱】

POP3服务器地址：pop.qq.com（端口：110）

SMTP服务器地址：smtp.qq.com（端口：25）

 

【QQ企业邮箱】

POP3服务器地址：pop.exmail.qq.com （SSL启用 端口：995）

SMTP服务器地址：smtp.exmail.qq.com（SSL启用 端口：587/465）

 

【yahoo.com】

POP3服务器地址:pop.mail.yahoo.com（端口：995）
SMTP服务器地址:smtp.mail.yahoo.com（端口：587) 

 

【HotMail】

POP3服务器地址：pop3.live.com（端口：995）

SMTP服务器地址：smtp.live.com（端口：587）

 

【Gmail】
POP3服务器地址:pop.gmail.com（SSL启用端口：995）
SMTP服务器地址:smtp.gmail.com（SSL启用 端口：587）

 

【263.net】

POP3服务器地址:pop3.263.net（端口：110）
SMTP服务器地址:smtp.263.net（端口：25）

  

【Foxmail】

POP3服务器地址:POP.foxmail.com（端口：110）

SMTP服务器地址:SMTP.foxmail.com（端口：25） 

 