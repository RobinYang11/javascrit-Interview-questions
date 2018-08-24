# 100 javascript  高级技巧
## 1.用promise 封装原生ajax
```
function ajax(options) {
    return new Promise(function(resolve, reject) {
        var xhr = new XMLHttpRequest();
        xhr.open(options.method, options.url, options.isAsync);
        //设置h请求头
        Object.keys(options.headers).forEach(function(i){
            xhr.setRequestHeader(i,options.headers[i]);
        })
        //设置请求超时时间
        xhr.timeout=options.timeout;
        //超时回调函数
        xhr.ontimeout=function(){
            xhr.abort();
            console.log("xhr aborted!")
        }
        xhr.onreadystatechange = function() {
            if(this.readyState == 4) {
                if(this.status == 200) {
                    resolve(xhr.responseText)
                } else {
                    reject(xhr.responseText)
                }
            }
        }
        xhr.send();
    })
    }

    ajax({
    method: 'get',
    url: 'a.json',
    isAsync: true,
    timeOut:50,
    headers:{
        "x-test":'robin'
    }
    }).then(function(res) {
    console.log(res)
    }).catch(function(res) {
    console.log(res)
    })
```

## 2.原生js实现事件代理


```
function delegate(e, srcSelector, targetSelector, fn) {
    var src = document.querySelectorAll(srcSelector);
    var target = document.querySelectorAll(targetSelector)[0];
    target.addEventListener(e, function(event) {
        var isDelegated = false;
        for(var i = 0; i < src.length; i++) {
            if(src[i] == event.target) {
                isDelegated = true;
            }
            if(isDelegated) {
                fn();
                return;
            }
        }
    })
}

delegate("click", "ul>li", "#list", function() {
    console.log("robin")
})
```