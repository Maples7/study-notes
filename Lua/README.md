# Lua

## Basics

- 这是一门以 1 作为 index 开头的与大多数语言都不同的神奇语言。

- 用 and 与 or 实现三元表达式：`A and B or C`。前提是 **B 不是 false 值**。

- 用取余运算实现浮点数保留几位小数位：`x - x % 0.01`，即会保留两位小数。
  - 用整除定义取余运算：`a % b == a - ((a // b) * b)`
  - `a % b` 取余运算的结果总是与 `b`  有相同的符号

- 16 进制数 `0x1p-1` 对应的十进制是：`(1 * 16^0) * 2^(-1) = 0.5`
  - `0xa.bp2 = (10 * 16^0 + 11 * 16^(-1)) * 2^2 = 42.75`
