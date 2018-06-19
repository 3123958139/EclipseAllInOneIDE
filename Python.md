# 记录1

*实现*

- 从tushare下载个股日线数据

~~~python
import datetime
import matplotlib.pyplot as plt
import tushare as ts

# rushare的数据接口get_k_data
df = ts.get_k_data("002237")
~~~

- 进行描述统计

~~~python
# 头5行、后5行
print(df.head(6), "\n", df.tail(5))
~~~

> date  open  close   high   low    volume    code
> 0  2015-10-28  9.87  10.00  10.43  9.76  301307.0  002237
> 1  2015-10-29  9.88   9.81   9.98  9.68  172770.0  002237
> 2  2015-10-30  9.62   9.61   9.74  9.54  127858.0  002237
> 3  2015-11-02  9.50   9.37   9.60  9.36   88001.0  002237
> 4  2015-11-03  9.38   9.39   9.54  9.30   95569.0  002237
> 5  2015-11-04  9.33   9.73   9.76  9.33  159696.0  002237 
> date   open  close   high    low    volume    code
> 636  2018-06-12  11.52  11.53  11.66  11.28  349401.0  002237
> 637  2018-06-13  11.31  11.58  11.75  11.28  414907.0  002237
> 638  2018-06-14  11.49  11.51  11.66  11.30  371703.0  002237
> 639  2018-06-15  11.47  10.37  11.54  10.36  613449.0  002237
> 640  2018-06-19  10.03   9.83  10.37   9.77  322026.0  002237

~~~python
# 每一列的数据格式
print(df.dtypes)
~~~

> date       object
> open      float64
> close     float64
> high      float64
> low       float64
> volume    float64
> code       object
> dtype: object

date是object，说明是string格式，需转换成datetime格式

~~~python
# 基本统计量
print(df.describe())
~~~

> open       close        high         low        volume
> count  641.000000  641.000000  641.000000  641.000000  6.410000e+02
> mean    11.151076   11.192028   11.366412   10.992434  2.486659e+05
> std      1.188738    1.204152    1.220832    1.183558  2.219213e+05
> min      7.450000    7.500000    7.810000    7.410000  3.422400e+04
> 25%     10.310000   10.350000   10.450000   10.160000  9.329400e+04
> 50%     11.060000   11.120000   11.280000   10.910000  1.629700e+05
> 75%     11.760000   11.770000   11.920000   11.600000  3.249060e+05
> max     15.350000   15.650000   15.800000   14.900000  1.377287e+06

可以大致观察一下数据情况

~~~python
# 检查缺失值
print(df.isnull().any())
# df[df.isnull().values==True] # 若有缺失值可以这样定位到缺失值的行列位置
~~~

> date      False
> open      False
> close     False
> high      False
> low       False
> volume    False
> code      False
> dtype: bool

说明每一列都没有缺失值，若有缺失值可以使用**df.dropna()**或**df.replace()**进行处理

~~~python
# 检查异常值
dfnum = df.ix[:, 1:5]  # 选择数值列OHLC，V由于量级不一样并且这里不是主要考察的目标，省去
dfnum.boxplot() # 箱线图
dfnum.hist() # 直方图
plt.show()
~~~

> ![](/pics/python_boxplot1.png)
>
> ![](/pics/python_hist1.png)

这里图示比较粗糙，结果不可信，因为股价是时间序列，应该差分后再检查异常点

~~~python
dfnumdiff = dfnum.diff(1)  # 一阶差分
dfnumstd = dfnumdiff.dropna()  # 因为经过差分后第一个数据为Nan,须去掉
dfnumstd.apply(lambda x: preprocessing.scale(x))  # 标准化
dfnumstd.boxplot()
dfnumstd.hist()
dfnumstd.plot()
plt.show()
~~~

> ![](/pics/python_boxplot2.png)
>
> ![](/pics/python_hist2.png)
>
> ![](/pics/python_diff2.png)

明显的异方差，需分段检查异常点或根据具体需要选择检查方法

- 日期进行整理

~~~python
df['date'] = df['date'].map(lambda x: datetime.datetime.strptime(x, "%Y-%m-%d"))
print(df.dtypes)
~~~

> date      datetime64[ns]
> open             float64
> close            float64
> high             float64
> low              float64
> volume           float64
> code              object
> dtype: object

可见日期由object转为datetime64

- 画图

~~~python
df.set_index('date', inplace=True)
dfplt = df.ix[:, 0:4]

if __name__ == "__main__":
    dfplt.plot()
    plt.show()
~~~

> ![](/pics/python_plot3.png)
