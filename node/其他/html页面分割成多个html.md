# html页面分割成多个html
[TOC]
***
### iframe
通过iframe引入其他html，引入的html是一个独立的html

***
### art-template服务端
`art-template服务端模板`

1. 子模板
node端，通过express，art-template和express-art-template三个模块
渲染的模板html插入
```html
{{include './header.html'}}
```
header.html写html的body内的标签内容

2. 模板继承
服务端渲染index.html，会根据{{extend}}首先寻找骨架页面，需要被替换的部分则用{{block 'xxx'}}
{{/block}}表示，骨架页面该部分默认被渲染，但具体页面该区域需要被替换，则在index.html具体页面同样用{{block 'xxx'}}{{/block}}表示，当实际渲染时，同名的block，则在渲染时，骨架页面该区域被替换
index.html
```html
{{extend './layout.html'}}

//渲染该页面时，替换掉骨架页面layout.html同名部分
{{block "content1"}}
    <h1>骨架上需要被替换的部分</h1>
{{/block}}
```
layout.html
```html
    {{include "./header.html"}}
    
    {{include "./content.html"}}


    {{block "content1"}}
    <h1>骨架页面原来部分</h1>
    {{/block}}

    {{block "content2"}}
    <h1>骨架页面原来部分</h1>
    {{/block}}

    {{include "./header.html"}}
```
**思考顺序**
`一个html，存在header，footer等需要复用部分，而中间部分根据实际需求选择性的替换或保留，layout.html骨架页面`
`在骨架页面的基础上需要进一步修改得到index.html等页面`
`layout.html为骨架页面，便于复用，而每个具体的页面在骨架页面基础上进行部分调整`


