---
title: 记一次手忙脚乱的base64debug之旅
date: 2019-09-10 00:32:26
tags:
---
这几天做了一个需求，读取上传的公私钥，然后利用私钥采用RSA加密摘要，发送给后端。其中运用到了`base64`的加解密，RSA加密采用的是node的[`Crypto`模块](https://nodejs.org/dist/latest-v10.x/docs/api/crypto.html)，`base64`的转码采用的是[js-base64](https://github.com/dankogai/js-base64), 然而万万没有想到，这里面有坑啊。
![](https://user-gold-cdn.xitu.io/2019/9/9/16d169612d63f39a?w=123&h=109&f=jpeg&s=10462)

（开个玩笑，这个库还是很不错的）
### 文件的读取
首先是文件的读取，采用的是`FileReader`, 并且二进制文件的读取应为`readAsArrayBuffer`，否则会乱码。
``` js
readFile(file) {
    return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = function(evt) {
            resolve(evt.target.result)
        };
        reader.readAsArrayBuffer(file);
    })
}
```

从这里读取到的数据,调用`Object.prototype.toString.call([data])`，结果为`[object Uint8Array]`。
> `ArrayBuffer`对象用来表示通用的、固定长度的原始二进制数据缓冲区。`ArrayBuffer`不能直接操作，而是要通过类型数组对象或`DataView` 对象来操作，它们会将缓冲区中的数据表示为特定的格式，并通过这些格式来读写缓冲区的内容。<br>
> `Uint8Array` 数组类型表示一个8位无符号整型数组，创建时内容被初始化为0。创建完后，可以以对象的方式或使用数组下标索引的方式引用数组中的元素。

类型数组对象就是`Uint8Array`之类的`TypedArray`，我们需要这些对象操作`ArrayBuffer`。

这是一个摸索了很久的点，因为有的公私钥读取可以直接采用` reader.readAsText(file)`。

### ArrayBuffer与base64的转换
另一个摸索了很久的东西便是`ArrayBuffer`与`base64`的转换，缘由于`js-base64`对这两个的转换并不支持。

#### ArrayBuffer转base64
很令人捉🐔的是，调用库中的方法`Base64.encode([ArrayBuffer])`，得出的数据一直不对。
略微看了一下源码。

``` js
var _Base64 = global.Base64;
var version = "2.5.1";
// if node.js and NOT React Native, we use Buffer
var buffer;
if (typeof module !== 'undefined' && module.exports) {
    try {
        buffer = eval("require('buffer').Buffer");
    } catch (err) {
        buffer = undefined;
    }
}
...
var _encode = buffer ?
    buffer.from && Uint8Array && buffer.from !== Uint8Array.from
    ? function (u) {
        return (u.constructor === buffer.constructor ? u : buffer.from(u))
            .toString('base64')
    }
    :  function (u) {
        return (u.constructor === buffer.constructor ? u : new buffer(u))
            .toString('base64')
    }
    : function (u) { return btoa(utob(u)) }
;
var encode = function(u, urisafe) {
    return !urisafe
        ? _encode(String(u))
        : _encode(String(u)).replace(/[+\/]/g, function(m0) {
            return m0 == '+' ? '-' : '_';
        }).replace(/=/g, '');
};
```

不知是不是采用`IIFE`的关系，`buffer`恒为`undefined`，并且此时`!urisafe`为真，会流入第一个条件，而的`u`为`Uint8Array`，强制类型转换会导致乱码。

故而不用库，用原生方法。
`Buffer.from([key]).toString('base64')` 这是`ArrayBuffer`转`base64`的原生方法。

#### base64转ArrayBuffer

``` js
function _base64ToArrayBuffer(base64) {
    var binary_string =  window.atob(base64);
    var len = binary_string.length;
    var bytes = new Uint8Array( len );
    for (var i = 0; i < len; i++)        {
        bytes[i] = binary_string.charCodeAt(i);
    }
    return bytes.buffer;
}
```

### and...

```
const encryptDigest = crypto.privateEncrypt({ key: privateKey },
    Buffer.from(_base64ToArrayBuffer(digest))
);
const res = crypto.publicDecrypt({ key: publicKey }, encryptDigest);

res.toString('base64') === digest ?
```

只要用私钥加密的摘要用公钥能成功解密，并且数据不变，就证明这个流程正确了。

其实用`window.atob`也可以将`ArrayBuffer`转`base64`，但是有大小限制，码位应在 0x00 ~ 0xFF 范围内。然而后端小哥说业务不支持`ascii`格式的，只能作罢。


> [javascript – 将base64字符串转换为ArrayBuffer](https://codeday.me/bug/20180112/117123.html)<br>
> [深入学习 Node.js Buffer](https://semlinker.com/node-buffer/#ArrayBuffer-%E5%92%8C-TypedArray)