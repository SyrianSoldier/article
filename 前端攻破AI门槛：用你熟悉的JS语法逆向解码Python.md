# 用你熟悉的JS语法逆向解码Python

## 一、常用数据类型

| 数据类型     | JS 示例                    | Python 示例                             | 语法差异说明                                   |
| -------- | ------------------------ | ------------------------------------- | ---------------------------------------- |
| 数值       | `let num = 42;`          | `num = 42`                            | 🔹Python不需要let, const                    |
| 字符串      |                          |                                       |                                          |
| └ 普通字符串  | ` "Hello, World!"`       | `"Hello, World!"`                     |                                          |
| └  模版字符串 | `Hello, ${World}!`       | `f"Hello, {World}!"`                  | 🔹JS用`飘号`, Python用`f""`<br />🔹JS用`${}` , Python用`{}` |
| └  原始字符串 | `String.raw`             | `r""`                                 | 🔹原始字符串中的 `\`被视为普通字符                     |
| 布尔值      | `true/false`             | `True/False `                         |                                          |
| 空值       | `null/undifined;`        | ` None`                               | 🔹JS 有两种空值类型(`null和undifined` )<br />🔹Python 只有 `None` |
| 数组       | `arr = [1, 2, 3]`        | `arr = [1, 2, 3]`                     |                                          |
| 元组       | \                        | `arr = (1,2,3)`                       | 🔹JS没有元组.  <br />🔹元组 ≈  无法增删改的数组        |
| 集合       | `s = new Set([1, 2, 3])` | `s = set([1,2,3])`<br />`s = {1,2,3}` | 🔹Python 还可以通过`{}`直接创建集合                 |
| 对象       | `obj = { key: "value" }` | `obj = { "key": "value" }`            | 🔹Python 的对象的key必须加引号                    |



## 二、分支、循环语句

### 2.1 分支语句 

```js
// js
if (x > 0) {

} else if (x == 0) {

} else {
	console.log("HELLO")
}


// python
if x > 0:
    
elif x == 0:
    
else:
	print("HELLO")
    
```



### 2.2 循环语句

#### 2.2.1 普通for循环

```js
// js
for (let i = 0; i < 5; i++) {

}

//python range(5)代表[0, 5)
for i in range(5):
    
```



#### 2.2.2 循环数组

```js
//js
const arr = [1, 2, 3, 4];
for (let item of arr) {
  console.log(item)
}

//python
arr = [1, 2, 3, 4]
for item in arr:
    print(item)

my_tuple = (1, 2, 3, 4, 5)
for item in my_tuple:
    print(item)

my_set = {1, 2, 3, 4, 5}
for item in my_set:
    print(item)


// 循环时, 同时获得索引和元素
for index, element in enumerate(my_list):
    print(f"Index: {index}, Element: {element}")

for index in range(len(my_string)):
    print(f"Index: {index}, Element: {my_string[index]}")
```



#### 2.2.3 循环对象

```js
//js
for (let key in obj) {
    console.log(`${key}: ${obj[key]}`);
}

//python
obj = { "a": 1, "b": 2 }
for key, value in obj.items():
    print(f"{key}: {value}")
```



#### 2.3.4 while循环

```js
//js
while (i < 5) {
}

//python
while i < 5:
    print(i)
```


## 三、运算符

### 3.1 三目运算符

```js
//js
const message = x > 0 ? "条件成立时候返回的结果" : "条件不成立时候返回的结果"

//python
message = "条件成立时候返回的结果" if x > 0 else "条件不成立时候返回的结果"
```



### 3.2 逻辑与和逻辑或

```js
// js
const result = getReulst() || "默认值"
const result = obj && obj.name

//python
result = getReulst() or "默认值"
result = obj and obj.name
```



### 3.3 取反操作符和双取反操作符

```js
const condition = x > 0
const bool = !condition

//python
condition =  x > 0
bool = not condition
```





### 3.4 自增/减运算符

```js
// js
a++
--a

//python没有自增/自减运算符, 用+=/-=实现
a+=1
a-=1
```



### 3.5 === 和 ==

```js
//js
if(a == b){}
if(a === b){}
if (a !== b){}  

