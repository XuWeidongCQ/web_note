

# 一、使用

```python
#1.chrome浏览器首先下载Chromedriver.exe 国内镜像地址:http://npm.taobao.org/mirrors/chromedriver/
from selenium import webdriver
'''
注意在webdriver包的__init__.py中有以下语句
from .chrome.webdriver import WebDriver as Chrome 
from .chrome.options import Options as ChromeOptions

所以不需要再单独导入webdriver.py和options.py
'''



#2.打开浏览器(此时浏览器是空白的)
#excutorPath为Chromedriver.exe的路径(如果把Chromedriver.exe放到Scripts文件夹中就需要写路径)
#返回一个WebDriver对象
browser = webdriver.Chrome(excutor_path)  
```

![image-20200811212730564](../../../AppData/Roaming/Typora/typora-user-images/image-20200811212730564.png)

# 二、WebDriver对象的属性和方法 [获取元素]

## 2.1 所有方法和属性

```python
[
    'add_cookie', 
    'application_cache', 
    'back', 
    'capabilities', 
    'close', 
    'command_executor', 
    'create_options', 
    'create_web_element', 
    'current_url', 
    'current_window_handle', 
    'delete_all_cookies', 
    'delete_cookie', 
    'desired_capabilities', 
    'error_handler', 
    'execute', 
    'execute_async_script', 
    'execute_cdp_cmd', 
    'execute_script', 
    'file_detector', 
    'file_detector_context', 
    'find_element', 
    'find_element_by_class_name', 
    'find_element_by_css_selector', 
    'find_element_by_id', 
    'find_element_by_link_text', 
    'find_element_by_name', 
    'find_element_by_partial_link_text', 
    'find_element_by_tag_name', 
    'find_element_by_xpath', 
    'find_elements', 
    'find_elements_by_class_name', 
    'find_elements_by_css_selector', 
    'find_elements_by_id', 
    'find_elements_by_link_text', 
    'find_elements_by_name', 
    'find_elements_by_partial_link_text', 
    'find_elements_by_tag_name', 
    'find_elements_by_xpath', 
    'forward', 
    'fullscreen_window', 
    'get', 
    'get_cookie', 
    'get_cookies', 
    'get_log', 
    'get_network_conditions', 
    'get_screenshot_as_base64', 
    'get_screenshot_as_file', 
    'get_screenshot_as_png', 
    'get_window_position', 
    'get_window_rect', 
    'get_window_size', 
    'implicitly_wait', 
    'launch_app', 
    'log_types', 
    'maximize_window', 
    'minimize_window', 
    'mobile', 
    'name', 
    'orientation', 
    'page_source', 
    'quit', 
    'refresh', 
    'save_screenshot', 
    'service', 
    'session_id', 
    'set_network_conditions', 
    'set_page_load_timeout', 
    'set_script_timeout', 
    'set_window_position', 
    'set_window_rect', 
    'set_window_size', 
    'start_client', 
    'start_session', 
    'stop_client', 
    'switch_to', 
    'switch_to_active_element', 
    'switch_to_alert', 
    'switch_to_default_content', 
    'switch_to_frame', 
    'switch_to_window', 
    'title', 
    'w3c', 
    'window_handles'
]
```

* 这部分可以直接去webdriver.py的源码中看

## 2.2 常用方法和属性

