### 变量、常量和占位符

- 常量：在运行过程中值不会改变的单元，在tensorflow中无需进行初始化操作，创建语句如下：

```python
constant_name = tf.constant(value)
```

- 变量：在运行过程中值会发生变化的变量，需要在tensorflow中进行初始化操作，创建语句：

```python
name_variable = tf.Variable(value, name)
```

- 占位符：Tensorflow中的Variable变量类型，在定义时需要初始化，但是有些变量在定义时并不知道数值，在程序开始时才有外部输入，举个例子，参数为数据的Type和Shape的占位符PlaceHolder的函数接口如下：

```python
tf.placeholder(dtype, shape = None, name = None)
``
