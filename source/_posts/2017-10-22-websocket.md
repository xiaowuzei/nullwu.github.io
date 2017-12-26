---
title: websocket
date: 2017-10-22 09:39:08
tags: [websocket]
---
## first
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <input type="text" id="sendTxt">
    <button id="sendBtn">发送</button>
    <div id="recv"></div>
    <script>
        var websocket = new WebSocket("ws://echo.websocket.org/")
        websocket.onopen = function () {
            console.log('websocket已经连接')
            document.getElementById('recv').innerHTML = 'websocket已经连接'
        }
        websocket.onclose = function () {
            console.log('websocket关闭')
        }
        websocket.onmessage = function (e) {
            console.log(e);
            document.getElementById('recv').innerHTML = e.data;
        }
        document.getElementById('sendBtn').onclick = function () {
            var txt = document.getElementById('sendTxt').value;
            websocket.send(txt)
        }
    </script>
</body>

</html>
```
一打开页面就显示'websocket已经连接'

当输入文本框发送的时候，服务端返回的数据中，其中的data是我们发送的数据

## sec
`https://github.com/sitegui/nodejs-websocket`