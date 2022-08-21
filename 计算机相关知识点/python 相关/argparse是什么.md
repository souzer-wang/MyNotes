# 定义

argparse是python内置的一个用于命令行选项和参数解析的模块，可以让人编写用户友好的命令行接口

- 通过定义我们需要的参数，argparse可以从sys.argv中解析出这些参数

- 还可以自动生成帮助和使用手册

- 可以在用户给程序传入无效参数的时候报出错误信息



具体有三个步骤，拿下面的代码举栗：

```python
import argparse

parser = argparse.ArgumentParser(description='test')

parser.add_argument('--sparse', action='store_true', default=False, help='GAT with sparse version or not.')
parser.add_argument('--seed', type=int, default=72, help='Random seed.')
parser.add_argument('--epochs', type=int, default=10000, help='Number of epochs to train.')

args = parser.parse_args()
print(args.sparse)
print(args.seed)
print(args.epochs)
```

其中，第一步是创建一个解析器，即创建ArgumentParser()对象

```python 
parser = argparse.ArgumentParser(description='test')
```

第二步是调用add_argument()方法添加参数

```python
parser.add_argument()
```

第三步是解析参数，使用parse_args()解析添加的参数

```python
args = parser.parse_args()
```