//python
// is 和 is not 用来比较两个"引用数据类型"的内存地址是否相同, 与js的===和!==功能一致
// == 和 != 用来比较两个'字面量'是否相等, py中一般字符串使用 ==/!= 判断, 对象使用 is/is not 判断
if a == b:
if a is b:
if a is not b:
```



## 四、语法特性

### 4.1 真假值与隐式转换


| **类型/值描述**  | **JavaScript 语法示例**          | **JavaScript 判断** | **Python 语法示例**          | **Python 判断** | **关键差异说明**                       |
| ----------- | ---------------------------- | ----------------- | ------------------------ | ------------- | -------------------------------- |
| **布尔假值**    | `false`                      | 假值                | `False`                  | 假值            |                                  |
| **布尔真值**    | `true`                       | 真值                | `True`                   | 真值            |                                  |
| **数字假值**    | `0`, `-0`                    | 假值                | `0`, `0.0`, `-0`         | 假值            | **一致**                           |
| **数字真值**    | `1`, `-1`, `3.14`            | 真值                | `1`, `-1`, `3.14`        | 真值            | **一致**                           |
| **NaN**     | `NaN`                        | 假值                | `float('NaN')`           | 真值            | Python 中 `NaN` 属于 `float` 类型且为真值 |
| **空字符串**    | `""`                         | 假值                | `""`                     | 假值            | **一致**                           |
| **非空字符串**   | `"abc"`, `" "`(带空格)          | 真值                | `"abc"`, `" "`(带空格)      | 真值            | **一致**                           |
| **空数组/列表**  | `[]`                         | 真值                | `[]`                     | 假值            | **最大差异！** JS 空数组为真，Python 空列表为假  |
| **空对象/字典**  | `{}`                         | 真值                | `{}`                     | 假值            | **最大差异！** JS 空对象为真，Python 空字典为假  |
| **空集合**     | `new Set()`                  | 真值                | `set()`                  | 假值            | **最大差异！** JS 空集合为真，Python 空集合为假  |
| **特殊假值**    | `null`, `undefined`          | 假值                | `None`                   | 假值            |                                  |
| **函数/匿名函数** | `function() {}` 或 `() => {}` | 真值                | `lambda: None`           | 真值            | **一致**                           |
| **非空容器**    | `[1]`, `{a: 1}`              | 真值                | `[1]`, `{"a": 1}`, `{1}` | 真值            | **一致**                           |



### 4.2 真假值的实际运用案例

#### **1. 存在性检查（避免访问未定义属性）**

**场景**：如果对象存在且属性存在，才执行操作。

javascript

```js
const a = { b: "hello" };
if (a && a.b) { // 若 a 存在且 a.b 存在
  console.log(a.b); // 输出 "hello"
}
```

python

```python
a = {"b": "hello"}
if a and "b" in a:  # 若 a 存在（非空字典）且键 "b" 存在
  print(a["b"])  # 输出 "hello"
```

#### **2. 短路逻辑（设置默认值）**

**场景**：若变量为假值，则赋予默认值。

js

```js
// JavaScript
const value = null;
const result = value || "default"; // value 为假值，返回 "default"
console.log(result); // 输出 "default"
```

python

```python
# Python
value = None
result = value or "default"  # value 为假值，返回 "default"
print(result)  # 输出 "default"
```

#### **3. 空值处理**

**场景**：检查变量是否为假值（如 `null`/`undefined`/`None`）。

javascript

```js
// JavaScript
function checkValue(val) {
  if (!val) { // 若 val 是假值（如 null、undefined、""、0）
    console.log("无效值");
  }
}
checkValue(null); // 输出 "无效值"
```

python

```python
# Python
def check_value(val):
  if not val:  # 若 val 是假值（如 None、""、0、空容器）
    print("无效值")

