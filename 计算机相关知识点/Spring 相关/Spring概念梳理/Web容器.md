> 前言：
> 早期的 web 应用主要浏览静态内容，**由http浏览器向页面返回静态html**，浏览器负责解析html，呈现给用户
> 随着互联网发展，静态页面就不够了，还需要一些交互操作，获取动态结果。

于是 SUN 公司推出了 servlet 技术，**可以把servlet理解为运行在服务端的小程序**，但是servlet没有main方法，不能独立运行。

必须部署到Servlet容器中，由容器来实例化并调用Servlet

> **Tomcat** 和 **Jetty** 就是一个 Servlet 容器，同时他们还具备http服务器的功能。
>
> 换句话说，它们是 HTTP 服务器 + Servlet 容器，我们称之为 **Web容器**


### Web容器与Spring、Web应用等的关系

几乎所有的Java Web框架（如Spring）都是基于Servlet的封装，Spring应用本身就是一个Servlet，而 Tomcat 和 Jetty 这样的 Web 容器，负责加载和运行 Servlet。

具体的结构关系可以通过下图了解：

![关系图](https://img-blog.csdnimg.cn/20190630105150710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h6azE1NjIxMTA2OTI=,size_16,color_FFFFFF,t_70)

## 什么是Web容器

OK，回到正题，刚刚提到servlet没有main方法，那么如何启动、结束、寻找 servlet，都需要另外一个Java应用来完成，这个应用就称为 **Web容器**

假如某个时刻，Web服务器接到指向某个 servlet 的请求，此时将由部署该servlet的容器来向servlet提供 http 请求和响应，而且由该容器调用servlet的方法，如doPost 和 doGet

## Web容器的好处

-   **通信支持**  
    利用容器提供的方法，你可以简单的实现servlet与web服务器的对话。
	
	否则你就要自己建立server搜创可贴，监听端口，创建新的流等等一系列复杂的操作。而容器的存在就帮我们封装这一系列复杂的操作。使我们能够专注于servlet中的业务逻辑的实现。
    
-   **生命周期管理**  
    容器负责servlet的整个生命周期。如何加载类，实例化和初始化servlet，调用servlet方法，并使servlet实例能够被垃圾回收。有了容器，我们就不用花精力去考虑这些资源管理垃圾回收之类的事情。
    
-   **多线程支持**  
    容器会自动为接收的每个servlet请求创建一个新的java线程，servlet运行完之后，容器会自动结束这个线程。
    
-   **声明式实现安全**  
    利用容器，可以使用xml部署描述文件来配置安全性，而不必将其硬编码到servlet中。
    
-   **jsp支持**  
    容器将jsp翻译成java！
    

  ## Web容器处理的流程
  
**1. client点击一个url，其url指向一个servlet**

![第一步](https://img-blog.csdnimg.cn/20190630111525984.png)

**2. 容器识别这个请求索要一个servlet，然后创建两个对象**
	- 请求对象：httpservletrequest
	- 响应对象：httpservletresponse

![第二步](https://img-blog.csdnimg.cn/20190630112032805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h6azE1NjIxMTA2OTI=,size_16,color_FFFFFF,t_70)

**3. 容器根据请求中的url找到对应的servlet，为这个请求创建或分配一个线程，并把两个对象（request 和 response）传递到servlet线程中**

![第三步](https://img-blog.csdnimg.cn/20190630112206437.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h6azE1NjIxMTA2OTI=,size_16,color_FFFFFF,t_70)

**4. 容器调用servlet的 service() 方法，根据请求的不同类型， service() 方法会调用 doPost() 和 doGet() 方法**

![第四步](https://img-blog.csdnimg.cn/20190630112306679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h6azE1NjIxMTA2OTI=,size_16,color_FFFFFF,t_70)

**5. doGet() 方法生成动态页面，然后把这个页面填入到 response 对象中**

![第五步](https://img-blog.csdnimg.cn/20190630112443412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h6azE1NjIxMTA2OTI=,size_16,color_FFFFFF,t_70)

**6. 线程结束，容器把 response 对象转换为http响应，传回client，然后销毁 request 和 response 对象**

![第六步](https://img-blog.csdnimg.cn/20190630112525233.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h6azE1NjIxMTA2OTI=,size_16,color_FFFFFF,t_70)