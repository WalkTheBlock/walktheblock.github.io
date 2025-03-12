# 一些视频的源码

## Selenium自动做问卷星

> 已更新  
> 视频  
> > 1. [Python自动化填写问卷星](https://www.bilibili.com/video/BV1b94y1n7J2/)  
> > 2. [自动化填问卷的第一桶金，半小时一百块](https://www.bilibili.com/video/BV1BSDTYnEoa/)  
> >3. [填问卷不再手动！自动生成自动化代码，一键搞定所有问卷类型！](https://www.bilibili.com/video/BV1AdDLYJEFv/)   
>  
> 文章  
> >1. [Python自动化填写问卷星](http://www.stardream.vip/blog/2)  
> >2. [自动化填问卷的第一桶金，半小时一百块](http://www.stardream.vip/blog/22)  
> >3. [填问卷不再手动！自动生成自动化代码，一键搞定所有问卷类型！](http://www.stardream.vip/blog/23)  

代码写的挺low的，哎，没时间优化了

:::tip
由于之前自动化做问卷星的视频数据不错，很多人都要了代码。

但是我觉得那个代码写的特别烂，所以甚至我现在稍稍有些惭愧的感觉。

距离上一版代码到现在已经有许久许久时间了。

在这段时间中，爬虫技术在进步，我也同样在进步，虽然说我的技术还不是很厉害，但是这个自动做问卷星的优化思路其实我很早以前就有的，然后刚好再配合上最近发现的一个新技术实现一下。看看这能不能做出一个完美的脚本或者是软件。

使用的技术依旧是一个自动化工具叫做DrissionPage，代码思路就是首先自己创建一份问卷，所有的题型都加上，然后观察网页标签类名的分布规律，然后自动化去识别填写提交问卷。
:::

```python
#导入相关库
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.options import Options
from time import sleep
from selenium.webdriver.common.by import By
from random import choice
#问卷网址
url='https://www.wjx.cn/vm/hKvbqcL.aspx'
#绕过问卷星的智能检测，将webdriver属性设置为undefined，不设置也不会错
option=Options()
option.add_experimental_option('excludeSwitches',['enable-automation'])
option.add_experimental_option('useAutomationExtension',False)
web=Chrome(options=option)
web.execute_cdp_cmd('Page.addScriptToEvaluateOnNewDocument',{'source':'Object.defineProperty(navigator,"webdriver",{get:()=>undefined})'})
#对页面进行请求
web.get(url)
#设置每个题目的选项列表
#分别对每个题进行随机的，或者有倾向填充
for i in range(1,4):
	qa_tmp=web.find_element(By.XPATH,f"//*[@id='div{i}']")
	answers=qa_tmp.find_elements(By.XPATH,f".//div[@class='ui-radio']")
	# 生成随机选项
	# [1,2,3,3,3,33,3,33,3,33,]
	answer=choice(answers)
	answer.click()
qa_tmp=web.find_element(By.XPATH,"//*[@id='div4']")
input_=qa_tmp.find_element(By.XPATH,".//input")
input_.send_keys('Python 好啊！')
sleep(0.1)
#点击提交按钮
web.find_element(By.XPATH,'//*[@id="ctlNext"]').click()
sleep(0.1)
web.quit()

```