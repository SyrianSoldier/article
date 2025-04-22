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

| 基本数据类型 | Typescript                               | Python                                   | 含义           | 备注                                       |
| ------ | ---------------------------------------- | ---------------------------------------- | ------------ | ---------------------------------------- |
| 布尔值    | `let flag: boolean = true`               | `flag: bool = True`                      | 真/假值         | 🔹 类型注解：TS用`boolean`<br/>🔹Python用`bool`,且布尔值`True/False`首字母要大写 |
| 数字     | `let num: number = 10`                   | `num: int = 10` 或<br/>`num: float = 10.0` | 数值类型         | 🔹TS统一使用`number`<br/>🔹 Python区分整型和浮点型   |
| 文本     | `let text: string = "hello"`             | `text: str = "hello"`                    | 字符串          | 🔹TS用`string`<br/>🔹Python用`str`         |
| 数组     | `let arr: number[ ] = [1,2]`             | `arr: list[int] = [1,2]`                 | 数组/列表        | 🔹 Python使用`list[元素类型]`  (Python3.9+)    |
| 元组     | `let t: [string, number] = ["a", 1]`     | `t: tuple[str, int] = ("a", 1)`          | 固定长度、固定类型的序列 | 🔹 类型和顺序必须一致                             |
| 枚举     | `enum Color { Red, Green}`               | `class Color(Enum): Red = 1`             | 枚举类型         | 🔹TS有原生`enum`<br/>🔹Python需要借助`Enum类`实现  |
| 空值     | `function fn(): void {}`                 | `def fn() -> None: ...`                  | 无返回值         | 🔹TS用void表示函数无返回值<br/>🔹Python用None      |
| any    | `let a: any = '任意类型'                     | `a: Any = '任意类型'`                        | 动态类型         | 🔹 TS的`any`是内置类型<br/>🔹 Python需导入：`from typing import Any` |
| never  | `function error(): never { throw new Error()}` | `def error() -> NoReturn: raise Exception()` | 永不返回的函数      | 🔹表示函数抛异常或死循环<br/>🔹 Python用`NoReturn，需导入from typing import NoReturn` |
| 对象     | `let obj:Record<string, any> = {}`       | `obj:dict[str,Any] = {}`                 | 键值对集合        | 🔹TypeScript 使用 `Record<K, V>`<br/>🔹Python 使用 `dict[键类型, 值类型] (Python3.9+)` |

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

## 类

### 类类型与实例类型

### 链式调用(返回实例)

### 泛型类

## 描述复杂对象

Typescript

```typescript
interface User {
    readonly ID: string
    username:string
    email?: string
}

const user:User = {
    ID: "1",
    username: "ZhangSan"
}
```

Python

```python
from typing import TypedDict, NotRequired, ReadOnly 

class User(TypedDict):
  ID: ReadOnly[str]
  username: str
  email: NotRequired[str]

user:User = {
    "ID": "1",
    "username": "ZhangSan",
}
```



## 语法特性

### 联合类型与交叉类型

### 字面量类型

### 类型断言

###类型推导

### 类型别名

###类型谓词

### 索引类型

### 只读字段

### ts中的Brand/py中的newType





## 扩展

### mypy的使用

> 放图片+文字教程

