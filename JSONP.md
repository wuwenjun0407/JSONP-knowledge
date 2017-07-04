# JSONP

@(AJAX和DOS命令)

### 同源（同域）/非同源（跨域）
#### 同源
>当前页面的URL地址：http://localhost:1234/index.html
>获取数据的URL：http://localhost:1234/getUserList-->可以使用AJAX获取数据

#### 非同源
>当前页面的URL地址：http://localhost:1234/index.html
>获取数据的URL：http://matchweb.sports.qq.com/kbs/hotMatchList-->不能使用AJAX请求

#### 怎么区分同源和非同源？
>从三个维度（看页面URL地址和获取数据的URL地址进行比较）
-   协议
-  域名
-  端口号
>以上三个都相同就是同源，有一个不一样就是跨域，跨域是不可以使用AJAX的

#### 在真实项目中什么时候会用到跨域请求？
>1）我们自己的网站上需要展示别人网站上的数据，这样的话，我们需要在自己的服务器上获取其他网站服务器的数据，例如：我们需要展示百度，腾讯，京东....第三方平台的数据，这就是跨域
>2）如果我们的项目比较大，访问的人也比较多，我们一般都会用很多服务器取管理，例如：我们的资源文件放在A服务器，数据放在B服务器上，A服务器想从B服务器获取数据，也叫跨域（只要不是同一个服务器请求都叫跨域）
>3二级域名（sports.qq.com）向一级域名（www.qq.com）获取数据也是跨域，因为域名不同

#### 跨域请求的方式？
>
-   JSONP
-  ifram
-  postMessage
-  document.domain
-  crossDomain...
>一般我们最常用的是JSONP

#### 什么是JSONP/JSONP原理？

>script标签是没有同和非同源之分的,src引入的JS文件是自己服务器上的或者别人服务器上的都可以（换句话说，我们的script标签，可以向其他服务器发送请求，并且其他服务器也可以接受到请求，把需要的内容返回给客户端）其他的标签：link Img audio video iframe
>JSONP原理：利用script不存在跨域限制，我们把需要的请求数据地址赋给src属性，这样就可以从别人服务器上获取相应的数据，这样还不够，我们还需要在JS中把浏览器获取出来的数据得到，来做后续的操作
**注意：JSONP请求方式是GET**
```
<script>
    function ff(result){
        console.log(result);
    }
    ff();
</script>
<script type="text/javascript" src="http://matchweb.sports.qq.com/kbs/hotMatchList?callback=ff"></script>

腾讯服务器就会接受请求，去查找参数callBack的值，比如：我们这里传的ff，然后进行解析赋值“ff({'name':'aa','age':'12'})”,然后把这个字符串返回给客户端,客户端让ff执行就可以了
```
>JQ中JSONP请求，JQ会默认给URL后面追加一个变量来清缓存
>JQ中的方法：
```
<script type="text/javascript" src="JS/jquery-3.2.1.min.js"></script>
<script>
    $.ajax({
        url:"http://matchweb.sports.qq.com/kbs/hotMatchList?callback=fn",
        type:"get",
        dataType:"jsonp",
        jsonp:"callback",
        jsonpCallback:"fn",//设置传给服务器的函数名，不是JQ随机默认生成的那个，而是我们自己起的名字fn了
        success:function(result){
            console.log(result);
        }
    })
在JQ处理JSONP请求的时候，会默认帮我们在请求地址后面创建一个callback=jquery数字，这是JQ随机生成的一个函数，可以认为是success赋值的那个匿名函数    
```