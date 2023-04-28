---
title: 动手学数据分析 Task-01
tags:
  - python
abbrlink: edb8e65c 
date: 2021-09-13 19:55:24  
category: 数据分析
toc: true
---
## 第一章： 数据载入及初步观察
### **第一节** 
#### **载入数据**  

首先将数据集载入  
数据集来源 <https://www.kaggle.com/c/titanic/overview>

此时要用到python中的pandas来帮助读取数据。而常用的的两种引入数据的方式为：  
`read_csv` 和 `read_table`  
这两种方式有什么区别。执行以下代码来看一下
```python
import pandas as pd

def load_data():
    return pd.read_csv("train.csv"), pd.read_table("train.csv")

if __name__ == '__main__':
    csv, table = load_data()
    print(csv.head())
    print(table.head())
    print(table.head().shape)
```
分别用两种方式读取数据集，然后打印前五行<!--more-->
```   
   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
1            2         1       1  ...  71.2833   C85         C
2            3         1       3  ...   7.9250   NaN         S
3            4         1       1  ...  53.1000  C123         S
4            5         0       3  ...   8.0500   NaN         S

[5 rows x 12 columns]
  PassengerId,Survived,Pclass,Name,Sex,Age,SibSp,Parch,Ticket,Fare,Cabin,Embarked
0  1,0,3,"Braund, Mr. Owen Harris",male,22,1,0,A/...                             
1  2,1,1,"Cumings, Mrs. John Bradley (Florence Br...                             
2  3,1,3,"Heikkinen, Miss. Laina",female,26,0,0,S...                             
3  4,1,1,"Futrelle, Mrs. Jacques Heath (Lily May ...                             
4  5,0,3,"Allen, Mr. William Henry",male,35,0,0,3...                             
(5, 1)

Process finished with exit code 0
```
可以看到用 `read_csv` 读取的前五行是 5x12 的数组，说明是以每一个字符串作为存储单位来读取的。而 `read_table` 读取的前五行是一个 5x1 的数组，每一个元素之前以逗号分隔，说明是以每一行作为存储单位来读取的。如果想让两者的读取效果一样，可以在读取的时候选择用逗号分隔 `pd.read_table("train.csv", delimiter=",")` 

另外，如果载入数据的数据集非常大，可以利用chunk划分成数据块  
`data = pd.read_csv("train.csv", chunksize=100)`
其中chunksize的值代表每个数据块的行数，chunk的数据类型是dataframe。所以读取的时候遍历即可。下面是一个例子：  
```
for chunk in data:
  print(chunk)
```
输出结果为（一个chunk）：
```
PassengerId  Survived  Pclass  ...     Fare    Cabin  Embarked
0             1         0       3  ...   7.2500      NaN         S
1             2         1       1  ...  71.2833      C85         C
2             3         1       3  ...   7.9250      NaN         S
3             4         1       1  ...  53.1000     C123         S
4             5         0       3  ...   8.0500      NaN         S
..          ...       ...     ...  ...      ...      ...       ...
95           96         0       3  ...   8.0500      NaN         S
96           97         0       1  ...  34.6542       A5         C
97           98         1       1  ...  63.3583  D10 D12         C
98           99         1       2  ...  23.0000      NaN         S
99          100         0       2  ...  26.0000      NaN         S

[100 rows x 12 columns]
```

接着通过观察训练集可以知道，表头一共有12种。将表头转换成中文更有助于我们看清数据背后的联系。
> PassengerId => 乘客ID  
Survived    => 是否幸存   
Pclass      => 乘客等级(1/2/3等舱位)  
Name        => 乘客姓名  
Sex         => 性别                 
Age         => 年龄                 
SibSp       => 堂兄弟/妹个数  
Parch       => 父母与小孩个数  
Ticket      => 船票信息             
Fare        => 票价                
Cabin       => 客舱                
Embarked    => 登船港口  

如果要把表头换成中文并且将索引改为`乘客ID`。我们可以进行如下操作：  
首先先创建一个中文名称的array，然后利用`header`参数将表头去掉，之后把刚才创建好的中文表头“安装”上去。最后再用`index_col`参数指定`乘客ID`为索引。
```python
headers = ["乘客ID","是否幸存","舱位等级","姓名","性别","年龄","兄弟姐妹数量","父母小孩数量","船票信息","票价","客舱","登船港口"]
data2 = pd.read_csv("train.csv", names=headers,header=0,index_col="乘客ID")
```
************

