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

10. ​ 排序 **list.sort(reverse=True)** #按字母反向排序 , 临时排序  **list2 = list.sorted()**.

11. 长度  **len(list)**

12. **for** 循环  for 语句以：结尾， 下一行有缩进

    ```python
    dogs = [‘harmi’, ‘lucy’, ‘catty’]

    for dog in dogs:
    	pring('welcome' + dog)
    ```

    ​

13. 生成数字列表 **list(range(1,5))**,  # returns [1,2,3,4],**指定步长 list(range(5,10,2))**,# returns [5,7,9]

14. 列表统计计算 list = [1,2,3]   **min(list)** # returns 1,**max(list)** #returns 3,**sum(list)** #returns 6

15. **列表解析** a = [**value \** 2** for value in range(1,5)] #returns [1,4,9,16]

16. 列表**切片** list = [1,2,3,4,5]     **list[1:3]**    # returns [2,3],  **list[:3]**   # returns [1:2,3], **list[1:]** # returns [2,3,4,5] 

17. **复制**列表   list2 = list[:]

18. 定义**元祖**（不可改变其中元素的列表）  dimensions = **(1,2,3,4,5)**

19.  **条件判断**   **与**  **and** ,  **或**  **or** , **存在于** **in** , 例如： 1 in  (1,2,3)  # returns True，**不包含 not in**

20. **if语句**  if condition_test:

21. if []:   不会执行，空数组的布尔值等于false

22. **字典定义**  alien = {'color': 'red', num: 5},  **访问值**  alien['color'] # returns 'red' ,  **删除键值** del alien['red']

23.字典遍历
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

24. ​