check_value(None)  # 输出 "无效值"
```

#### **4. 综合示例（链式安全访问）**

**场景**：安全访问深层属性。

javascript

```js
// JavaScript
const data = { user: { profile:"11" } };
const name = data && data.user && data.user.profile  || "unknown";
console.log(name); // 输出 "Alice"
```

python

```python
# Python
data = {"user": {"profile": "11"}}
name = data and data.get("user") and data.get("user").get("profile") or "unknown";
print(name)
```



## 五、数组常用方法

#### ES5

| 操作          | JavaScript 数组方法                | Python 实现                                | 备注                                       |
| ----------- | ------------------------------ | ---------------------------------------- | ---------------------------------------- |
| **添加元素**    | `arr.push(4)`                  | `arr.append(4)`                          |                                          |
| **删除元素**    | `arr.pop()`                    | `arr.pop(index)`                         | 🔹Python的`pop`方法不传参默认删除最后一个元素, 传入索引则删除指定索引的元素 |
| **插入元素**    | `arr.splice(index,0,...新增的元素)` | `list.insert(index, element)`            |                                          |
| **获取数组长度**  | `arr.length`                   | `len(arr)`                               |                                          |
| **合并数组**    | `arr.concat(arr2)`             | `arr + arr2`                             | 🔹JS和Python都返回新数组/列表                     |
| **排序**      | `arr.sort()`                   | `arr.sort()`或<br />`sorted(arr,key=lambda,)`<br /> |  `sorted`的key参数可以为一个lambda表达式或者函数, 自定义排序的逻辑, 这个函数接收列表中的元素，并返回一个用于排序的值 |
| **排序(降序)**      | `arr.sort((a, b) => b - a)`                   |  `arr.sort(reverse=True)`或<br/> `sorted_words = sorted(words, key=len, reverse=True)`  | 原地排序，Python可指定key，JS默认字符串排序 |
| **反转数组**    | `arr.reverse()`                | `arr.reverse()`<br />` arr[::-1]`        | `arr[::-1]`会返回翻转后的新数组                    |
| **数组切片**    | `arr.slice(start, end)`        | `arr[start:end:step]`                    | `step`可以定义一个步长, 按步长截取元素                  |
| **数组浅拷贝**   | `arr.slice(0)`                 | `arr[:]`                                 |                                          |
| **查找元素的索引** | `arr.indexOf(element)`         | `arr.index(element)`                     |                                          |
| **数组转字符串**  | `arr.join(",")`                | `",".join(arr)`                          |                                          |
| **扁平化数组**   | `arr.flat()`                   | `arr = [elem for sublist in arr for elem in sublist]` |                                          |

------

#### ES6

| 操作             | JavaScript 数组方法         | Python 实现                                | 备注                                       |
| -------------- | ----------------------- | ---------------------------------------- | ---------------------------------------- |
| **filter数组**   | `arr.filter(callback)`  |  `[x for x in arr if condition]`<br />  |                                          |
| **every函数**    | `arr.every(callback)`   |  `all(callback(x) for x in arr)`<br /> | `all()`函数接受一个可迭代对象作为参数，检查所有元素是否满足条件，返回布尔值。<br/>例子:`result = all(x > 0 for x in numbers)` |
| **some函数**     | `arr.some(callback)`    |  `any(callback(x) for x in arr)`<br /> | `any()`函数接受一个可迭代对象作为参数，检查至少一个元素是否满足条件，返回布尔值。<br/>例子: `result = any(x > 10 for x in numbers)` |
| **map数组**      | `arr.map(callback)`     |  `[callback(x) for x in arr] 或<br/> list(map(callback, arr)) | callback可用普通函数或lambda表达式;简单映射时，列表生成式更直接。<br />如:`[{"name": name} for name in ["张三","李四"]]` |
| **find元素**     | `arr.find(callback)`    | `next((x for x in arr if condition), None)` | 若只是检测元素是否在数组中, 请用`element in arr`进行判断    |
| **includes元素** | `arr.includes(element)` | `element in arr`                         |                                          |
| **reduce**     | `arr.reduce(callback)`  | `from functools import reduce`<br />` reduce(callback, list, [initializer])` | `callback`的参数和`js`的`reduce callback`的一致  |


## 六、正则

> python需要提前`import re`

| 功能      | JavaScript                    | Python                            |
| ------- | ----------------------------- | --------------------------------- |
| 是否匹配    | `/regex/.test(str)`           | `re.search(pattern, str)`         |
| 提取第一个匹配 | `str.match(/regex/)`          | `re.search(pattern, str).group()` |
| 提取所有匹配  | `str.matchAll(/regex/g)`      | `re.findall(pattern, str)`        |
| 替换      | `str.replace(/regex/, "new")` | `re.sub(/regex/, "new", str)`     |



