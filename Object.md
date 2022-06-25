```js
Object.keys() 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致。
Object.keys({name: "tom", age: 11})  ===> ['name','age']  // 返回以对象key为元素的数组
Object.keys([1, 2, 3])  ===> ['0', '1', '2']  // 返回索引组成的数组
Object.keys('hello')  ===> ['0', '1', '2', '3', '4']  // 返回索引组成的数组


Object.entries({naem: 'tom', age: 11}) ===> [['naem': 'tom'],['age': 11]]
Object.entries(['tom', 11]) ===> [['0': 'tom'],['1': 11]]
Object.entries('he') ===> [['0':'h'],['1':'e']]
```



