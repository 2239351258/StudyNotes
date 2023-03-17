<center><h1>Cookie、Session、Token</h1></center>

### Cookie

HTTP Cookie（也叫 Web Cookie 或浏览器 Cookie）是**服务器发送到用户浏览器并保存在本地的一小块数据(key-value)**。浏览器会存储  cookie  并在下次向同一服务器再发起请求时携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器——如保持用户的登录状态。Cookie  使基于无状态的 HTTP 协议记录稳定的状态信息成为了可能。

##### Cookie分类

- *会话期* Cookie（Expires属性缺省时）会在当前的会话结束之后删除。浏览器定义了“当前会话”结束的时间，一些浏览器重启时会使用*会话恢复*。这可能导致会话 cookie 无限延长。
- *持久性* Cookie 在过期时间（`Expires`）指定的日期或有效期（`Max-Age`）指定的一段时间后被删除。
  - 当 Cookie 的过期时间（ `Expires`）被设定时，设定的日期和时间只与客户端相关，而不是服务端。

##### Cookie的缺点

- cookie是客户端技术，对客户端是可见的，数据安全性较低。
- cookie中只能保管ASCII字符串，假如需求存取Unicode字符或者二进制数据，需求先进行编码
- 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

### Session

**服务器为每个用户浏览器创建一个会话对象**（session对象），注意：一个浏览器独占一个session对象(默认情况下)。因此，在需要保存用户数据时，服务器程序可以把用户数据写到用户浏览器独占的session中，当用户使用浏览器访问其它程序时，其它程序可以从用户的session中取出该用户的数据，为用户服务。

##### Session的实现

服务器创建session出来后，会把session的id号，以cookie的形式回写给客户机，这样，只要客户机的浏览器不关，再去访问服务器时，都会带着session的id号去，服务器发现客户机浏览器带session id过来了，就会使用内存中与之对应的session为之服务。

##### Session的缺点

- Session是保管在服务器端的，每个用户都会产生一个Session。假如并发访问的用户多，会产生大量的Session，很耗费存储空间

### Cookie与Session的区别

- 数据存放位置不同：Session数据是存在服务器中的，cookie数据存放在浏览器当中。
- cookie中只能保管ASCII字符串，假如需求存取Unicode字符或者二进制数据，需求先进行编码，Session中能够存取任何类型的数据
- Session是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件或内存中
- Cookie是客户端保存用户信息的一种机制，用来记录用户的一些信息，也是实现Session的一种方式。
- session 的运行依赖 session id，而 session id 是存在 cookie 中的，也就是说，如果浏览器禁用了 cookie ，同时 session 也会失效（但是可以通过其它方式实现，比如在 url 中传递 session_id）

- 本来 session 是一个抽象概念，开发者为了实现中断和继续等操作，将 user agent 和 server 之间一对一的交互，抽象为“会话”，进而衍生出“会话状态”，也就是 session 的概念。
- cookie 是一个实际存在的东西，http 协议中定义在 header 中的字段。可以认为是 session 的一种后端无状态实现。

### Token

token的意思是“令牌”，是服务端生成的一串字符串，作为客户端进行请求的一个标识。

简单的token组成:uid(用户唯一的身份标识)、time(当前时间的时间戳)、sign(签名，由token的前几位+盐以哈希算法压缩成一定长的十六进制字符串，可以防止恶意第三方拼接token请求服务器)。

##### Token优点

- 支持跨域访问: Cookie是不允许垮域访问的，token支持
- 无状态： token无状态，session有状态的
- 去耦: 不需要绑定到一个特定的身份验证方案。Token可以在任何地方生成，只要在 你的API被调用的时候， 你可以进行Token生成调用即可

##### Token缺点

- 占带宽，正常情况下要比 session_id 更大，需要消耗更多流量，挤占更多带宽
- 无法在服务端注销，很难解决劫持问题