## 七、时间/日期方法

> 需要`import datetime`

| 操作              | JavaScript                               | Python                                   | 备注                                       |
| --------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| **获取当前时间的时期对象** | `new Date()`                             | `datetime.datetime.now()`                |                                          |
| **获取当前时间戳**     | `Date.now()` 或 `new Date().getTime()`    | 秒级时间戳:`datetime.datetime.now().timestamp()`<br />毫秒级时间戳: `int(datetime.datetime.now().timestamp()* 1000)` | 🔹Python返回秒级时间戳<br />🔹JS返回毫秒级时间戳        |
| **获取年**         | `date.getFullYear()`                     | `date.year`                              |                                          |
| **获取月**         | `date.getMonth()`（0-11）                  | `date.month`（1-12）                       | 🔹Python月份从1月开始<br />🔹JS月份从0开始          |
| **获取日**         | `date.getDate()`                         | `date.day`                               |                                          |
| **获取星期几**       | `date.getDay()`（0=周日）                    | `date.weekday()`（0=周一）                   | 🔹Python获取星期,0代表周一 <br />🔹JS获取星期, 0代表周一 |
| **获取小时**        | `date.getHours()`                        | `date.hour`                              |                                          |
| **获取分钟**        | `date.getMinutes()`                      | `date.minute`                            |                                          |
| **获取秒**         | `date.getSeconds()`                      | `date.second`                            |                                          |
| **格式化日期**       | `dayjs(date).format("YYYY-MM-DD HH:mm:ss")` | `date.strftime("%Y-%m-%d %H:%M:%S")`     |                                          |
| **获取时间差**       | `date1 - date2`（毫秒）                      | `(date1 - date2).total_seconds()`        | 🔹JS和python中都支持Date对象的减法<br />🔹Python此方法返回一个浮点数秒 |




## 八、序列化 & 反序列化

### 8.1 **JSON序列化**

```js
// js
const obj = {name: "Alice", age: 30}
str = JSON.stringfy(obj)
```

```python
# python
import json
obj = {"name": "Alice", "age": 30}
json_str = json.dumps(obj)
```

### 8.2 JSON 反序列化

```js
//js
const json_str = '{"name": "Alice", "age": 30}'
JSON.parse(json_str)
```

```python
# python
json_str = '{"name": "Alice", "age": 30}'
json.loads(json_str)
```

###  8.3 JSON 格式化（美化输出）

```python
#python
print(json.dumps(obj, indent=4))
# 输出:
# {
#     "name": "Alice",
#     "age": 30
# }

```



## 九、前置知识

### 9.1 python的列表生成式

> `python`的列表生成式语法可以快速生成`list`, `python`的列表生成式主要迭代`list`, **也可以迭代其他Iterable数据结构**

#### 9.1.1 基本列表生成式

```python
# 循环list时, 每次迭代都执行表达式, 并将表达式的返回值组成为list
[表达式 for item in list]

# 例
sum_list =  [item * 10 for item in [1,2,3]] # [10,20,30]
```



#### 9.1.2 带条件的列表生成式

```python
# 循环list, 且只有满足condition才执行本次迭代, 并将表达式的返回值组成为list
[表达式 for item in list if condition]

# 例
sum_list =  [item * 10 for item in [1,2,3] if item > 1] # [20,30]
```



#### 9.1.3 多重循环列表生成式

```python
# 多重for循环生成式
[表达式 for item1 in list1 for item2 in list2]

# 例
pairs = [(x, y) for x in range(1, 4) for y in range(1, 4)]# [(1, 1), (1, 2), (1, 3), (2, 1), (2, 2), (2, 3), (3, 1), (3, 2), (3, 3)] 相当于双重for循环，for里嵌套for
```

#### 9.2 python的字典生成式

```python
{key的表达式: value的表达式 for item in iterable}

#例
names = ["zhangsan","lisi"]
obj = { names[index]:index for index in range(len(names)) } # {'zhangsan': 0, 'lisi': 1}
```