---
title: vue小记
date: 2018-03-23 20:40:06
tags:
---

使用vue也有一段时间了，现在对vue的一些以前没有注意到的点小结一番~

**本文均采用npm安装依赖**

## json-server

数据存储的利器啊，我之前是采用[easy-mock](https://easy-mock.com/)，遗憾的是其只能使用get请求。

在[json-server](https://github.com/typicode/json-server)中 我们采用`npm install -g json-server`安装依赖。

在最外层文件新建mock文件，下建db.json

![showImage](http://oz3on0sof.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-03-22%20%E4%B8%8B%E5%8D%882.37.31.png)

然后采用json格式输入数据
``` json
{
  "posts": [
    { "id": 1, "title": "json-server", "author": "typicode" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 }
  ],
  "profile": { "name": "typicode" }
}
```

然后改改script，在`package.json`中，修改script

``` js
    "scripts": {
        "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
        "start": "node build/dev-server.js",
        "build": "node build/build.js",
        "mock": "json-server mock/db.json --port 9092",
        "mockdev": "npm run mock & npm run dev"
    },
```

你可以用`npm run dev`打开项目`npm run mock`打开json-server,`npm run mockdev`两个一起打开~~

在localhost9092可以看到我们的mock数据，端口地址可以在`port`后面改

![localhost:9092](http://oz3on0sof.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-03-22%20%E4%B8%8B%E5%8D%883.18.07.png)

要对数据进行操作，需设置我们的基地址
``` js 
let baseUrl = 'http://localhost:9092'; 
```

配合axios使用，即可对数据进行增删改查

``` js
import axios from 'axios'
export default async(url = '', data = {}, type = 'GET', method = 'fetch') => {
	type = type.toUpperCase();
	url = baseUrl + url;

	if (type == 'GET') { // search
		try {
            var res = await axios.get(url)
            return res
        } catch (error) {
            console.error(error)
        }
	} else if(type == 'PUT') { // edit
		try {
            await axios.put(url,data.data)
        } catch (error) {
            console.error(error)
		}
	} else if(type == 'POST') { // add
		try {
			await axios.post(url,data.data)
        } catch (error) {
            console.error(error)
		}
	} else if(type == 'DELETE') { // delete
		try {
            await axios.delete(url,data.data)
        } catch (error) {
            console.error(error)
		}
	}
}
```

比如我们要对数据中的posts进行get操作，只需在基地址后加上`/posts`，而若是要对其中的id为1的进行操作，则在基地址后加上`/posts/1`

?

## vuex

使用[vuex](https://vuex.vuejs.org/zh-cn/)的时候参照了vue大项目[elm](https://github.com/bailicangdu/vue2-elm)，觉得这种数据分离的方式特别好，推荐给大家

首先安装依赖`npm install vuex --save`

新建文件夹store，下建`index.js`和`mutation.js`,这里我只建了`mutation.js`，原版还有`getter.js`和`action.js`，但因为项目中没用到，故没有建。。。

![mulu](http://oz3on0sof.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-03-22%20%E4%B8%8B%E5%8D%883.26.00.png)

`index.js`
``` js
import Vue from 'vue'
import Vuex from 'vuex'
import mutations from './mutations'

Vue.use(Vuex)

const state = {
    example: ''
}


export default new Vuex.Store({
    state,
    mutations
})
```


`mutation.js`
``` js
export default {
    setExample (state, newData) {
        state.example = newData
    }
}
```
从而在我们的工程文件中，引入相应的函数传入相应的数据即可

`helloWorld.vue`

``` js
...mapMutations([
    'setExample'
]),
```

``` js
    async initData(){ // 异步大法好
        let res = await getData();
        this.setExample(res.data)
        this.data = res.data;
    },
```

`service/getData.js`
``` js
import fetch from '../config/fetch'

export const getData = () => fetch('/posts', {
});
```

最后，项目结构是酱紫的

![project](http://oz3on0sof.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-03-22%20%E4%B8%8B%E5%8D%883.44.50.png)

## 还有一点点。。。

项目中还用到了html转pdf，放[github](https://github.com/suedar/how-transform-html-into-pdf)上了，感兴趣可以看一下~

感谢你的阅读，希望你哈皮每一天
