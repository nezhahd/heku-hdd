有很多反馈说heroku被秒封！建议先用其他的heroku项目或先避开当前的敏感期

### 请勿fork，请勿fork，请勿fork，点个Star就好了

直接点击绿色的`Use this template`创建自己名称的项目，workers反代与pages反代及自定义域，配置文件信息等相关操作拓展教程，请关注：[博客视频教程](https://ygkkk.blogspot.com/2022/05/heroku-cloudflare-workers-pages.html)

一、操作步骤：

1、在浏览器复制链接   https://dashboard.heroku.com/new?template= 加上你自定义的项目链接



2、之前没有登录记录的话，会先提示注册并或登录Heroku界面，大家自己注册或者登录下



3、Heroku app名称与国家随意，最后设置图如下



4、输入UUID，建议使用V2rayN等工具生成，点击Deploy app，几秒种后就完成安装。

-------------------------------------------------------------------------------------------

二、关于为什么套CF以及满足自选IP/域名的条件解答



三、客户端配置如下（V2rayN）

协议：(vless/vmess/trojan)-ws

地址：app.heroku.com（自选IP/域名）

端口：443

用户ID/密码：自定义的UUID

传输协议：ws

伪装host：app.heroku.com（workers或pages反代/自定义域）

路径path：留空或填/

传输安全：tls

SNI：app.heroku.com（workers或pages反代/自定义域）

其他设置保持默认不变！！！

### CloudFlare Workers反代代码（可分别用两个账号的应用程序名（`协议`、`UUID`保持一致），单双号天分别执行，那一个月就有550+550小时（每个账号一个月免费使用550小时））
<details>
<summary>CloudFlare Workers单账户反代代码</summary>

```js
addEventListener(
    "fetch",event => {
        let url=new URL(event.request.url);
        url.hostname="appname.herokuapp.com";
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>CloudFlare Workers单双日轮换反代代码</summary>

```js
const SingleDay = 'app0.herokuapp.com'
const DoubleDay = 'app1.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>CloudFlare Workers每五天轮换一遍式反代代码</summary>

```js
const Day0 = 'app0.herokuapp.com'
const Day1 = 'app1.herokuapp.com'
const Day2 = 'app2.herokuapp.com'
const Day3 = 'app3.herokuapp.com'
const Day4 = 'app4.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        let day = nd.getDate() % 5;
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4){
            host = Day4
        } else {
            host = Day1
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>CloudFlare Workers一周轮换反代代码</summary>

```js
const Day0 = 'app0.herokuapp.com'
const Day1 = 'app1.herokuapp.com'
const Day2 = 'app2.herokuapp.com'
const Day3 = 'app3.herokuapp.com'
const Day4 = 'app4.herokuapp.com'
const Day5 = 'app5.herokuapp.com'
const Day6 = 'app6.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        let day = nd.getDay();
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4) {
            host = Day4
        } else if (day === 5) {
            host = Day5
        } else if (day === 6) {
            host = Day6
        } else {
            host = Day1
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>


参考项目
[heshan2/heroku-xray-server](https://github.com/heshan2/heroku-xray-server)
