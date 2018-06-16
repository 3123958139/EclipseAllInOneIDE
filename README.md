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

Python是学习的主要语言，当作语言胶水用，用Python做数据整理，用SQL操作数据库，用Mathematica做数学计算和画图，用Spark进行规模运算

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





