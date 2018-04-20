web自动化参考
## 一.环境准备

软件：python3.x 、pycharm   
1.mac下安装配置的python路径为：/Library/Frameworks/Python.framework/Versions/3.6/bin/python3.6   
2.pip3 install selenium 安装selenium或者在pycharm里面安装selenium，具体步骤可以看[APPIUM](https://github.com/GongK/APPIUM)，里面的pycharm部分      
3.HTMLtestrunner.py用于测试报告生成，在python3.x下是需要修改的   
4.下载最新版的Firefox浏览器以及[浏览器驱动](https://github.com/mozilla/geckodriver/releases)，选择对应的版本即可，存放路径为：/Library/Frameworks/Python.framework/Versions/3.6/bin   

---
## 二.定位元素
元素定位的方式多种多样，我这里下列定位方式：   
**1.通关id或者name来定位**     
find_elment_by_id&find_elment_by_name   
find_elments_by_tag_name("input")[0].send_keys("python")多个input元素的时候用s，表明第几个    
**2. 通过标签的名称来定位**     
find_elment_by_tag_name，类似input 元素 a元素       

**3. 通关class来定位**    
find_elment_by_class 

**4.CSS选择器定位**   
find_element_by_css_selector   
**5.其余方式**   
find_element_by_xpath  绝对定位
find_element_by_link_text 根据链接的文字
find_element_by_partial_link_text   链接的部分文字定位
参考链接：
https://www.cnblogs.com/yufeihlf/p/5717291.html   
**6.父子双层定位**      
sub_element = driver.find_element_by_id('xx').find_element_by_link_text('Another_action')

xpath定位参考链接
https://www.cnblogs.com/qingchunjun/p/4208159.html

**7.逻辑定位组合**   
driver.find_elemnet_by_xpath("//input[@class='loinp' and @name='username'])
driver.find_element_by_xpath("//form[@id='login']/ui/input[1]")

**8.如果下拉框的标签是option，可以用select**   
 
 
  #选择select定位
        Select(driver.find_element_by_name("isImportant2")
               ).select_by_visible_text(u"是")   
## 等待   
selenium中等待的方式有3中，分别是强制等待、隐式等待、显式等待    

- 强制等待：强制等待需要在文件开头导入时间模块，再在需要的地方使用sleep方法
```
from time import sleep
sleep(5) //睡眠5秒

```
- 隐式等待：为全局等待方法，意义是在规定的时间内A需要等B，如果B提前来了，则A可以提前去做其他事情，如果B在规定时间内没有来，A规定的时间等够了也要去做自己的事情。

```
from selenium import webdriver
dr=webdriver.Firefox()
dr.get('http:www.baidu.com')
dr.implicitly_wait(10) //等待10秒
dr.quit()
```
- 显式等待：这个方法一般不单独使用，意义是A等B，每隔x秒看一眼B来没有，如果B没来且超过规定时间那就抛出异常

```
from selenium import webdriver
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support.ui import expected_conditions as EC
from selenium.webdriver.common.by import By//By方法也是一种常用的定位方法 

dr=webdriver.Firefox()
dr.get('http:www.baidu.com')
locator=(By.Id,'kw')

try:
   WebDriverWait(dr,30,0.5).until(EC.presence_of_element_located(locator))
   print("等待30秒，每0.5秒查看一次，直到找到id为kw的元素")
finally:
dr.quit()
