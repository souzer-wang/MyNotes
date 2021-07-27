# Session

## 为什么需要 Session ？

因为 http 是无状态的协议，为了让服务器记住用户的状态，就引入了session


---
## session 如何存储

### PHP 中 session 的存储

存储到服务器端的 **文件** 或 **数据库** 中

- PHP 以读写文件的方式保存 session 数据，存到指定目录（文件多的话可采用多层目录）
- 写入时，PHP获取客户端的session_ID，根据这个ID到指定目录中找到对应session文件（如果没有的话就创建），将session数据序列化后写入文件
- 读取时，操作类似，	对读出来的文件内容进行反序列化，生成session数据

### Java 中的 session 存储

#### session 如何创建

当客户端第一次请求session对象时候(不等同于第一次访问)，服务器会为客户端创建一个session
> 在访问 tomcat 服务器时，HttpServletRequest 的 getSession(true) 创建 session

#### sessionid 哪儿来的

tomcat 的 ManagerBase类提供创建 sessionid 的方法，具体是：

> 随机数 + 时间 + jvmid

#### 存到哪里？

 tomcat 中的 StandardManager 将 session **存储在内存中**
>  可以持久化到：
> 1. 文件
> 2. 数据库
> 3. memcache
> 4. redis 等

客户端只保存sessionid到cookie中，而不会保存session

#### 如何删除？

- invalidate： 程序调用HttpSession.invalidate()
- 超时

**关掉浏览器并不会关闭session**

---
## Java 的分布式 Session 如何实现

**问题点：** 如果不做任何处理，用户将出现频繁登陆的情况。

> 比如集群中存在两台服务器 A、B，用户第一次访问时，访问了A，那么A就给用户创建了session；如果用户第二次访问，访问了B，那么因为B中没有用户的session，就会将用户踢回登陆页面。

---
1. **粘性 session** (Session Stick)

**原理：** **将用户绑定到某个服务器上**（这样就不会出现，用户访问完A服务器后，又访问B服务器）

**优点：** 简单，不需要对session进行处理

**缺点：** 缺乏容错性，如果绑定的服务器出现故障，用户会转移到另一个服务器，之前的session信息就会失效。

**适用场景：**
- 发生故障对用户影响小
- 服务器发生故障的概率低

---
2. **服务器 session 复制** (Session Replication)

**原理：** 任何一个服务器上的session发生改变（增删改），该节点会把这个session的所有内容序列化，**广播给所有其它节点**（不管它们是否需要），来保证session同步。

**优点：** 容错性好，各个服务器之间的session可实时响应

**缺点：** 会对网络负荷造成压力，如果session量大，会造成网络拥塞，拖慢服务器性能。

---
3. **session 数据集中存储** 

**原理：** 把用户的session存储在单台或者集群服务器的缓存中，所有web服务器从中拿取session，实现session共享

**缺点：** 如果集中存储session的机器或集群出现问题，会影响应用。

---
4. **cookie based**

将session存储在cookie里，在访问web时，再由web服务器生成对应的session数据

**缺点：**
- cookie长度限制，导致session长度也受限
- 安全性：session本来是服务器数据，存在客户端即便可以加密，也不够安全。
- 性能影响：每次http请求和响应都带上了session数据，影响性能。


cookie和session

由于HTTP协议是无状态的协议，所以服务端需要记录用户的状态时，就需要用某种机制来识具体的用户，这个机制就是Session.

典型的场景比如购物车，当你点击下单按钮时，由于HTTP协议无状态，所以并不知道是哪个用户操作的，所以服务端要为特定的用户创建了特定的Session，用用于标识这个用户，并且跟踪用户，这样才知道购物车里面有几本书。
这个Session是保存在服务端的，有一个唯一标识。

服务端如何识别特定的客户？这个时候Cookie就登场了

每次HTTP请求的时候，客户端都会发送相应的Cookie信息到服务端。实际上大多数的应用都是用 Cookie 来实现Session跟踪的，第一次创建Session的时候，服务端会在HTTP协议中告诉客户端，需要在 Cookie 里面记录一个Session ID，以后每次请求把这个会话ID发送到服务器，我就知道你是谁了。

  
- Session是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件中；  
- Cookie是客户端保存用户信息的一种机制，用来记录用户的一些信息，也是实现Session的一种方式