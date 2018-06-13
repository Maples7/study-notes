# Python

## Basics

- 通过 `{}` 创建集合（Set）字面量，如 `a = {1, 2, 3, 3, 3, 2}` 创建了一个长度为 3 的集合

- 元组在创建时间和占用的空间上面都优于列表。

  可以使用 `sys` 模块的 `getsizeof` 函数来检查存储同样的元素的元组和列表各自占用了多少内存空间:

  ```shell
  ~ python
  Python 3.6.4 |Anaconda, Inc.| (default, Jan 16 2018, 12:04:33)
  [GCC 4.2.1 Compatible Clang 4.0.1 (tags/RELEASE_401/final)] on darwin
  Type "help", "copyright", "credits" or "license" for more information.
  >>> import sys
  >>> a = [1, 2, 3]
  >>> b = (1, 2, 3)
  >>> sys.getsizeof(a)
  88
  >>> sys.getsizeof(b)
  72
  ```

  可以在 **ipython** 中使用魔法指令 `%timeit` 来分析创建同样内容的元组和列表所花费的时间：

  ```shell
  ~ ipython
  Python 3.6.4 |Anaconda, Inc.| (default, Jan 16 2018, 12:04:33)
  Type 'copyright', 'credits' or 'license' for more information
  IPython 6.4.0 -- An enhanced Interactive Python. Type '?' for help.

  In [1]: %timeit [1, 2, 3, 4, 5]
  56.1 ns ± 0.543 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

  In [2]: %timeit (1, 2, 3, 4, 5)
  13.6 ns ± 0.697 ns per loop (mean ± std. dev. of 7 runs, 100000000 loops each)
  ```

## Decorator

- 使用 `functools.wraps` 来保留原始函数的 `__name__`、`__doc__` 以及参数列表等元信息

## 作用域

- Python 中有「局部作用域」、「嵌套作用域」（函数嵌套）、「全局作用域」和「内置作用域」（一些隐含标识符比如 `min`、`len`），变量查找时按以上顺序由内向外。

- `global` 关键字使得变量提升到全局作用域；`nonlocal` 使得变量提升到嵌套作用域。这两种都应该尽量避免使用，以降低代码块之前的耦合，防止全局变量的滥用。

## Generator

- 通过 `(x ** 2 for x in range(1, 1000))` 可以创建生成器对象，相对于通过 `[x for x in range(1, 10)]` 创建列表，可以更节省内存，因为生成器不占用存储数据的空间。

- 在 Python3 中，`filter`、`map` 等类似的算子返回的是生成器对象，所以即便不给它重新赋值也有可能在你逻辑上真正需要它的元素之前就已经把元素「抛」出了。

  举例如下：

  ```python
  data_times = filter(lambda x: x not in existing_data_times, data_times)
  logger.info(f'New Times: {list(data_times)}')
  # data_times has no elements now, `list()` has made it empty
  for data_time in data_times:
      # Never be reached here
      pass
  ```

  所以总是在这些算子执行完之后马上用 `list()` 将其转换成 `list` 是对于 Python 而言一个独特的良好的编码习惯。

  从上面的例子来说，可以改成：

  ```python
  data_times = list(filter(lambda x: x not in existing_data_times, data_times))
  logger.info(f'New Times: {data_times}')
  for data_time in data_times:
      pass
  ```

## Class

- 在类的定义中，以两个下划线开头的属性无法被外部访问，如 `self.__foo`。但是，Python 实际上并没有严格保证私有属性或方法的私密性，它只是给私有的属性和方法换了一个名字来「妨碍」对它们的访问，事实上如果你知道更换名字的规则仍然可以访问到它们。它的名字被改成了 `_<class_name>__foo`，但不建议这样访问。实际上，在 Python 中最好不要自定义私有属性，表示被保护或私有的属性时用一个下划线前缀来表示即可。开放比封闭更好，程序员也应当对自己的代码负责。

  此外，如果实在要访问私有属性，更好的办法是使用 `@property` 包装器来包装属性的 `getter` 和 `setter` 方法

- 在类中定义 `__slots__` 变量可以限定由该类产生的对象只能绑定特定的属性。`__slots__` 的限定只对当前类的对象生效，对子类并不起任何作用。

- Python 中除了使用 `NotImplementedError` 来达到类似其他语言抽象方法的效果外，更专业的做法是通过 `abc` 模块的 `ABCMeta` 元类和 `abstractmethod` 包装器来实现抽象类的效果：如果一个类中存在抽象方法那么这个类就不能够实例化（创建对象）。
