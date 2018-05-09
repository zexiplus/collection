#### usage

```shell
# 手动下载库函数
pip install pandas
```



#### Tip

1. 是索引-1返回列表最后一个元素，-2 返回倒数第二个...   \[1,2,3][-1]       # return 3

2.  ****** 表示乘方 ,      3 ** 2       # return 9

3.  命令行键入 **quit()** + 回车 退出命令行

4. 列表末尾增加元素 **list.append()**。

5. 插入 **list.insert(index, el)**。

6. 删除元素**del list[0]**。删除并弹出最后一个**list.pop()**。

7. 删除并弹出任一项**list.pop(index)** 。

8. 根据值删除元素**list.remove(value)**并使用，如果有重复只删除第一个。

9. 语句末尾**无**分号，注释用 **#**

10. 去除字符串右边空格（空行/换行符等） str.rstrip(), 去除左边空格 str.strip()

11. ​ 排序 **list.sort(reverse=True)** #按字母反向排序 , 临时排序  **list2 = list.sorted()**.

12. 长度  **len(list)**

13. **for** 循环  for 语句以：结尾， 下一行有缩进

    ```python
    dogs = [‘harmi’, ‘lucy’, ‘catty’]

    for dog in dogs:
    	pring('welcome' + dog)
    ```

    ​

14. 生成数字列表 **list(range(1,5))**,  # returns [1,2,3,4],**指定步长 list(range(5,10,2))**,# returns [5,7,9]

15. 列表统计计算 list = [1,2,3]   **min(list)** # returns 1,**max(list)** #returns 3,**sum(list)** #returns 6

16. **列表解析** a = [**value \** 2** for value in range(1,5)] #returns [1,4,9,16]

17. 列表**切片** list = [1,2,3,4,5]     **list[1:3]**    # returns [2,3],  **list[:3]**   # returns [1:2,3], **list[1:]** # returns [2,3,4,5] 

18. **复制**列表   list2 = list[:]

19. 定义**元祖**（不可改变其中元素的列表）  dimensions = **(1,2,3,4,5)**

20.  **条件判断**   **与**  **and** ,  **或**  **or** , **存在于** **in** , 例如： 1 in  (1,2,3)  # returns True，**不包含 not in**

21.  **等于 == ,大于等于 >= ,取非 not** 

22. **if语句**  if condition_test:

23. if []:   不会执行，空数组的布尔值等于false

24. **字典定义**  alien = {'color': 'red', num: 5},  **访问值**  alien['color'] # returns 'red' ,  **删除键值** del alien['red']

23.**字典遍历**
```python
# 遍历键值
for key,value in alien.items():
    print(key+':'+value)
    
# 遍历键
for key in alien.keys():
    print(key)
    
# 遍历值
for val in alien.values():
    print(val)
    
# 顺序遍历键
for key in sorted(alien.keys()):
    print(key)

# 遍历不重复的集合
for key in set(alien.values()):
    print(key)
    

```

24. **input()用户输入**  message = input('give some tips')  ， 输入的文字被视为字符串，格式化为数字   message=int(message)

25. **函数**

    ```python
    # 定义函数
    def fn():
        """此处为文档字符串，描述函数功能，以三个引号起始"""
        print('hello word')
        
    # 默认值函数
    def fn(a=1, b=2):
        print(a,b)
        
    # 传递任意数量的参数, *会创建一个args 的空元组，然后依次入参
    def fn(*args):
    	print(args)
    fn(1,2,3,4) # returns (1,2,3,4)

    # 传递任意数量的关键字形参 ，** 会创建一个空字典，并将收到的键值对装进这个空字典
    def fn(**dir):
        for key,val in dir:
            print(key,val)
    fn(a = 1, b = 2 )  # returns {'a': 1, 'b': 2}

    ```

26. **模块**

    ```python
    # 整体导入
    import module_name 
    # 调用模块的函数
    module_name.fn()

    # 导入特定的函数
    from module_name import fn
    fn()

    # as 给导入的函数起别名
    from module_name import function_name as fn

    # as 给导入的模块取别名
    import module_name as mn

    # 导入模块中的所有函数 , 引入之后元模块中所有的变量都可以直接使用
    from module_name import *

    ```

27. **类**

    ```python
    # 创建类
    class Dog():
        """模拟小狗"""
        def __init__(self, name, age):
            """初始化函数,每次实例化时会自动调用"""
            self.name = name,
            self.age = age
        def jump(self):
            """模拟小狗跳"""
            print('dog' + self.name + 'is jumping')

    # 实例化
    haski = Dog('haski', 2)

    # 继承类
    class Parent():
        """我是父类"""
        def __init__(self, name, age):
            self.name = name
            self.age = age
            
        def some_fn(self):
            print('i am ' + self.name):

    class Sub(Parent):
        def __init__(self, name, age):
            self.name = name
            self.age = age
    ```

28. **读写文件**

    ```python
    # 打开文件读取
    with open('some.txt') as file:
        contents = file.read()
        print(contents)

    # 逐行读取
    with open('some.txt') as file:
        for line in file:
            print(line)
            
    # 读取文件赋值给列表
    with open(some.txt) as file:
        lines = file.readLines()
    for line in lines:
        print(line.strip())

    # 打开文件模式 , 第二个参数 mode 为 a追加, w写入, r读取(默认), r+读写 
    open('some.txt', 'w')

    # 使用json存储数据 json.dump(data, json_obj)
    import json
    numbers = [1,2,3,4,5]
    with open('temp.json', 'w') as json_obj:
        json.dump(numbers, json_obj)
        
    # 读取json文件
    import json
    with open('temp.json', 'r') as json_obj:
        number = json.load(json_obj)
    print(number)

       
    ```

    ​


