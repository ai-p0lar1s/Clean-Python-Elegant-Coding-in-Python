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
