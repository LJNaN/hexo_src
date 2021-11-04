---
title: Python+Selenium 的一些脚本制作方法
date: 2021-11-04 10:44:41
tags: [python, selenium, 脚本]
categories: [知识点]
---

## 关于Selenium
#### 什么是 Selenuium？
Selenium是一个用于测试网站的自动化测试工具，支持各种浏览器包括Chrome、Firefox、Safari等主流界面浏览器，同时也支持phantomJS无界面浏览器。

#### Selenuim 支持哪些系统？
Windows、Linux、IOS、Android等都能支持

#### 我拿它做了些啥？
- 学校每日打卡
- 抢到4价疫苗(没用到`Selenium`，主要是轮询+短信提醒)
- 还有一些脚本是用的油猴插件，如抢课(纯`js脚本`)、查全专业同学考试的成绩组成并做成表格(`python+js脚本`)等
![gif](3.gif)

<!--more-->

#### Selenuim 支持哪些浏览器？
Chrome、Edge、Firefox

#### 你需要具备的一点点能力
- 会使用`id`、`name`、`class name`、`tag name`、`xpath`等方法查找元素节点
- 会一点`python`
> 附加：
> - 短信能力：能够找到一家靠谱的平台商
> - 邮件能力：能够获取到自己邮箱的授权码
> - 定时任务：需要的话，要有能力部署在`Linux`上面

## 安装Selenium
#### 1. 安装
```python
pip install Selenium
```

#### 2. 找到自己浏览器的驱动
一定要对应自己的浏览器以及版本（版本！！！）
![jpg](Snipaste_2021-11-04_11-30-04.jpg)
[Chrome驱动文件](https://chromedriver.storage.googleapis.com/index.html)
[Edge驱动文件](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/)

#### 3. 把下下来的驱动`exe文件`复制到`Python`根目录下
不同浏览器可能要对`exe文件`进行重命名

#### 4. 写一小段代码片段
```python
# 为了方便演示，中间加了 time.sleep() 延迟
from selenium import webdriver
from selenium.webdriver.common.by import By
def main():
  driver = webdriver.Edge()
  driver.get("https://www.baidu.com/")
  driver.find_element(By.ID, "kw").send_keys("Selenium")
  driver.find_element(By.ID, "su").click()
  driver.quit()
if __name__ == '__main__':
    main()
```

#### 5. 冲
![gif](1.gif)

#### 6. 定位元素的方式
定位元素|含义
-----------|----------
find_element(By.ID, "xxx")                  |通过元素id定位
find_element(By.NAME, "xxx")                |通过元素name定位
find_element(By.XPATH, "xxx")               |通过xpath表达式定位
find_element(By.LINK_TEXT, "xxx")           |通过完整超链接定位
find_element(By.PARTIAL_LINK_TEXT, "xxx")   |通过部分链接定位
find_element(By.TAG_NAME, "xxx")            |通过标签定位
find_element(By.CLASS_NAME, "xxx")          |通过类名进行定位
find_element(By.CSS_SELECTOR, "xxx")        |通过css选择器进行定位

如果要定位多个，使用`find_elements`即可

#### 6. 常用方法的使用
方法                    |  说明
------------------------|-----------------------
set_window_size()       |  设置浏览器的大小
back()                  |  控制浏览器后退
forward()               |  控制浏览器前进
refresh()               |  刷新当前页面
clear()                 |  清除文本
send_keys (value)       |  模拟按键输入
click()                 |  单击元素
submit()                |  用于提交表单
get_attribute(name)     |  获取元素属性值
is_displayed()          |  设置该元素是否用户可见
size                    |  返回元素的尺寸
text                    |  获取元素的文本

## 邮件能力(以QQ邮件为例)
#### 需要用到的包
```python
import smtplib
from email.mime.text import MIMEText
```

#### 方法
```python
def mail(subject, content, msg_to):
  subject = "主题"
  content = "内容"
  msg_to = "10000@qq.com" #目标邮箱
  msg_from = "1086@qq.com" #发信方邮箱
  passwd = "xxxxx" #授权码

  msg = MIMEText(content)
  msg['Subject'] = subject
  msg['From'] = msg_from
  msg['To'] = msg_to
  try:
    s = smtplib.SMTP_SSL("smtp.qq.com", 465)
    s.login(msg_from, passwd)
    s.sendmail(msg_from, msg_to, msg.as_string())
  finally:
    s.quit()
```

> [获取授权码](https://service.mail.qq.com/cgi-bin/help?subtype=1&id=28&no=1001256)

![jpg](Snipaste_2021-11-04_14-42-33.jpg)

## 短信能力
每家平台不一样，我用的[互亿无线](https://user.ihuyi.com/new/login.html)，大同小异，因为我买的学生版(50r/1000条)，所以只能用发验证码的固定模板。
但你可以制定一些代号(但是只有你自己看得懂，笑死)，如 验证码为 0， 就是打卡全部成功，否则为打卡失败的个数，为 999999, 就是服务器炸了 等。

#### 方法
```python
# 平台一般会有自己的示例，建议看你买的平台的示例
# 短信配置，这些平台会给你，都差不多
host = "xxxxxxxxxxxx"     
sms_send_uri = "xxxxxxx"
account = "xxxxxxxx"
password = "xxxxxxxxxxxx"
sms_list = ["13333333333"] #接收短信的数组

def send_sms(mobile, num):
  global password
  global account
  global sms_send_uri
  global host
  params = urllib.parse.urlencode({'account': account, 'password': password, 'content': '您的验证码是：' + str(num) + '。请不要把验证码泄露给其他人。', 'mobile': mobile, 'format': 'json'})
  headers = {"Content-type": "application/x-www-form-urlencoded",
              "Accept": "text/plain"}
  conn = http.client.HTTPConnection(host, port=80, timeout=30)
  conn.request("POST", sms_send_uri, params, headers)
  response = conn.getresponse()
  res = json.loads(response.read().decode("utf-8"))
  response_str = response.read()
  conn.close()
  return response_str
```

![短信](Snipaste_2021-11-04_14-52-46.jpg)

## 参考资料
[Python Selenium库的使用](https://blog.csdn.net/weixin_36279318/article/details/79475388)