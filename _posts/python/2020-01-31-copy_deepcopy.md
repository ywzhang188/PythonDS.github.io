---
layout: post
title: 彻底理解copy和deepcopy的区别
category: python
tags: [python]
no-post-nav: true
excerpt: python
---
开年第一篇博客，数据科学搞起来！
这篇文章想换个角度了解下copy和deepcopy的区别是什么，应用于List以及Ndarray的copy和deepcopy的案例整理了一下，希望能使思路更加清晰。


**应用于List的Copy**

关于一层List的Copy
```
a = [1,2,3]
b = a.copy()
a[1] = 999
print('a:',a) #打印结果 a: [1, 999, 3] 改变
print('b:',b) #打印结果 b: [1, 2, 3] 不受影响
```
关于嵌套List的Copy，修改嵌套内的值
```
a = [1,2,[1,2]]
b = a.copy()
a[2][1] = 999
print('a:',a) #打印结果 a: [1, 2, [1, 999]] 改变
print('b:',b) #打印结果 b: [1, 2, [1, 999]] 改变
```
关于嵌套List的Copy，修改嵌套外的值
```
a = [1,2,[1,2]]
b = a.copy()
a[1] = 999
print('a:',a) #打印结果 a: [1, 999, [1, 2]] 改变
print('b:',b) #打印结果 b: [1, 2, [1, 2]] 不受影响
```
证明在list中进行copy时，只有当列表嵌套时，修改嵌套内的值，一变都变；其他情况都不受影响

**应用于List的Deepcopy**

关于一层List的Deepcopy
```
import copy
a = [1,2,3]
b = copy.deepcopy(a)
a[1] = 999
print('a:',a) #打印结果 a: [1, 999, 3] 改变
print('b:',b) #打印结果 b: [1, 2, 3] 不受影响
```
关于嵌套List的Deepcopy，修改嵌套内的值
```
import copy
a = [1,2,[1,2]]
b = copy.deepcopy(a)
a[2][1] = 999
print('a:',a) #打印结果 a: [1, 2, [1, 999]] 改变
print('b:',b) #打印结果 b: [1, 2, [1, 2]] 不受影响
```
关于嵌套List的Deepcopy，修改嵌套外的值
```
import copy
a = [1,2,[1,2]]
b = copy.deepcopy(a)
a[1] = 999
print('a:',a) #打印结果 a: [1, 999, [1, 2]] 改变
print('b:',b) #打印结果 b: [1, 2, [1, 2]] 不受影响
```
证明在list中进行deepcopy时，无论是否有嵌套list，deepcopy后都不受影响，完全无关

**应用于Ndarray的Copy**

关于一层Ndarray的Copy
```
a = np.arange(5)
b = a.copy()
a[1] = 999
a #返回结果 array([  0, 999,   2,   3,   4]) 改变
b #返回结果 array([0, 1, 2, 3, 4]) 不受影响
```
关于多维Ndarray的Copy
```
a = np.array([[0,1,2],[3,4,5]])
b = a.copy()
a[0,1] = 999
a 
#返回结果 array([[  0, 999,   2],
#              [  3,   4,   5]])改变
b 
#返回结果 array([[0, 1, 2],
#              [3, 4, 5]])不受影响
```
证明在Ndarray中进行copy时，无论是否有多层array，copy后都不受影响，完全无关

**应用于Ndarray的Deepcopy**

关于一层Ndarray的Deepcopy
```
import copy
a = np.arange(5)
b = copy.deepcopy(a)
a[1] = 999
a #返回结果 array([  0, 999,   2,   3,   4]) 改变
b #返回结果 array([0, 1, 2, 3, 4]) 不受影响
```
关于多维Ndarray的Deepcopy
```
import copy
a = np.array([[0,1,2],[3,4,5]])
b = copy.deepcopy(a)
a[0,1] = 999
a 
#返回结果 array([[  0, 999,   2],
#              [  3,   4,   5]]) 改变
b 
#返回结果 array([[0, 1, 2],
#              [3, 4, 5]]) 不受影响
```
证明在Ndarray中进行deepcopy时，无论是否有多层array，deepcopy后都不受影响，完全无关

总结：
Deepcopy属于深复制(deep copy)，也就是复制之后，两者完全不相关，互相不影响。
Copy属于浅复制(shallow copy)，在嵌套List应用copy方法时，只有嵌套内的值会收到影响，其余互不影响。


