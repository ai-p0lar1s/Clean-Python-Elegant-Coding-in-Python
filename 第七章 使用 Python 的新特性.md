# 第七章 使用 Python 的新特性

## 7.1 异步编程
* 在同步世界中，事情一次只发生一次。调用一个函数或操作时，你的程序只能等待它完成，然后继续做下一件事情。当函数完成其操作时，函数才能返回结果
* 在异步世界中，多个事情可以同时发生。当启动一个操作或调用一个函数时，程序将会继续运行，你可以执行其他操作或调用其他函数，而不只是等待异步函数执行完成。一旦异步函数完成了工作，程序就可以访问异步函数的执行结果
* 例如，假设你需要通过调用不同公司的股票 API 来获取不同公司的股票数据。同步代码会使程序浪费大量时间来等待响应

### 7.1.1 Python 中的 async
为了在 Python 中实现异步编程，Python 引入了两个主要组件：
* asyncio：允许 API 管理和运行协程的一个 Python 包
* async / await：Python 引入了两个新的关键字来处理异步代码，它们能够定义协程

### 7.1.2 asyncio 是如何工作的
暂略

### 7.1.3 异步生成器
暂略


## 7.2 类型标记
* 类型标注是动态语言世界的一个特点。在 typing 模块，Python 提供了可供使用的类型

### 7.2.1 Python 中的类型
* 在 Python 中，可以通过如下方式添加类型
    ``` python
    def is_key_present(data: dict, key: str) -> bool:
        return key in data
    ```
    Python 理解这种语法，但它不会进行验证。如果你传入的参数类型不正确是，Python 也不会报错。但是 IDE 可以识别这种类型，帮助开发人员静态分析

### 7.2.2 typing 模块
typing 模块提供了一些基本类型
1. Union
    * 如果事先不确定传递给函数的是什么类型，但是希望在有限的类型中选择一个类型，那么可以使用 `Union` 
    ``` python
    from typing import Union

    def find_user(user_id: Union[str, int]) -> None:
        if isinstance(user_id, int):
            user_id = str(user_id)
        return find_user_by_id(user_id)
    ```

2. Any
    * 它可以容纳所有其他的值和方法，当你不知道使用什么类型时，可以考虑使用 Any 类型
    ``` python
    from typing import Any

    def stream_data(sanitize: bool, data: Any) -> None:
        if sanitize:
            pass
        send_to_pipeline_for_processing(data)
    ```

3. Tuple
    * 顾名思义，这是元组的类型。唯一的区别是可以定义元组元素的类型
    ``` python
    from typing import Tuple

    def check_fraud_users(users_id: Tuple[int]) -> None:
        for user_id in users_id:
            try:
                check_fraud_by_id(user_id)
            exception FraudException as error:
                pass
    ```

4. TypeVar 和 Generic
    * 如果你希望定义自己的类型或重命名特定类型，那么可以使用 TypeVar 来实现
    * typing 模块提供了足够多的类型，大多数情况下可能不需要用到它
    ``` python
    from typing import TypeVar, Generic
    
    Employee = TypeVar("Employee")
    
    def get_employee_payment(emp: Generic[Employee]) -> None:
        pass
    ```

### 7.2.3 类型检查会影响性能吗
* 不会

### 7.2.4 类型标记如何帮助编写更好的代码
* 类型标记可以在将代码发送到生产环境之前进行静态代码分析以捕获类型错误，反之出现一些明显的 bug
* 类型标记可以让代码更整洁。在文档字符串中可以不注释变量类型，转而使用不需要任何性能成本的类型标记
* 使用 PyCharm 和 VSCode 之类的 IDE 时，typing 模块可以支持代码补全

### 7.2.5 typing 的陷阱
使用 typing 模块的时候，需要注意以下已知陷阱：
* 注释文档不完善
* 类型不严格
* 不支持第三方库

## 7.3 super() 方法
* 在子类中可以使用 `super()` 方法调用父类的方法（即继承）
    ``` python
    class PaidStudent(Student):
        def __init__(self):
            super().__init__(self)
    ```


## 7.4 类型提示
* typing 模块

## 7.5 使用 pathlib 处理路径
* 功能包括查找最后修改的文件、创建唯一的文件名、显示目录树、计数文件、移动和删除文件、获取文件的特定属性以及路径
    ``` python
    import pathlib

    path = pathlib.Path("error.txt")

    >>> path.resolve()
    PosixPath("home/python/error.txt")

    >>> path.resolve().parent == pathlib.Path.cwd()
    False
    ```

## 7.6 Print 现在是一个函数
* 旧版本： `print "hello"`
* 新版本： `print("hello")`

## 7.7 f-string
* 一种改进的字符串编写方法，比 `% format` 和 `format` 更具有可读性
    ``` python
    >>> num = 1
    >>> print(f"num: {num}")
    num: 1
    ```

## 7.8 关键字参数
* 使用 `*` 作为函数参数来强制使用关键字参数
    ``` python
    def create_report(user, *, file_type, location):
        pass

    create_report("skpl", file_type="txt", location="/user/skpl")
    ```

### 7.9 有序字典
* 以前必须使用 OrderDict 来实现字典按照插入顺序排序，现在默认字典就行

## 7.10 迭代解包
* 略
