#  前言

还是用开源软件Eclipse吧，穷人伤不起！

# 环境搭建

- JDK
- Eclipse
- 其他语言的编译器或解析器
- 其他语言的Eclipse插件

# 成员描述

- C：命令式，一步一步告诉计算机先做什么再做什么 
- SQL：声明式，告诉计算机应该做什么但不指定具体要怎么做 
- Lisp：函数式，将一切电脑运算视为代入参数式的函数计算 
- Python：对象式，一切皆对象，创建对象即可对数据选择操作

C和Lisp作为编程规范的理论来学习，Python是工作的主要语言，进行项目快速开发和当作语言胶水用，用Python做数据整理和画图，用SQL操作数据库，用Mathematica做数学计算，用Spark进行规模运算

- Mathematica
- Spark

------

# 1. Python操作Spark

*Spark环境的搭建*

- 准备

  - JDK
  - Python
  - Hadoop
  - Spark

- 安装

  参考*https://blog.csdn.net/hjxinkkl/article/details/57083549?winzoom=1*

  或见[/pics/在windows安装部署spark(python版) - CSDN博客.png](/pics/在windows安装部署spark(python版) - CSDN博客.png)

*一个例子*

- 代码

  ```python
  # -*- coding: utf-8 -*-
  from __future__ import print_function
  
  import os
  
  from pyspark import *
  print(os.environ['SPARK_HOME'])
  print(os.environ['HADOOP_HOME'])
  if __name__ == '__main__':
      sc = SparkContext("local[8]")
      rdd = sc.parallelize("hello PySpark world".split(" "))
      counts = rdd \
          .flatMap(lambda line: line) \
          .map(lambda word: (word, 1)) \
          .reduceByKey(lambda a, b: a + b) \
          .foreach(print)
      sc.stop
  
  ```

- 输出

![](/pics/Spark的一个例子.jpg)

# Python基础库的使用

*数据处理基础库*

- numpy：对Python数据类型list的拓展，可以得到n*m的矩阵
- pandas：基于numpy得到的数据表
- matplotlib：可定制性的画图软件

*领域应用库*

- scikit-learn：机器学习
- tensorflow：深度学习
- statsmodel：统计
- NTLK：自然语言

