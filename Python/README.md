# Python

## Basics

- 在 Python3 中，`filter`、`map` 等类似的算子返回的是迭代器对象，所以即便不给它重新赋值也有可能在你逻辑上真正需要它的元素之前就已经把元素「抛」出了。

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
