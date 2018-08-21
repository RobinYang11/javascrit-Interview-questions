# 100 javascript  高级技巧
## 1.用promise 封装原生ajax
```
function ajax(options) {
    return new Promise(function(resolve, reject) {
        var xhr = new XMLHttpRequest();
        xhr.open(options.method, options.url, options.isAsync);
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
    isAsync: true
}).then(function(res) {
    console.log(res)
}).catch(function(res) {
    console.log(res)
})
```