## 1. 说明
本节主要描述H5页面的开发方法

## 2 H5页面开发步骤
### 2.1 页面头部引入JS
```
<script type="text/javascript" src="xxx"></script>
```
提示：[JS地址的获取方法](https://cloud.tencent.com/doc/product/295/6620)
### 2.2 添加验证码展示控件
```
 <div id="TCaptcha" style="" ></div>
```
### 2.3 初始化并显示验证码
```
<script type="text/javascript">
    var capOption = {callback:cbfn, showHeader:true};
    capInit(document.getElementById("TCaptcha"), capOption);
    //回调函数：验证码页面关闭时回调
    function cbfn(retJson) {
        if (retJson.ret == 0) {
        // 用户验证成功
        }
        else {
            //用户关闭验证码页面，没有验证
        }
    }
</script>
```

## 3 完整页面代码示例
```
<!DOCTYPE html>
<html>
    <head>
        <script type="text/javascript" src="xxx"></script>
        <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no, width=device-width">
    </head>

    <body>
        <form action="xxx" id="myform" method="post">
            <input type="hidden" id="ticket" name="ticket" value="">
            <div id="TCaptcha" style="" ></div>
        </form>
        <script type="text/javascript">
            var capOption = {callback:cbfn, showHeader:true};
            capInit(document.getElementById("TCaptcha"), capOption);
            //回调函数：验证码页面关闭时回调
            function cbfn(retJson) {
                if (retJson.ret == 0) {
                    // 用户验证成功
                    document.getElementById("ticket").value = retJson.ticket;
                    document.getElementById("myform").submit();
                }
                else {
                    //用户关闭验证码页面，没有验证
                }
            }
        </script>
    </body>
</html>
```

## 4. javascript接口说明
|函数名         |  描述 |
| ------------- |:-------------|
| capInit(iframe_div, options)|初始化并显示验证码,参数如下：<br> 1. iframe_div（必填）：嵌入验证码iframe的元素。<br> 
2. options： {callback:xxx,showheader:xxx, themeColor:xxxxxx,type:"embed"}，json格式对象<br>
<blockquote>
<b>callback</b>： 验证码页面关闭回调函数。用户验证之后，会调用该函数，传入json格式验证参数。
<blockquote>{ret:xxx,ticket:"xxx"}<br> ret=0 表示用户验证完成，业务可以校验ticket；<br>
ret=1 表示用户未验证验证码，此时没有ticket参数。<br>
参数ticket需要提交给业务后台，具体填哪个字段参考后面后台server开发部分。<br>
</blockquote>
<b>themeColor</b>：设置页面的主题色彩，值为16进制色彩，比如ff572d。设置后页面里的按钮和图标会变成设置的颜色<br>
<b>showHeader</b>：显示验证码页面的header(返回和帮助，只对手机页面有效)
<blockquote>false：不显示<br>
</blockquote>
<b>type</b>：PC端可选选项，配置验证码的样式。具体样式表现可以查看[验证码官网](http://open.captcha.qq.com/cap_web/experience-character.html)<br>
<blockquote>"point"：触发式（默认）<br>"embed"：嵌入式<br>"popup"：弹窗式
</blockquote>
<b>pos</b>：设置弹框验证码的位置属性，该参数只对PC弹框验证码有效
<blockquote>
absolute: 绝对定位<br> 
fixed：相对于浏览器窗口的绝对定位<br> 
static：静态定位<br> 
relative：相对定位<br>
</blockquote>
<b>keepOpen</b>：设置验证通过页面属性
<blockquote>false：验证通过刷新（默认）<br>
true：保持显示，不刷新<br>
</blockquote>
<b>lang</b>：设置验证码语言类型
<blockquote>简体中文：2052（默认）<br>
繁体中文：1028<br>
英文：1033<br>
</blockquote>
</blockquote>|
| capGetTicket()|获取验证码验证结果<br>返回Josn格式数据{ret: 0, randstr: "xxx"}其中ticket是验证码验证成功票据，如果票据为空表示验证码验证没通过|
| capRefresh()  | 刷新验证码<br>要求用户重新验证当票据验证失败时也可调用该接口刷新验证码 |
| capDestroy()  | 无参数，当dom被销毁需要重新使用capInit的时候，在capInit之前调用 |

## 5. 接入规范
手机验证码页面要全屏显示，否则验证码页面会显示异常影响用户使用。  


