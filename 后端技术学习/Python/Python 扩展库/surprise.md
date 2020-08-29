<!-- TOC -->

- [1. surprise](#1-surprise)
  - [1.1. 自动交叉检验法](#11-自动交叉检验法)
  - [1.2. Train_test split和fit()方法](#12-train_test-split和fit方法)
  - [1.3. 训练全部训练集和预测方法](#13-训练全部训练集和预测方法)
- [2. 使用自定义数据集](#2-使用自定义数据集)
  - [2.1. 使用交叉验证迭代器](#21-使用交叉验证迭代器)
  - [2.2. 使用GridSerachCV调整算法参数](#22-使用gridserachcv调整算法参数)
- [3. 参考](#3-参考)

<!-- /TOC -->
# 1. surprise
1. surprise是用来进行推荐系统的计算。

## 1.1. 自动交叉检验法
```py
from surprise import SVD
from surprise import Dataset
from surprise.model_selection import cross_validate
data = Dataset.load_builtin('ml-100k') #加载movielens-100k数据集
algo = SVD() #使用SVD算法
cross_validate(algo, data, measures=['RMSE', 'MAE'], cv=5, verbose=True) #采用5折交叉验证并打印结果
```
1. load_builtin()方法将提供下载ml-100k数据集，并且保存到相应位置。
2. 这里使用的是SVD算法，还有<a href = "https://surprise.readthedocs.io/en/stable/prediction_algorithms.html#prediction-algorithms">其他算法</a>可以使用
3. 其中的cross_validate()函数根据cv参数运行交叉验证程序。

## 1.2. Train_test split和fit()方法
1. 如果不想使用完全的交叉验证方法，可以用 `train_test_split()` 法来得到给定样本规模的测试集和训练集，并且可以自己选择精度的衡量方法 `accuracy metric`。训练的时候使用`fit()`方法作用于训练集，测试的时候使用`test()`方法，它将会返回作用于测试集上的预测结果。
```py
from surprise import SVD
from surprise import Dataset
from surprise import accuracy
from surprise.model_selection import train_test_split
data = Dataset.load_builtin('ml-100k') #加载movielens-100k数据集
trainset, testset = train_test_split(data, test_size=.25) #随机抽样选出训练集和测试集，这里选取了25%作为测试集
algo = SVD() #使用SVD算法
algo.fit(trainset) #做训练
predictions = algo.test(testset) #做测试
accuracy.rmse(predictions) #计算RMSE
```

## 1.3. 训练全部训练集和预测方法
1. 我们可以通过`bulid_full_trainset()`方法建立`trainset`对象来完成:
```py
from surprise import KNNBasic
from surprise import Dataset
data = Dataset.load_builtin('ml-100k') #加载movielens-100k数据集
trainset = data.build_full_trainset() #纠正/取出训练集
algo = KNNBasic() #建立算法并训练
algo.fit(trainset) 
```
2. 然后使用`predict()`方法来预测评分

# 2. 使用自定义数据集
1. surprise库有一组内建 数据集，但当然可以使用自定义数据集。加载评分数据集可以从文件（例如csv文件）或pandas数据框中完成。无论哪种方式，都需要定义一个Reader对象来解析文件或数据框。
2. 从csv文件中加载数据集，需要使用`load_from_file()`方法
```py
from surprise import BaselineOnly
from surprise import Dataset
from surprise import Reader
from surprise.model_selection import cross_validate
file_path = os.path.expanduser('~/.surprise_data/ml-100k/ml-100k/u.data')#数据集文件所在目录
reader = Reader(line_format='user item rating timestamp', sep='\t')
data = Dataset.load_from_file(file_path, reader=reader)
cross_validate(BaselineOnly(), data, verbose=True) #现在可以使用这个数据集，例如调用cross_validate 
```
3. 从pandas数据框加载数据集，需要使用 `load_from_df()`方法。这里不多赘述，可自己查看说明。

## 2.1. 使用交叉验证迭代器
1. 对于交叉验证，可以用 cross_validate() 完成所有的工作。但是为了更好地控制，可以实例化交叉验证迭代器，并使用且迭代器的 split() 方法和算法的 test() 方法，对每一折进行预测。
2. 下面是一个栗子，我们使用了一个经典的K-fold交叉验证程序，其中包含数据被分为3份（3折交叉验证）：
```py
from surprise import SVD
from surprise import Dataset
from surprise import accuracy
from surprise.model_selection import KFold
data = Dataset.load_builtin('ml-100k') #加载数据集
# define a cross-validation iterator
kf = KFold(n_splits=3) #定义交叉验证迭代器
algo = SVD()
for trainset, testset in kf.split(data):
    # 训练并测试算法
    algo.fit(trainset)
    predictions = algo.test(testset)
    # 计算并打印RMSE
    accuracy.rmse(predictions, verbose=True)
```
3. 也可以使用其他交叉验证迭代器，比如LeaveOneOut或ShuffleSplit。

## 2.2. 使用GridSerachCV调整算法参数
1. 该`cross_validate()`函数针对给定的一组交叉验证参数报告过程的准确性度量（如RMSE、MAE这些）。如果你想知道哪个参数组合能够产生最好的结果，那么这个 `GridSearchCV`类就可以解决问题。给定一个dict参数，这个类会尝试所有的参数组合，并报告任何准确性度量（对不同分割进行平均的）的最佳参数。它受到`scikit-learn`的`GridSearchCV`的启发。
2. 接下来这个例子我们尝试了SVD算法的参数 `n_epochs`, `lr_all` 和 `reg_all` 的不同值。
```py
from surprise import SVD
from surprise import Dataset
from surprise.model_selection import GridSearchCV
data = Dataset.load_builtin('ml-100k')
param_grid = {'n_epochs': [5, 10], 'lr_all': [0.002, 0.005],

              'reg_all': [0.4, 0.6]}
gs = GridSearchCV(SVD, param_grid, measures=['rmse', 'mae'], cv=3)
gs.fit(data)
# best RMSE score
print(gs.best_score['rmse'])
# combination of parameters that gave the best RMSE score
print(gs.best_params['rmse'])
```
3. 上面是用来评估3倍交叉验证过程的平均RMSE和MAE，但可以使用任何交叉验证迭代器
4. 一旦`fit()`被调用，`best_estimator`这个属性给了我们一个算法实例最优的一组参数，可以根据我们的喜好使用它：
```py
# 可以使用产生最优RMSE的算法
algo = gs.best_estimator['rmse']
algo.fit(data.build_full_trainset())
```
5. 需要注意:例如`bsl_options`与`sim_options`需要特殊对待。
```py
param_grid = {'k': [10, 20],
              'sim_options': {'name': ['msd', 'cosine'],
                              'min_support': [1, 5],
                              'user_based': [False]}
              }
```
6. 两者可以结合使用:
```py
param_grid = {'bsl_options': {'method': ['als', 'sgd'],
                              'reg': [1, 2]},
              'k': [2, 3],
              'sim_options': {'name': ['msd', 'cosine'],
                              'min_support': [1, 5],
                              'user_based': [False]}
              }
```
7. 进一步分析，cv_results属性具有所有需要的信息，并且可以在pandas数据框中导入
```py
results_df = pd.DataFrame.from_dict(gs.cv_results)
```

# 3. 参考
1. <a href = "https://blog.csdn.net/yuxeaotao/article/details/79851576">surprise库使用（一）——使用内置数据集</a>
2. <a herf = "https://blog.csdn.net/yuxeaotao/article/details/79852254">surprise库使用（二）——使用自定义数据集</a>