#### 完成 convert(list) 函数，实现将 list 转为 tree

```````javascript
const list = [
	{
    "id": 19,
    "parentId": 0,
  },
  {
    "id": 18,
    "parentId": 16,
  },
  {
    "id": 17,
    "parentId": 16,
  },
  {
    "id": 16,
    "parentId": 0,
  }
];

const result = convert(list, 'parentId', 'id', 0);
const tree = {
  "id": 0,
  "children": [
    {
      "id": 19,
      "parentId": 0
    },
    {
      "id": 16,
      "parentId": 0,
      "children": [
        {
          "id": 18,
          "parentId": 16
        },
        {
          "id": 17,
          "parentId": 16
        }
      ]
    }
  ]
}
```````

代码实现如下：

```javascript
function convert (list, parentKey, currentKey, rootValue) {
	let map = new Map();
	for (let i = 0; i < list.length; i++) {
	let node = list[i];
	if (map.has(node[parentKey])) {
		let arr = map.get(node[parentKey]);
		arr.push(node);
	} else {
	 map.set(node[parentKey], [node]);
	}
}
list.forEach(e => {
	if (map.has(e[currentKey])) {
		e.children = map.get(e[currentKey]);
	}
});
return list.filter(e => e[parentKey] === rootValue);
}
```

#### 对象扁平化

说明：请实现 flatten(input) 函数，input 为一个 javascript 对象（Object 或者 Array），返回值为扁平化后的结果。
示例：

```javascript
var input = {
    a: 1,
    b: [ 1, 2, { c: true }, [ 3 ] ],
    d: { e: 2, f: 3 },
    g: null, 
}
var output = flatten(input);
//output如下
{
    "a": 1,
    "b[0]": 1,
    "b[1]": 2,
    "b[2].c": true,
    "b[3][0]": 3,
    "d.e": 2,
    "d.f": 3,
    //"g": null,  值为null或者undefined，丢弃
}

```

代码实现如下：

```javascript
function isObject (o) {
  return Object.prototype.toString.call(o) === '[object Object]'
}

function isArray (o) {
  return Array.isArray(o);
}

let obj = {};

const flatten = function (input, keys = '') {
  if (!isObject(input) && !isArray(input))
  	obj[keys] = input;
  for (let key in input) {
  	if (isObject(input[key])) {
			flatten(input[key], key);

  	} else if (isArray(input[key])) {
			for (let i in input[key]) {
				flatten(input[key][i], ${key}.[${i}]);
  		}
		} else if (input[key]) {
  		keys = ${keys ?${keys}.: ''}${isArray(input) ?[${key}]: key};
			obj[keys] = input[key];
    }
  }
  return obj;
}
```



