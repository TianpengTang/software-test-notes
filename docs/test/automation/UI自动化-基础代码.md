# UI自动化-基础代码

### 元素定位方法

```
说明：Selenium框架中一共提供了八大类元素定位方法，只要能够定位目标元素，使用哪一种方法没有任何限制！
包含：
1、id方法
2、name方法
3、class_name方法
4、tag_name方法
5、link_text方法
6、partial_link_text方法
7、Xpath方法
8、CSS方法
```

具体实现见文档《UI自动化-元素定位方法》

### 元素等待

#### 隐式等待

```yaml
说明：定位元素时，如果能定位到元素则直接返回该元素，不触发等待；如果不能定位到该元素，则间隔一段时间[不可控制]后再去定位元素；如果在达到最大时长时还没有找到指定元素，则抛出元素不存在的异常NoSuchElementException。
```

#### 案例实现

```yaml
"""
元素等待: 隐式等待
"""
# 1. 导⼊模块
from time import sleep

from selenium import webdriver

# 2. 实例化浏览器对象
driver = webdriver.Chrome()
# 3. 打开页面
driver.get('file:///Users/comesoon/Desktop/page/%E6%B3%A8%E5%86%8CA%E7%AD%89%E5%BE%85.html')
# 设置隐式等待
driver.implicitly_wait(10)
# 定位元素并输⼊
driver.find_element_by_id('userA').send_keys('admin')
# 注意:
# 1. 隐式等待是全局有效, 只需要设置一次即可
# 2. 当隐式等待被激活时, 虽然⽬标元素已经出现了,但是还是会由于当前⻚面内的其他元素的未加载完成, ⽽继续等待, 进而增加代码的执行时长

# 4. 展示效果
sleep(3)
# 5. 退出浏览器
driver.quit()
```

#### 显示等待

```
说明：定位指定元素时，如果能定位到元素则直接返回该元素，不触发等待；如果不能定位到该元素，则间隔一段时间[可以控制]后再去定位元素；如果在达到最大时长时还没有找到指定元素，则抛出超时异常TimeoutException
```

#### 案例实现

```yaml
"""
元素等待: 显式等待
"""
# 1. 导⼊模块
from time import sleep
from selenium import webdriver
from selenium.webdriver.support.wait import WebDriverWait
# 2. 实例化浏览器对象
driver = webdriver.Chrome()
# 3. 打开⻚⾯
driver.get('file:///Users/comesoon/Desktop/page/%E6%B3%A8%E5%86%8CA%E7%AD%89%E5%BE%85.html')

# 需求：打开注册A⻚面，完成以下操作
# 1).使⽤显式等待定位用户名输⼊框，如果元素存在，就输入admin
# driver.find_element_by_id('userA').send_keys('admin')
# 设置显式等待: 按住 Ctrl, ⿏标左键点击类名, 拷⻉示例代码, 根据实际需求修改对应参数即可
"""
driver: 浏览器对象
timeout: 超时长
poll_frequency: 定位⽅法执行间隔时⻓(默认 0.5 秒)
"""
element = WebDriverWait(driver, 10, 1).until(lambda x:x.find_element_by_id('userA'))
# 说明: 元素操作方法没有代码提示, 需要⼿写
element.send_keys('admin')
# 4. 展示效果
sleep(3)
# 5. 退出浏览器
driver.quit()
```

#### 隐式等待和显示等待的对比

| 对比                  | 元素个数 | 调用方法         | 异常类型               |
| --------------------- | -------- | ---------------- | ---------------------- |
| 隐式等待              | 全局     | 浏览器对象调用   | NoSuchElementException |
| 显示等待              | 单个     | 实例化对象调方法 | TimeoutException       |
| 扩展：强制等待(sleep) | 全局     | 方法             | NoSuchElementException |

### 验证码

```yaml
说明：一种随机生成的信息（数字、字母、汉字、图片、算术题）等为了防止恶意的请求行为，增加应用的安全性。
注意：在自动化测试过程中，必须处理验证码，否则无法继续执行后续测试。
```

#### 常见验证码处理策略

```yaml
1、去掉验证码[常用]：测试环境下-采用
2、设置万能验证码[常用]：生产环境和测试环境下-采用，必须注意保密不要外泄万能验证码
3、验证码识别技术：通过Python-tesseract来识别图片类型验证码；识别率很难达到100%
4、记录cookie：通过记录cookie进行跳过登录，不能对登录本身功能进行测试时使用
```

### cookie绕过原理

```
说明：客户端登录账号后，服务器将登录状态相关的cookie信息发送给浏览器保存。后续浏览器向同一服务器再次发送请求时，会自动携带上这些 Cookie 数据。携带的cookie信息如果和服务器保留的一致，则服务器信任客户端为登录状态。
应用场景：
1、实现会话跟踪，记录用户登录状态。
2、实现记住密码和自动登录的功能。
3、用户未登录的状态下，记录购物车中的商品。
```

