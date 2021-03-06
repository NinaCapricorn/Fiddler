# Fiddler
**1. 抓包工具**<br>
     1) Fiddler是一个http协议调试代理工具，它能够记录并检查所有你的电脑和互联网之间的http通讯，设置断点，查看Fiddler拦截到的数据; <br>
     2) 原理：本机开启了一个http代理服务器，转发所有的http请求和响应; <br>
     3) Fiddler将自己设置为一个代理服务器，默认监听 127.0.0.1:8888，同时将浏览器的http、https协议设置为使用代理服务器; <br>
****
**2. 安装** ：下载安装包，按照步骤一步一步进行就ok <br>
     Fiddler整体包括这几个部分：<br>
     1) 工具栏<br>
     2) 监听开关 capturing<br>
     3) 监听类型<br>
     4) 命令行<br>
     5) 请求列表<br>
     6) 请求相关信息<br>
****     
**3. 抓包** <br>
     1) 设置 Tools-Fiddler Options-Connections，设置端口号为8888，勾选 “Allow romote computers to connect”;<br>
     2) 在本机中输入ipconfig确定本机的ip地址;<br>
     3) 打开Andorid/IOS在设置-WLAN中找到“修改网络”选项，“代理”选择“手动”、"代理服务器主机名"中输入步骤2中的ip地址、"代理服务器端口”设置为"8888"<br>
     此时，启动手机app就可以开始抓包，以上设置完成后只能进行简单的http抓包
****

**4. Https抓包**<br>
* 简单https抓包： 完成以上的配置，只能抓取简单的http请求<br>
* 若想抓取https请求，需要进行以下设置：<br>
1) 勾选**"Capture Https connects"、"decrypt https traffic"** <br>
2) 若监听的程序访问的https站点使用的是不可信的证书，则勾选**"Ignore server certificate errors"**<br>
以上步骤完成后可以抓取简单的https请求
* 过证书验证：某些app对https证书进行验证，还需要将Fiddler代理服务器的证书导到Android设置上才能进行抓包<br>
操作步骤：<br>
1) 打开浏览器，输入代理IP+端口<br>
2) 点击页面 “FiddlerRootcertificate” 链接<br>
3) 在弹框页面中，输入一个证书名称，点击 “确定” 即可<br>
以上步骤完成后，可以抓取大部分https请求<br>
* 使其在httpURLConnection下正常抓包：点击Rules-Customize Rules在OnBeforeResponse后面添加以下的语句即可。<br>
     static function OnBeforeResponse(oSession: Session) {
        if (m_Hide304s && oSession.responseCode == 304) {
            oSession["ui-hide"] = "true";
        }
       if (oSession.oRequest["User-Agent"].indexOf("Android")> -1 && oSession.HTTPMethodIs("CONNECT")) {
           oSession.oResponse.headers["Connection"] = "Keep-Alive"; }
    } 
    <br>
以上步骤设置完成后，基本可以完成所有相关https的抓包<br>

****

**5. 断点设置**<br>
* 完成以上设置就可以抓取http及https请求，但是抓到包还不够，更多的是需要对requestURL进行修改或者修改response包<br>
* 设置断点，筛选出相应需要修改数据的http请求；<br>
* 断点设置的方式很多，以bpu 命令为例：例如，<br> 
1) 在底部的输入框中输入bpu api.impingo.me, 断点拦截所有域名为api.impingo.me的http请求 <br>
2) 若直接输入bpu，不带任何参数则会清空设置了断点的http请求 <br>

****

**6. Composer**<br>
右侧选项卡中composer，可以模拟post、get的http请求<br>
****

**7. Replay**<br>
1) 选中请求列表中http请求，单击replay按钮就可试下回放；<br>
2) 按着Shift+Replay，可以输入重放次数（Hold SHIFT to Reissue multiple times）；<br>
3) 按着CTRL+Replay，可以无条件的重放（Hold CTRL to Reissue unconditionally）;<br>
****

**8. Hosts**<br>
fiddler中也支持host修改，路径tools-hosts<br>
****

**9. Filters**<br>
监控列表中出现的http请求会存在很多无关的请求，此时需要过滤<br>
怎样设置过滤规则，需要使用正则表达式<br>