| #    | 属性或者方法                         | 说明                                             | 返回值     |
| ---- | ------------------------------------ | ------------------------------------------------ | ---------- |
|      | add_cookie(cookie_dict)              | 向当前的browser添加一条cookie                    | None       |
|      | back()                               | 点击浏览器的后退按钮                             | None       |
|      | close()                              | 关闭当前窗口                                     | None       |
|      | current_url                          | 返回当前页面的url                                | 字符串     |
|      | execute(driver_command, params=None) |                                                  |            |
|      | execute_script(js_script)            | 让浏览器执行一段js脚本(可以用来开启一个新的窗口) |            |
|      | forword()                            | 点击浏览器的前进按钮                             | None       |
|      | fullscreen_window()                  | 浏览器全屏显示                                   | None       |
| ++   | get(url)                             | 浏览器访问具体的网站                             | None       |
| ++   | implicitly_wait(time_to_wait)        | 让浏览器隐式等待一段时间                         | None       |
| ++   | page_source                          | 当前页面的html(拿到这个就可以使用其他解析库了)   | 字符串     |
|      | quit()                               | 退出driver，关闭所有的窗口                       | None       |
|      | switch_to_window(window_name)        | 切换到指定的窗口                                 | None       |
|      | window_handles                       | 返回浏览器的所有窗口列表                         | 可迭代对象 |

## 2.3 查找元素的方法[如果没有找到就报错]

| #    | 方法名                               | 说明                                          | 返回值(找不到就报错) |
| ---- | ------------------------------------ | --------------------------------------------- | -------------------- |
| ++   | find_element(by,value)               | 通过By对象来指定使用哪种选择器                | WebElement对象       |
|      | find_element_by_id(id)               | 通过html元素的id属性值查找                    | WebElement对象       |
|      | find_element_by_name(name)           | 通过html元素的name属性值查找                  | WebElement对象       |
|      | find_element_by_xpath(xpath)         | 通过xpath字符串查找                           | WebElement对象       |
| ++   | find_element_by_link_text(link_text) | 查找a标签,并且其中的文字为link_text           | WebElement对象       |
|      | find_element_by_partial_link_text    | 查找a标签,并且其中的文字为link_text(模糊匹配) | WebElement对象       |
|      | find_element_by_tag_name             | 通过html元素的标签名查找                      | WebElement对象       |
|      | find_element_by_class_name           | 通过html元素的name属性值查找                  | WebElement对象       |
| ++   | find_element_by_css_selector         | 通过标准的css选择器查找                       | WebElement对象       |

```
注意:
查找多个元素,在element后面加上s即可,返回的是WebElement对象组成的列表
```

# 三、WebElement对象的属性和方法 [解析元素]

## 3.1 所有属性和方法

```python
[
    'clear', 
    'click', 
    'find_element', 
    'find_element_by_class_name', 
    'find_element_by_css_selector', 
    'find_element_by_id', 
    'find_element_by_link_text', 
    'find_element_by_name', 
    'find_element_by_partial_link_text', 
    'find_element_by_tag_name', 
    'find_element_by_xpath', 
    'find_elements', 
    'find_elements_by_class_name', 
    'find_elements_by_css_selector', 
    'find_elements_by_id', 
    'find_elements_by_link_text', 
    'find_elements_by_name', 
    'find_elements_by_partial_link_text', 
    'find_elements_by_tag_name', 
    'find_elements_by_xpath', 
    'get_attribute', 
    'get_property', 
    'id', 
    'is_displayed', 
    'is_enabled', 
    'is_selected', 
    'location', 
    'location_once_scrolled_into_view', 
    'parent', 
    'rect', 
    'screenshot', 
    'screenshot_as_base64', 
    'screenshot_as_png', 
    'send_keys', 
    'size', 
    'submit', 
    'tag_name', 
    'text', 
    'value_of_css_property'
]
```

## 3.2 常用方法和属性

* 获取元素的方法和webdriver对象一样

