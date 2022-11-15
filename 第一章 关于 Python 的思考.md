# 第一章 关于 Python 的思考
「Python 之禅」：注重简洁而不是复杂 


## 1.1 编写 Python 代码
[PEP8 规范](https://peps.python.org/pep-0008/)：定义了编写 Python 化代码的最佳实践


### 1.1.1 命名

#### 1. 变量和函数
* 使用小写字母命名变量和函数，并且用下划线分割单词
    ```
    names = "Python"
    job_title = "Software Engineeer"
    
    def get_data():
        pass
    ```
* 使用一个下划线为前缀命名私有变量（只是约定俗成，Pytyon 没有强制规定这样定义私有化）
    ```
    self._books = {}

    def _get_data(self):
        pass
    ```
* 使用两个下划线区分 Python 内置的变量、函数和方法
    ```
    __dict = []

    def __path():
        pass
    ```
* 使用具体（而不是模糊）的名称
    ```
    # wrong 
    def get_user_info(id):
        pass
    “”“
    get_user_info 模棱两可，参数 id 含义不明确（用户 ID，付款 ID，还是其他什么 ID？）
    ”“”
    
    # right
    def get_user_by(user_id):
        pass
    ```

#### 2. 类
* 类的命名像其他语言一样，采用大驼峰方法
    ```
    Class UserInformation:
        pass
    
    Class Player:
        pass
    ```

#### 3. 常量
* 全部使用大写字母来定义常量名
    ```
    TIMOUT = 6
    MAX_OVERFLOW = 7
    ```

#### 4. 函数和方法参数
* 遵循与变量和方法名相同的规则


### 1.1.2 代码中的表达式和语句
* 不要因为“巧妙”的编码方法，牺牲代码的可读性
    
    例：对嵌套字典进行排序
    ```
    users = [
        {"first_name": "Helen", "age": 39},
        {"first_name": "Buck", "age": 10},
        {"first_name": "anni", "age": 9},
    ]
    ```
    比较“巧妙”的做法是：
    ```
    users = sorted(users, key=lambda user: user["first_name"].lower())
    ```
    这样可以通过 lambda 节省代码行数，但它对于新手来说并不容易理解，且无法解决丢失键值或者字典合法性问题。一种更具可读性的方法是：
    ```
    def get_user_name(users):
    """Get name of the user in lower case"""
        return users["first_name"].lower()
    
    def get_sorted_dictionary(users):
    """Sort the nested dictionary"""
        if not isinstance(users, dict):
            raise ValueError("Not a correct dictionary")
        if not len(users):
            raise ValueError("Empty dictionary")
        users_by_name = sorted(users, key=get_user_name)
        return users_by_name
    ```
    是否要采用一行代码风格，取决于 **是否有利于代码的可读性**。

* 将复杂代码分解成辅助函数有助于提高可读性，也更便于调试

    例：读取 csv 文件
    ```
    import csv

    with open("employee.csv", "r") as csv_file:
        csv_reader = csv.DictReader(csv_file)
        line_count = 0
        for row in csv_reader:
            # process logic
            line_count += 1
        print(f'processed {line_count} lines.')
    ```
    上述代码在 with 语句里做了很多事情，为了使它更具可读性和便于调试，可以把处理逻辑放到一个不同的函数中。
    ```
    import csv

    with open("employee.csv", "r") as csv_file:
        csv_reader = csv.DictReader(csv_file)
        process_csv(csv_reader)
    
    def process_csv(csv_reader):
        line_count = 0
        for row in csv_reader:
            # process logic
            line_count += 1
        print(f'processed {line_count} lines.')
    ```
    同理，如果想要另外进行异常处理或者更复杂的操作，为了遵循 **单一职责原则**，可以进一步拆分函数。


### 1.1.3 拥抱 Python 编写代码的方式
#### 1. 更偏向使用 join 而不是内置的字符串连接符
* Python 字符串是不可更改的，使用 join 时，Python 对已经连接的字符串只分配一次内存，但是使用连接符「+」时，Python 不得不为每一次连接字符串分配新的内存。因此，在考虑代码性能时，尽量使用 join 方法，这样耗时更少。
    ```
    first_name = "Json"
    last_name = "smart"

    # Not recommended way
    full_name = first_name + " " + last_name

    # More performant way
    " ".join([first_name, last_name])
    ```

#### 2. 在判断是否为 None 时使用 is 和 is not
* 尽可能显式地编写代码，更好的可读性，更少地引发错误
    ```
    # wrong way
    if val:
        pass

    # right way
    if val is not None:
        pass
    ```


#### 3. 更偏向使用 is not 而不是 not ... is
* is not 可读性更好
    ```
    # wrong way
    if not val is None:
        pass
    
    # right way
    if val is not None:
        pass
    ```

#### 4. 绑定标识符时考虑使用函数而不是 lambda 表达式
* lambda 是 Python 实现一行代码的关键字，但有时候不是一个好的选择
    ```
    # wrong way
    square = lambda x: x * x

    # right way
    def square(val):
        return val * val
    ```
    def square(val) 函数对象比起泛型 lambda 更有助于字符串表示和回溯。考虑在较大的表达式中用 lambda。


#### 5. 与 return 语句保持一致
* 如果函数有返回值，确保在函数退出的所有地方都有返回表达式。返回空时，不要 `return` 而应 `return None`。

#### 6. 更偏向使用 "".startswith() 和 "".endswith()
* 比切片操作更具可读性和更整洁
    ```
    data = "Hello, how are you doing?"

    # wrong way
    if data[:5] == "Hello":
        pass

    # right way
    if data.startswith("Hello"):
        pass
    ```

#### 7. 比较类型时更偏向使用 isinstance() 方法而不是 type()
* isinstance() 能识别继承关系，而 type() 返回的是对象的直接类型

    ```
    user_ages = {"Larry": 35, "Jon": 89, "Imli": 12}

    # wrong way
    if type(user_ages) == dict:
        pass

    # right way
    if isinstance(user_ages, dict):
        pass
    ```

#### 8. 比较 Boolean 值的 Python 化方法
    ```
    is_empty = False

    # wrong way
    if is_empty == False:
        pass
    if is_empty is False:
        pass

    # right way
    if is_empty:
        pass
    ```


#### 9. 为上下文管理器编写显示代码
* 使用 with 语句编写代码时，考虑使用函数来处理不同于资源获取和释放的其他任何操作
    ```
    # right way
    class NewProtocol:
        def __init__(self, host, port):
            self.host = host
            self.port = port
        
        def __enter__(self):
            self._client = socket()
            self._client.connect((self.host, self.port))
        
        def __exit__(self, exception, value, traceback):
            self._client.close()
        
        def transer_data(self):
            pass
    
    with connection.NewProtocol(host, port):
        transfer_data()
    ```
    不要将除了打开和关闭连接的操作放到 \_\_enter\_\_ 和 \_\_exit\_\_ 函数中。

#### 10. 使用 Lint 工具来格式化代码
* 运行 Lint 工具有不同的方式：
    * 编码时使用 IDE
    * 在提交时使用预提交工具
    * 通过使用 Jenkins、CircleCI 等进行持续集成

## 1.2 使用文档字符串
* 规则：
    * 使用三引号编写文档字符串，即使只有一行。
    * 三引号中的字符串前后不应有任何空行
    * 文档字符串中的语句应该使用句点（.）作为结束符

* 四种不同的方式：
    *  谷歌
    ```
    """Calling given url.

    Parameters:
        url (str): url address to call.

    Returns:
        dict: Response of the url api.
    """
    ```
    *  Python 官方推荐
    ```
    """Calling given url.

    :param url: Url to call.
    :type url: str
    :returns: Response of the url api.
    :rtype: dict
    """
    ```
    *  NumPy / SciPy
    ```
    """Calling given url.

    Parameters
    ----------
    url : str
        URL to call.

    Returns
    -------
    dict
        Response of the url.
    """
    ```
    *  Egytext
    ```
    """Calling specific api.

    @type url: str
    @param file_loc: Call given url.
    @rtype: dict
    @returns: Response of the called api.
    """
    ```
    确保在整个项目中对文档字符串使用相同的风格。

### 1.2.1 模块级文档字符串
* 应该在文件的顶部（import 之前）放置一个模块级文档字符串来简要描述模块的用法。它应该关注模块的目标，包括模块里所有的方法和类，而不是描述一个特定的方法或者类。
    ```
    """
    This module contains all of the network related requests. This
    module will check for all the exceptions while making the 
    network calls and raise exceptions for any unknown exception.
    Make sure that when you use this module, you handle these
    exceptions in client code as:
    NetworkError exception for network calls.
    NetworkNotFound exception if network not found.
    """

    import urllib3
    import json
    ```
    编写模块级文档字符串时，需要：
    * 写一个简要的描述模块的目的
    * 可以增加异常信息，但是注意不要太详细
    * 考虑模块文档字符串作为提供模块描述信息的方式，而不是所有函数或者类的操作细节

### 1.2.2 使类文档字符串具有描述性
* 类文档字符串主要用于描述类的使用及其目标
    * 单行文档字符串
    ```
    class Student:
    """This class handle actions performed by a student."""

        def __init__(self):
            pass
    ```
    * 多行文档字符串
    ```
    class Student:
    """Stduent class information.

    This class handle actions performed by a student.
    This class provides information about student full name,
    age, roll-number and other information.

    Usage:
    import student

    student = student.Student()
    student.get_name()
    >>> 678998

    def __init__(slef):
        pass
    ```

### 1.2.3 函数文档字符串
* 函数文档字符串可以写在函数之后或函数之前，主要关注描述函数的操作。如果没有使用 Python 类型标注，也可以考虑包含参数。
    ```
    def is_prime_number(number):
        """Check for prime number.

        Check the given number is prime number or not by checking
        against all the numbers less the square root of given number.

        :param number: Given number to check for prime.
        :type number: int
        :return True if number is prime otherwise False.
        :rtype: boolean
        """
    ```

### 1.2.4 一些有用的文档字符串工具
* Sphinx
* Pycoo
* Read the docs
* Egydocs


## 1.3 编写 Python 的控制结构

### 1.3.1 使用列表推导
* Python 有多种方法可以从一个列表派生出另一个列表，如 map、filter 等。建议使用列表推导的方法，因为更具可读性。

    例 1：
    ```
    numbers = [10, 45, 34, 89, 34, 23, 6]

    # 使用 map 的版本
    square_numbers = map(lambda num: num ** 2, numbers)

    # 使用列表推导的版本
    square_numbers = [num ** 2 for num in numbers]
    ```
    例 2：
    ```
    data = [1, "A", 0, False, True]

    # 使用 filter 的版本
    filtered_data = filter(None, data)

    # 使用列表推导的版本
    filtered_data = [item for item in data if item]
    ```
    列表推导比 filter 和 map 版本可读性强的多，且开销较少。


### 1.3.2 不要使用复杂的列表推导
* 列表推导对于一个条件下最多两个循环时可行的，除此之外，反而会妨碍代码的可读性
    ```
    # 使用列表推导实现矩阵转置 -> 可行
    matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
    transposed = [[matrix[row][col] for row in range(3)] for col in range(3)]

    # 当条件较多时 -> 不建议
    ages = [1, 34, 5, 7, 3, 57, 356]
    old = [age for age in ages if age > 10 and age < 100 and age is not None
    ```


### 1.3.3 应该使用 lambda 吗
* 在 lambda 对表达式有帮助的地方使用 lambda，而不是用它替换函数的使用
    ```
    data = [[7], [3], [0], [8], [1], [4]]

    # wrong way
    min_val = min(data, key=lambda x:len(x))

    # right way
    def min_val(data):
    """find minimum value from the data list."""
        return min(data, key=lambda x:len(x))
    ```
    将 lambda 表达式作为函数编写会重复 def 的功能，这违反了 Python <u>**以一种且只有一种方式做事**</u> 的哲学


### 1.3.4 何时使用生成器与何时使用列表推导
* 生成器和列表推导的主要区别在于，列表推导将数据保存在内存中，而生成器不这样做
* 使用列表推导的情形：
    * 当需要多次遍历列表时
    * 当需要列出方法来处理生成器中不可用的数据时
    * 当没有大量的数据可以重复，并且你认为把数据保存在内存中不是问题时
* 一个使用生成器的例子：从文本文件中读取内容，且文件可能太大，一次性读取可能影响内存并使运行变慢
    ```
    def read_file(file_name):
    """Read the file line by line."""
        with open(file_name) as fread:
            for line in fread:
                yield line
    
    for line in read_file("logfile.txt"):
        print(line.startswith(">>"))
    ```

### 1.3.5 为什么不要在循环中使用 else
* 在 Python 中，for ... else 或 while ... else 结构只有当从循环中正常退出时，才会执行 else 子句，而中断关键字（break、raise 等）则不会执行 else子句。这样便于在循环结束后执行一个额外的操作，但是为了代码的可读性，还是避免使用它。
    ```
    # wrong way
    for item in [1, 2, 3]:
        if item % 2 = 0:
            break
        print("Then")
    else:
        print("Else")
    """
    >>> Then
    """

    # right way
    flag = True
    for item in [1, 2, 3]:
        if item % 2 = 0:
            flag = False
            break
        print("Then")
    if flag:
        print("Else")
    """
    >>> Then
    """
    ```

### 1.3.6 为什么 range 函数在 Python 3 中更好
* range 是一个不可变的迭代对象，只存储 start、stop 和 step 值，并根据需要计算值，对内存比较友好。
    ```
    # wrong way
    for item in [0, 1, 2, 3, 4]:
        print(item)
    
    # right way
    for item in range(5):
        print(item)
    ```


## 1.4 引发异常
在 Python 中，异常由内置模块处理
### 1.4.1 习惯引发异常
* 当代码失败时，在 Python 中更倾向于使用异常
    ```
    # wrong way
    def division(dividend, divisor):
    """Perform arithmetic division."""
        try:
            return dividend / divisor
        except ZeroDivisionError as zero:
            return None
    
    # right way
    def division(dividend, divisor):
    """Perform arithmetic division."""
        try:
            return dividend / divisor
        except ZeroDivisionError as zero:
            raise ZeroDivisionError("Please provide greater than 0 value")
    ```

### 1.4.2 使用 finally 来处理异常
* 在 Python 中，finally 块中的代码总是会被运行。在处理资源时，无论是否异常，都可以使用 finally 来确保关闭或释放文件或资源。
    ```
    def write_file(file_name):
    """Read given file line by line."""
        my_file = open("file_name", "w")
        try:
            my_file.write("Python is awesome")
        finally:
            my_file.close()
    ```

### 1.4.3 创建自己的异常类
* 需要确保自己创建的异常类有良好的边界并且是可描述的
    ```
    class UserNotFoundError(Exception):
    """Raise the exception when user not found."""
        def __init__(self, message=None, errors=None):
            super().__init__(self, message=None, erros=None):
            self.errors = errors
    ```

### 1.4.4 只处理特定的异常
* 捕获异常时，建议仅仅捕获特定异常，这有助于调试或诊断问题
    ```
    # wrong way
    try:
        get_even_list(numbers)
    except:
        print("Something is wrong")
    
    # right way
    try:
        get_even_list(numbers)
    except NoneType:
        print("None value has been provided.")
    except TypeError:
        print("Type error has been raised due to non sequential data type.")
    ```
    `except:` 或 `except Exception` 将处理所有的异常，会导致代码隐藏不想隐藏的关键错误或异常 

### 1.4.5 小心第三方的异常
* 在调用第三方 API 时，了解第三方库引发的所有异常非常重要。如果认为一个异常不太适合你的场景，可以考虑创建自己的异常类。

    例：
    ```
    from botocore.exceptions import ClientError

    ec2 = session.get_client('ec2', 'us-east-2')
    try:
        parsed = ec2.describe_instances(InstanceIds=['i_badid'])
    except ClientError as e:
        logger.error("Received error: %s", e, exc_info=True)
        if e.response['Error']['Code'] = 'InvalidInstanceID.NotFound':
            raise WrongInstanceIDError(message=exc_info, errors=e)
    
    class WrongInstanceIDError(Exception):
    """Raise the exception whenever Invalid instance found."""
        def __init__(self, message=None, errors=None):
            super().__init__(message)
            self.errors = errors
    ```
    这里考虑两件事：
    * 当在第三方库中发现特定错误时，添加日志会使调试第三方库中的问题更容易
    * 如果创建一个新的异常类会使代码更整洁、更具有可读性，那么可以创建一个新的类

### 1.4.6 try 最少的代码块
* 可以在一行上使用不同的异常
    ```
    def write_to_file(file_name, message):
    """write to file this specific message."""
        try:
            write_file = open(file_name, "w")
            write_file.write(message)
            write.close()
        except (FileNotFoundError, IOError) as exc:
            FileNotFoundException(f"Having issue while writing into file {exc}")
    ```
* 最好在 try 中编写最少代码
    ```
    # wrong way
    try:
        data = get_data_from_db(obj)
        return data
    except DBConnectionError:
        raise
    
    # right way
    try:
        data = get_data_from_db(obj)
    except DBConnectionError:
        raise
    return data
    ```
