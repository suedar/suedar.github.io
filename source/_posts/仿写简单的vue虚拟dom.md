---
title: 仿写简单的vue虚拟dom
date: 2017-11-08 20:32:36
tags:
---

##### 最近在学vue 所以仿写了一些render函数的虚拟dom,还是挺有意思的:)

``` js  
        function createElement(tag, prop, children) {
            if (!(this instanceof createElement)) {
                return new createElement(tag, prop, children)
            }
            if (Object.prototype.toString.call(prop) === "[object Array]") {
                children = prop;
                prop = {};
            }
            this.tag = tag;
            this.prop = prop;
            this.children = children;
            var count = 0;
            this.children.forEach(function(item) {
                if (item instanceof createElement) {
                    count += item.count;
                }
                count++;
            })
            this.count = count;
        }
        createElement.prototype.render = function() {
            var tag = this.tag;
            var prop = this.prop;
            var children = this.children;
            var dom = document.createElement(tag);
            for (item in prop) {
                dom.setAttribute(item, prop[item]);
            }
            children.forEach(function(child) {
                if (child instanceof createElement) {
                    var childDom = child.render();
                } else {
                    var childDom = document.createTextNode(child);
                }
                dom.appendChild(childDom);
            })
            return dom;
        }
```

以上为代码片

测试节点如下
``` js
        var dom = createElement("div", {
            class: "demo"
        }, ["you are", createElement("p", {
            id: "lala"
        }, ["gorgeous"])]);

        console.log(dom.render());
```

得出结果
![图片](http://oz3on0sof.bkt.clouddn.com/%E6%8D%95%E8%8E%B7.PNG)