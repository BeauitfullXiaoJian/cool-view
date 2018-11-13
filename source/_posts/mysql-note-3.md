---
title: MySQL 8 Chapter 11 数据类型
date: 2018-11-13 11:10:49
tags: mysql
---

### 11.2 数值类型

MySQL支持所有的标准SQL数值数据类型。包括精确的类型（INTEGER，SMALLINT，DECIMAL 和 NUMERIC）,以及近似值（FLOAT，REAL和DOUBLE PRECISION）。关键词INT是INTEGER同义词，关键字DEC和FIXED是DECIMAL的同义词。MySQL将DOUBLE视为DOUBLE PRECISION（非标准扩展）的同义词。 除非启用REAL_AS_FLOAT SQL模式，否则MySQL还将REAL视为DOUBLE PRECISION（非标准变体）的同义词。

BIT数据类型用于存储位值，并支持MyISAM，MEMORY，InnoDB和NDB表。 有关MySQL如何处理超出范围值到列和溢出的信息，请参见第11.2.6节“超出范围和溢出处理”。

有关数字类型存储要求的信息，请参见第11.8节“数据类型存储要求”。

用于计算数字操作数的结果的数据类型取决于操作数的类型和对它们执行的操作。 有关更多信息，请参见第12.6.1节“算术运算符”。

#### 11.2.1整数类型（精确值） - INTEGER，INT，SMALLINT，TINYINT，MEDIUMINT，BIGINT

MySQL支持SQL标准整数类型INTEGER（或INT）和SMALLINT。 作为标准的扩展，MySQL还支持整数类型TINYINT，MEDIUMINT和BIGINT。 下表显示了每种整数类型所需的存储和范围。

| 类型      | 存储（Bytes） | 最小值（有符号） | 最大值（有符号） | 最小值（无符号） | 最大值（无符号） |
|-----------|---------------|------------------|------------------|------------------|------------------|
| TINYINT   | 1             | -128             | 127              | 0                | 255              |
| SMALLINT  | 2             | -32768           | 32767            | 0                | 65535            |
| MEDIUMINT | 3             | -8388608         | 8388607          | 0                | 16777215         |
| INT       | 4             | -2147483648      | 2147483647       | 0                | 4294967295       |
| BIGINT    | 5             | -263             | 263-1            | 0                | 264-1            |

#### 11.2.2定点类型（精确值） - DECIMAL，NUMERIC

DECIMAL和NUMERIC类型存储精确的数字数据值。 在保持精确精度很重要时使用这些类型，例如使用货币数据。 在MySQL中，NUMERIC实现为DECIMAL，因此以下有关DECIMAL的备注同样适用于NUMERIC。
MySQL以二进制格式存储DECIMAL值。 请参见第12.23节“精确算术”。

在DECIMAL列声明中，可以（通常是）指定精度和标度; 对于
例：
```SQL
salary DECIMAL(5,2)
```
在这个例子中，5是精度，2是刻度。 精度表示为值存储的有效位数，刻度表示小数点后可存储的位数。

标准SQL要求DECIMAL(5,2)能够存储五位数和两位小数的任何值，因此可以存储在salary列中的值的范围是-999.99到999.99。

在标准SQL中，语法DECIMAL（M）等同于DECIMAL（M，0）。 类似地，语法DECIMAL等同于DECIMAL（M，0），其中允许实现决定M的值.MySQL支持这两种DECIMAL语法的变体形式。 M的默认值为10。

#### 11.2.3浮点类型（近似值） - FLOAT，DOUBLE

FLOAT和DOUBLE类型表示近似数值数据值。 MySQL对于单精度值使用四个字节，对于双精度值使用八个字节。