#### **观察数据**  

查看数据基本信息的方法有
>  `.info()`  查看数据的基本信息  
>  `.head()`  查看头部的数据，可以指定行数  
>  `.tail()`  查看尾部的数据，可以指定行数  
>  `.shape()`  查看数据的形状  
>  `.describe()`  查看数据的一些数学/统计意义数据。例如 总数 平均数 方差  
> 
下面是是使用`.info()`得到的数据集的信息
```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 891 entries, 1 to 891
Data columns (total 11 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   是否幸存    891 non-null    int64  
 1   舱位等级    891 non-null    int64  
 2   姓名      891 non-null    object 
 3   性别      891 non-null    object 
 4   年龄      714 non-null    float64
 5   兄弟姐妹数量  891 non-null    int64  
 6   父母小孩数量  891 non-null    int64  
 7   船票信息    891 non-null    object 
 8   票价      891 non-null    float64
 9   客舱      204 non-null    object 
 10  登船港口    889 non-null    object 
dtypes: float64(2), int64(4), object(5)
memory usage: 83.5+ KB
None
```
观察发现一共有891个乘客信息。但是年龄这一列有714个非空数据，客舱有204个登船港口有889个，说明数据中存在很多空缺。统计缺失值的方法可以有：
>`isnull()`此函数返回的是dataframe中缺失值的情况，缺失为`true` 其余为`false`  
>`isnull().sum()` 来统计每一列中有多少空缺。  
>`isnull().any()` 如果某一列含有空缺则会在后面显示`true`

最后将中文表头的数据储存到新的文件里  
`data2.to_csv("train-chinese.csv",encoding='GBK')`  
注意因为我们将表头更改成中文，为了避免出现乱码所以需要指明编码格式

*****
### **第二节**
#### **数据类型**

之前提到过，通过**pandas**加载的数据的类型是`dataframe`。这是一种表格类型的数据结构，每一行每一列都有索引值。另一种常见的数据结构是`series`，它是一种类似数组的由索引值+数值组成的结构。一个小例子
```python
s1 = pd.Series(['a','b','c','d'])
s2 = pd.Series(['A','B','C','D'])
print(s1)
print(s2)
df1 = pd.DataFrame([s1],[s2])
print(df1)
```
```
0    a
1    b
2    c
3    d
dtype: object
0    A
1    B
2    C
3    D
dtype: object
   0  1  2  3
A  a  b  c  d
B  a  b  c  d
C  a  b  c  d
D  a  b  c  d
```

#### **查改数据**  

* 查看列的名称  
>`df.coloums`  

* 查看某列所有的值（`Cabin`）  
>`df.Cabin` 或者 `df['Cabin']`

* 删除某列 （`Age`）
>`del df.Age` 或者 `del df['Age']`

* 隐藏某列（`Name` , `Ticket`）
>`df.drop(coloums = ['Name','Ticket'])`
  

另外两个范围取值筛选的函数方法是`loc`和`iloc`  
**`loc`**  
通过行的索引的值来筛选数据  
**`iloc`**  
通过行号来选取数据  
```
        Columns1  Columns2  Columns3  Columns4
Index1  0.712295 -0.795627  0.335370 -0.471181
Index2  0.175748  0.995514 -0.605352  0.965604
Index3  1.748486  0.615215  0.494616 -1.241432
Index4  1.489864 -0.279662  1.057456  1.632853
```
用这个简单的数据当例子
```
data.loc['Index3','Columns1'] 
data.iloc[2,0]
```
的结果都是 **1.748486**  
```python
data.loc['Index2':'Index4', 'Columns1':'Columns2']
data.iloc[1:3,0:1]
```
的结果都是
```
Columns1
Index2	0.175748
Index3	1.748486
```

### 总结
本次学习的是python中非常强大的**pandas**和**numpy**两个库。由数据加载学习了**dataframe**这种数据结构以及其他相关的很多方法函数。还学习了对数据的各种操作：删除，隐藏，固定选取，筛选等。

****
**声明** ：以上学习内容根据DataWhale的[动手学数据分析课程](https://github.com/datawhalechina/hands-on-data-analysis)  
****
* 本文作者 ：王四喜  
* 版权声明 ：本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/)，转载请注明出处!  
* 本博客属个人所有，旨在整理和记录学习资料、心得体会，并方便日后查阅，不涉及任何商业目的。如文章不慎造成某方面侵权，实属抱歉，请联系我进行修改或删除。