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
| 布尔值    |  `let flag: boolean = true`     | `flag: bool = True` | 真/假值 | 🔹 类型注解：TS用`boolean`<br/>🔹Python用`bool`,且布尔值`True/False`首字母要大写 |
| 数字     | `let num: number = 10` | `num: int = 10` 或<br/>`num: float = 10.0` | 数值类型 | 🔹TS统一使用`number`<br/>🔹 Python区分整型和浮点型 |
| 文本     | `let text: string = "hello"` | `text: str = "hello"` | 字符串 | 🔹TS用`string`<br/>🔹Python用`str` |
| 数组     | `let arr: number[ ] = [1,2]` | `arr: list[int] = [1,2]` | 数组/列表 | 🔹 Python使用`list[元素类型]`  (Python3.9+) |
| 元组     | `let t: [string, number] = ["a", 1]` | `t: tuple[str, int] = ("a", 1)` | 固定长度、固定类型的序列 | 🔹 类型和顺序必须一致 |
| 枚举     | `enum Color { Red, Green}` | `class Color(Enum): Red = 1` | 枚举类型 | 🔹TS有原生`enum`<br/>🔹Python需要借助`Enum类`实现 |
| 空值     | `function fn(): void {}` | `def fn() -> None: ...` | 无返回值 | 🔹TS用void表示函数无返回值<br/>🔹Python用None |
| any    | `let a: any = '任意类型'                       | `a: Any = '任意类型'`                        | 动态类型 | 🔹 TS的`any`是内置类型<br/>🔹 Python需导入：`from typing import Any` |
| never  | `function error(): never { throw new Error()}` | `def error() -> NoReturn: raise Exception()` | 永不返回的函数 | 🔹表示函数抛异常或死循环<br/>🔹 Python用`NoReturn，需导入from typing import NoReturn` |
| 对象     | `let obj:Record<string, any> = {}` | `obj:dict[str,Any] = {}` | 键值对集合 | 🔹TypeScript 使用 `Record<K, V>`<br/>🔹Python 使用 `dict[键类型, 值类型] (Python3.9+)` |

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

### 类型推导

### 类型谓词

### 索引类型

### 只读字段

### ts中的Brand/py中的newType





## 扩展

### mypy 的使用

#### 1. mypy 简介
> mypy是一个用于Python的静态类型检查器。它可以在不运行代码的情况下检查Python代码中的类型错误。mypy可以帮助开发者在编写代码时发现潜在的错误，从而提高代码的质量和可维护性。

#### 2. 安装mypy

**首先，确保你有一个 Python 项目，例如 main.py** 

**mypy 可以安装到以下两种环境中（任选其一）：**

> 1.  全局环境（所有项目共用）
>    * 适合频繁使用 mypy 的场景
>    * 可能与其他项目的依赖冲突
```bash
pip install mypy
```
> 2. **虚拟环境（推荐）**
>    * 隔离项目依赖，避免冲突
>    * 需要每个项目单独安装

```bash
# 创建虚拟环境(Python 3.3+ 内置)
python -m venv .venv

# 激活虚拟环境
# 1.Windows:
.venv\Scripts\activate
# 2.macOS/Linux:
source .venv/bin/activate

# 安装到虚拟环境
pip install mypy
```

#### 3. 验证安装

```bash
mypy --version
# 成功输出示例： mypy 1.15.0 (compiled: yes)
```

> 如下图所示：

<img src="E:\新建文件夹\images\mypy version.png" alt="mypy version" style="zoom:75%;" />

#### 4. 安装 mypy 插件

**在VSCode中，可以通过以下步骤安装mypy插件：**

> 1. 打开VSCode,点击左侧的扩展图标（Ctrl+Shift+X），在搜索框中输入“mypy”,然后点击安装按钮，重启VSCode生效。

<img src="E:\新建文件夹\images\Mypy Type Checker.png" alt="Mypy Type Checker" style="zoom: 50%;" />

#### 5. 配置 VSCode 与 mypy 集成

**1. 选择解释器：**

> 1. 在VSCode左下角打开设置，选择“命令面板”（Ctrl+Shift+P）

<img src="E:\新建文件夹\images\Select Interpreter.png" alt="Select Interpreter" style="zoom: 50%;" />

> 2. 输入并选择:
>
> ```plaintext
>Python: Select Interpreter
> ```
> 
> 3. 从列表选择你的虚拟环境路径：

<img src="E:\新建文件夹\images\select.png" alt="select" style="zoom: 67%;" />

> 4. 在根目录下创建 .vscode/settings.json 文件，添加以下配置:

```json
{
 "python.linting.mypyEnabled": true,
 "mypy.runUsingActiveInterpreter": true,
 "mypy.configFile": "pyproject.toml",  // 或 mypy.ini
 "python.defaultInterpreterPath": ".venv\\Scripts\\python.exe"  // 指向你的虚拟环境
}
```



> 💡 如果使用 `mypy.ini` 而非 `pyproject.toml`，需修改 `configFile` 路径。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               

#### 6. 编写Python代码并进行类型检查

> 1. 在 main.py 文件中编写 Python 代码。例如：

```Python
def greet(name: str) -> str:
    return f"Hello, {name}!"

def add_numbers(a: int, b: int) -> int:
    return a + b

if __name__ == "__main__":
    name = "Kimi"
    print(greet(name))

    # 故意引入类型错误
    result = add_numbers(5, "3")  # 错误：第二个参数应该是int类型
    print(result)
```

> 2. 在 VSCode 中，mypy 会自动对代码进行类型检查，并在问题面板中显示任何类型错误。

也可以打开 VSCode 的终端，手动运行 mypy 命令进行类型检查：

```bash
mypy main.py
```

> 如下图所示：

![](E:\新建文件夹\images\error.png)

> 3. 根据mypy的输出，修改代码以解决类型错误。
>
> 这个输出告诉我们，在调用 add_numbers 函数时，第二个参数的类型不匹配。它应该是一个整数( int )，但我们传递了一个字符串 ( str ) 。

> 修改代码：

```Python
# main.py

def greet(name: str) -> str:
    return f"Hello, {name}!"

def add_numbers(a: int, b: int) -> int:
    return a + b

if __name__ == "__main__":
    name = "Kimi"
    print(greet(name))

    # 修改后的代码，确保类型正确
    result = add_numbers(5, 3)
    print(result)
```

> 4. 再次运行 mypy 命令验证修正后的代码：

![success](E:\新建文件夹\images\success.png)

> 这表示代码中没有类型错误，mypy 已经通过了类型检查。