| #    | 方法或者属性        | 说明                                                         | 返回值         |
| ---- | ------------------- | ------------------------------------------------------------ | -------------- |
|      | clear()             | 清除选中的html元素中的文本，常用于表单                       | None           |
| ++   | click()             | 点击选中的html元素                                           | None           |
| ++   | get_attribute(name) | 返回选中的html元素的属性值，功能比get_property强大           | 字符串         |
|      | get_property(name)  | 返回选中的html元素某属性的值(属性不存在返回None)             | 字符串         |
|      | id                  | 返回元素的被selenium使用的id值(用于内部比较两个WebElement是否相等) | 字符串         |
|      | is_displayed()      | 检查选中的html元素是否隐藏,常用于表单                        | 布尔           |
|      | is_enabled()        | 检查选中的html元素是否禁用,常用于表单                        | 布尔           |
|      | is_selected()       | 检查选中的html元素是否禁用,常用于checkbox radio              | 布尔           |
|      | location            | 返回元素在页面上的位置坐标                                   | 字典           |
|      | parent              | 返回元素的父元素                                             | WebElement对象 |
| ++   | send_keys(value)    | 向表单元素中输入值                                           | None           |
|      | submit()            | 提交表单                                                     | None           |
|      | tag_name            | 返回元素的标签名                                             | 字符串         |
| ++   | text                | 返回元素中的文本                                             | 字符串         |

## 3.3 获取元素中的文本内容

```
1. elm.get_attribute('textContent') 获取elm元素中的文本内容，子元素的内容也会被包含

2. elm.text 只能获取该元素中文本,不包含子元素的文本
```

# 四、等待

```
说明：
在selenium中，get()方法会在网页框架加载结束后执行(window.load事件触发过后就开始执行其下的代码,如果网页上有些呢内容是ajax渲染的将没有办法获取)

故需要使用显式等待(指定一个等待的最大时间,如果在该时间内找到元素就不会继续等待)
```



```python
#1. 导入等待控制对象和等待条件
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

#2. 建立一个等待控制对象的实例
wait = WebDriverWait(browser,timeout,poll_frequency=POLL_FREQUENCY, ignored_exceptions=None)
#timeout 指定一个最大的等待时间
'''
- driver - Instance of WebDriver (Ie, Firefox, Chrome or Remote)
- timeout - Number of seconds before timing out
- poll_frequency - sleep interval between calls By default, it is 0.5 second.
- ignored_exceptions - iterable structure of exception classes ignored during calls. By default, it contains NoSuchElementException only.
'''

#3. 使用
#这条语句会一直等待.btn_search的按钮能够点击为止，然后才能继续执行
btn = WebDriverWait(browser,10).until(EC.element_to_be_clickable((By.CSS_SELECTOR,'.btn_search')))
print(btn)
```

## 4.1 WebDriverWait对象的方法 [等待控制]

| #    | 方法名                   | 说明                                        | 返回值             |
| ---- | ------------------------ | ------------------------------------------- | ------------------ |
| ++   | until(fn,message='')     | 一直执行函数fn,直到得到结果或者超过最大时间 | 内部函数fn的返回值 |
|      | until_not(fn,message='') |                                             |                    |

## 4.2 EC模块的方法 [等待条件]

* 等待方法都是传入到上述until的fn方法中的

| #    | 方法                                     | 说明                                  | 返回值         |
| ---- | ---------------------------------------- | ------------------------------------- | -------------- |
|      | title_is(str)                            | 判断网页的标题是否显示了str           |                |
|      | title_contains(str)                      | 判断网页的标题是否包含了str           |                |
| ++   | presence_of_element_located((By.ID,'q')) | id为q的元素节点是否加载出来(传入元组) | WebElement对象 |
| ++   | element_to_be_clickable((By.ID,'q'))     | 节点可点击(传入元组)                  | WebElement对象 |
|      |                                          |                                       |                |



# 二、等待页面加载完成

现在的大多数的Web应用程序是使用Ajax技术。当一个页面被加载到浏览器时， 该页面内的元素可以在不同的时间点被加载。这使得定位元素变得困难， 如果元素不再页面之中，会抛出 ElementNotVisibleException 异常。 

使用 waits, 我们可以解决这个问题。

waits提供了一些操作之间的时间间隔- 主要是定位元素或针对该元素的任何其他操作。

```python

#显式等待
driver = webdriver.Firefox()
driver.get("http://somedomain/url_that_delays_loading")
try:
    # 让浏览器最大等待10s,直到选择的元素被选择
    element = WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.ID, "myDynamicElement")))
finally:
    driver.quit()
```



