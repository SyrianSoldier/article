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
| 数组     | `let arr: number[] = [1,2]`              | `arr: list[int] = [1,2]`                 | 数组/列表        | 🔹 Python使用`list[元素类型]`  (Python3.9+)    |
| 元组     | `let t: [string, number] = ["a", 1]`     | `t: tuple[str, int] = ("a", 1)`          | 固定长度、固定类型的序列 | 🔹 类型和顺序必须一致                             |
| 枚举     | `enum Color { Red, Green}`               | `class Color(Enum): Red = 1`             | 枚举类型         | 🔹TS有原生`enum`<br/>🔹Python需导入：`from enum import Enum` |
| 空值     | `function fn(): void {}`                 | `def fn() -> None: ...`                  | 无返回值         | 🔹TS用void表示函数无返回值<br/>🔹Python用None      |
| any    | `let a: any = '任意类型' `                   | `a: Any = '任意类型'`                        | 任意类型         | 🔹 TS的`any`是内置类型<br/>🔹 Python需导入：`from typing import Any` |
| never  | `function error(): never { throw new Error()}` | `def error() -> NoReturn: raise Exception()` | "永不"类型       | 🔹表示函数抛异常或死循环<br/>🔹 Python用`NoReturn，需导入from typing import NoReturn` |
| 对象     | `let obj:Record<string, any> = {}`       | `obj:dict[str,Any] = {}`                 | 键值对集合        | 🔹TypeScript 使用 `Record<K, V>`<br/>🔹Python 使用 `dict[K, V] (Python3.9+)` |



## 如何描述JSON

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

### 类型别名

Typescript

```typescript	
type ID = string
const id: ID = "12312"
```

Python

```python
type ID = str 
id:ID = "12312"
```



### 联合类型

Typescript	

```typescript
type ID = string | number
const id: ID = "123123"
const id: ID = 123123
```

Python

```python
type ID = str | int
id: ID = "123123"
id: ID = 123123
```



### 交叉类型

Typescript	

```typescript
interface ObjA {
    a:string
    b:string
}

interface Obj2 {
    c:string
}

const obj: ObjA & Obj2 = {
    a: "a",
    b: "b",
    c: "c"
}
```

Python

```python
from typing import TypedDict

class ObjA(TypedDict):
    a: str
    b: str

class Obj2(TypedDict):
    c: str

class CombinedObj(ObjA, Obj2): # python没有该语法,但可靠多继承实现类型交叉的效果
    pass

obj: CombinedObj = {
    "a": "a",
    "b": "b",
    "c": "c"
}
```



### 字面量类型

Typescript

```typescript	
type UserName = "张三" | "李四"

userName1:UserName = "张三" // ✅ 
userName2:UserName = "李四" // ✅ 
userName3:UserName = "王二" // ❌
```

Python

```python
from typing import Literal

type UserName = Literal["张三"] | Literal["李四"]
# 或者写成 type UserName = Literal["张三", "李四"]

user_name1:UserName = "张三" # ✅
user_name2:UserName = "李四" # ✅
user_name3:UserName = "王二" # ❌
```



### 类型断言

Typescript

```typescript	
const someValue: any = "this is a string"
const someStr: string = someValue as string
```

Python

```python
from typing import Any,cast

some_value: Any = "this is a string"
some_str: str = cast(str, some_value) 
```



### 类型品牌(Type Branding)

> 类型品牌用于**创建和原类型一样但逻辑上不同**的新类型

Typescript

```typescript	
type UserId = number & { __brand: "UserId" } 
//这个 __brand 字段不会实际存在，只是告诉 TS：“这不是普通 number，而是一个带品牌的 number”

function getUser(id: UserId) { /**...**/ }

getUser(123123) // ❌ 静态检查不通过
getUser(123123 as UserId) // ✅ 合法
```

Python

```python
from typing import NewType

UserId = NewType("UserId", int)

def get_user(id: UserId):
    ...

get_user(123)          # ❌ 静态检查不通过
get_user(UserId(123))  # ✅ 合法
```



## 函数中的类型

### 函数定义

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



### 没有返回值的函数

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



### 函数抛出异常

Typescript

```python
from typing import Never  # 或 NoReturn

def fn() -> Never:
    raise Exception("抛出一个异常")
```

Python

```typescript
const fn = ():never => {
  raise new Error("抛出一个异常")
}
```



### 校验函数

> 自定义保护类型

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

## 类中的类型

### 类类型与实例类型

Typescript

```typescript
class A {}

const aInstance: A = new A();
const aClazz: typeof A = A;
```

Python

```python
class A():
    pass

a_instance:A = A()
a_clazz:type[A] = A
```



### 链式调用(返回实例)

Typescript

```typescript
class LinkA {
    a(): this {
        console.log("a")
        return this
    }

    b(): this {
        console.log("b")
        return this
    }

    c(): this {
        console.log("c")
        return this
    }
}

const linkA = new LinkA()
linkA.a().b().c()
```

Python

```python
from typing import Self 

class LinkA:
    def a(self) -> Self:
        print("a")
        return self

    def b(self) -> Self:
        print("b")
        return self

    def c(self) -> Self:
        print("c")
        return self

linkA = LinkA()
linkA.a().b().c()
```



### 泛型类

Typescript

```typescript
class Stack<T> {
  private items:T[] = []
  
  push(item:T) -> void {
    this.items.push(item)
  }
  
  pop():T | undefined {
    return this.items[this.items.length - 1];
  }
}

const numStack = new Stack<number>()
numStack.push(1) // 添加数字被允许
//numStack.push("a") // 添加字符串报错
```

Python

```python
class Stack[T]:
    def __init__(self) -> None:
        self.items: list[T] = []

    def push(self, item: T) -> None:
        self.items.append(item)

    def pop(self) -> T | None:
        return self.items[len(self.items) - 1]

num_stack = Stack[int]()
num_stack.push(1) # 添加数字被允许
# num_stack.push("a") # 添加字符串报错
```



## 扩展

### mypy的使用

> 放图片+文字教程

