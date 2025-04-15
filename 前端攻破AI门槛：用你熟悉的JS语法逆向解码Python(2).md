# 用你熟悉的JS语法逆向解码Python

> 本文基于Python 3.13.3

注:

1. **重要:**先安装个**最新的(3.13.3版本)**python, 确保能跑代码, 及时跑代码验证自己写的对不对
2. **重要:**写之前保证自己对typescript有个最基本的了解(知道大概就可, 遇到不懂的再搜)
3. **非常重要:**搜索:"mypy是什么", "如何在vscode上安装/配置mypy插件". 保证在vscode上写python类型的时候, 有类型提示
4. 基本类型和扩展/mypy的使用交给你来写
5. 剩下的其他内容可以挑着写, 写之前在评论中给我说下, 我也再写不要写重复了
6. 基本类型写完就可以提交个pr, 剩下的内容每写完一个提交一个pr 
7. **非常重要**所有python类型以这个文档记载的为准https://docs.python.org/zh-cn/3/library/typing.html#specification-for-the-python-type-system  , 编写方法应该是 "查百度/chatgpt --> 参考下上面那个文档的说法 --> 编写". 
8. **非常重要**所有python的类型以最新语法为准(ps: 大模型查出来很多上都是老的,还是要参考上面面的官方文档)

## 基本类型

| 基本数据类型 | Typescript                      | Python                   | 含义   | 备注                                       |
| ------ | ------------------------------- | ------------------------ | ---- | ---------------------------------------- |
| 布尔值    |                                 |                          |      |                                          |
| 数字     |                                 |                          |      |                                          |
| 文本     |                                 |                          |      |                                          |
| 数组     |                                 |                          |      |                                          |
| 空值     |                                 |                          |      |                                          |
| 元组     |                                 |                          |      |                                          |
| 枚举     |                                 |                          |      |                                          |
| any    |                                 |                          |      |                                          |
| void   |                                 |                          |      |                                          |
| never  |                                 |                          |      |                                          |
| 对象     | `let obj:Record<str, any> = {}` | `obj:dict[str,Any] = {}` |      | 1. 这里写python和js语法的差异总结<br />2. 写两行, python一行,js一样<br />3. 每一行前加个符号,跟第一篇博客一样 |

## 函数类型与泛型

### 函数定义中的类型

Typescript

```typescript
const fn = (a:number, b:string): boolean => {
  return true
}
```

Python

```python
def fn(a:int, b:str) -> bool:
    return True
```



### 回调函数

Typescript

```ts
const fn = (callback: (a: number, b: string) => boolean): void => {
  callback(1, "a")
}
```

Python

```python
from typing import Callable

def fn(callback: Callable[[int, str], bool]) -> None:
    callback(1, "a")
```



### 函数泛型

Typescript

```typescript
function fn<T>(a: T): T {
  return a;
}

const result = fn("hello"); // 自动推断 T 为 string，返回类型也是 string
```

Python

```python
def fn[T](a:T) -> T:
    return a

result = fn("hello")     # 自动推断 T 为 string，返回类型也是 string
```



### 异步函数

> 对应python中返回一个协程



> TODO: 例子不对, 要修改

Typescript

```typescript
const fetchSomething = async (): Promise<{data:any}> => {
  return axios.get('http://xxx/test')
};

const result = await fetchSomething(); 
```

Python

```python
async def fetchSomething() -> Coroutine[Any]:
  return httpx.get('http://xxx/test')


result = await fetchSomething(); 
```



### 函数没有返回值

Typescript

```typescript
const fn = ():void => {
   console.log("test") 
}
```

Python

```python
def fn() -> None:
    print("test")
```



### 函数抛出异常的返回值

```python
from typing import Never  # 或 NoReturn

def fn() -> Never:
    raise Exception("抛出一个异常")
```



```typescript
const fn = ():never => {
  raise new Error("抛出一个异常")
}
```



### 类型谓词(自定义保护类型)

Typescript

```typescript
// 定义自定义类型保护
const isString = (value: unknown): value is string => {
    return typeof value === "string"
}

// 使用自定义类型保护
const fun = (value: string | number): void => {
    if(isString(value)){
        console.log(value) // 此时value被推断为string类型
    }else {
        console.log(value) // 此时value被推断为number类型
    }
}
```

Python

```python
from typing import TypeIs, Any

# 定义自定义类型保护
def is_string(value:Any) -> TypeIs[str]:
    return isinstance(value, str)

#  使用自定义类型保护
def fn(value: str | int) -> None:
    if is_string(value): # 此时value被推断为string类型
        print(value)
    else:
        print(value)     # 此时value被推断为number类型

```

###提取函数的参数参数类型/返回值类型

## 类

### 类类型与实例类型

### 链式调用(返回实例)

### 泛型类



## 接口与类型别名

### 类型别名

### 接口



## 语法特性

### 联合类型与交叉类型

### 字面量类型

### 类型断言

###类型推导

###类型谓词

### 索引类型

### 只读字段

### ts中的Brand/py中的newType





## 扩展

### mypy的使用

> 放图片+文字教程

