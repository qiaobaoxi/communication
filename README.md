# communication
    —> 什么是同源策略及限制
          —> 前后端如何通信                  —> 如何创建Ajax                          —> 跨域通信的几种方式
    什么是同源策略及限制： 源：协议，域名，端口 限制：不是一个源的文档没有权利去操作另一个源的文档，包括Cookie, LocalStorage, IndexDB, DOM无法获取， Ajax无法发送 同 源策略限制不同的源的文档之间进行交互。

    前后端如何通信：
    Ajax：同源限制 W ebSocket：不受限制 C ORS：支持同源，也支持非同源（新的通信协议标准）

    如何创建Ajax：有以下几个要点
    XMLHttpRequest对象的工作流程 兼 容性处理 事件 的触发条件 事件 的触发顺序

    var xhr = XMLHttpRequest ? new XMLHttpRequest() : window.ActiveXObject(‘Microsoft.XMLHTTP’); 
    var data = opt.data; 
    var url = opt.url; 
    var type = opt.type.toUpperCase(); 
    var dataArr = []; 
    for(k in data){     dataArr.push(k + ‘=‘ + data[k]); } if(type === ‘GET’) {     url = url + ‘?’ + dataArr.join(‘&’);     
      xhr.open(type, url.replace(/\?$/g, ‘’), true);
      // 如果以？结尾，则将问号去掉     
      xhr.send(); } if(type === ‘POST’) {     xhr.open(type, url, true);    
      xhr.setRequestHeader(‘content-type’, ‘application/x-www-form-urlencoded’);
      xhr.send(dataArr.join('&')) } 
      xhr.onload = function() {     If(xhr.status === 200 || xhr.status === 304) { 
            // 304是使用的本地缓存          // 206也可以，媒体资源          
      var res;          
      if(opt.success && opt.success instanceof Function) {             
        res = xhr.responseText;              
        if(typeof res === ’string’){                 
          Res = JSON.parse(res);                  
          opt.success.call(xhr, res);              
        }         
      } else { 
          if(opt.error && ope.error instanceof Function) {                 
            opt.error.cal(xhr, res);              
          }          
        }      
      } 
    }
    跨域通信的几种方式：
    JSONP H ash(hash改变，页面是不刷新的，？后是search，改变时会刷新页面) p ostMessage（新技术，h5的标准） W ebSocket C ORS（可以理解为支持跨域通信的变种Ajax。当你在浏览器中发送一个ajax跨域请求时，浏 览器会在http头中加入一个origin。如果只是一个普通的ajax，则会被浏览器拦截）

    JSONP的原理： 利用script标签的可以不同源加载实现的。
    1. 在window全局注册一个函数 2. 给服务端传递这个函数的名字，同时可以有参数 3. 服务端返回这个函数，内部填充有数据，就可以拿到数据 4. 删除全局注册的那个函数
    Hash的原理： 页面A中通过iframe或frame嵌入了B窗口。目标是A给B发消息。
    1. 拿到B的url地址 2. 改变其hash值 3. 在B中接收，onhashchange
    WebSocket的原理：
    1. var ws = new WebSocket('wss:echo.websocket.org'); 2. onopen, onmessage, onclose
    CORS的原理： fetch(‘/some/url’, {     method: ‘get’, // 加一些配置就可以实现跨域的通信，这里可以参考一篇文章 }).then(function (response) {     // xxx } ).catch(function(err){     // yyy } );
