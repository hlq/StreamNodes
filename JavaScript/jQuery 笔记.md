# jQuery 笔记

- 删除元素的属性removeAttr()
```js
$(this).removeAttr(name)
```

- jquery根据name属性查找
```js
//选择所有含有id属性的div元素 
$("div[id]") 
//选择所有的name属性等于'keleyicom'的input元素 
$("input[name='keleyicom']") 
//选择所有的name属性不等于'keleyicom'的input元素 
$("input[name!='keleyicom']") 
//选择所有的name属性以'keleyi'开头的input元素 
$("input[name^='keleyi']") 
//选择所有的name属性以'keleyi'结尾的input元素 
$("input[name$='keleyi']") 
//选择所有的name属性包含'keleyi'的input元素 
$("input[name*='keleyi']") 
//可以使用多个属性进行联合选择，该选择器是得到所有的含有id属性并且那么属性以keleyi结尾的元素 
$("input[id][name$='keleyi']") 
```

- jquery 子节点个数
```js
$(this).children().length
```
- checkbox 选中
```js
$(".xxx:checked")
```
