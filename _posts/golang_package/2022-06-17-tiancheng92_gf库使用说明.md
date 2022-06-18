---
layout: article
title: tiancheng92/gf库使用说明
---

# 项目简介

* gf包 是一个容纳一些比较常用的函数的库，主要用于简化 Go 编程。
* 项目地址：[tiancheng92/gf](https://github.com/tiancheng92/gf)

<!--more-->

注意：部分方法仅可在Go版本大于等于1.18时使用！
{:.warning}

# 使用方法

```shell
go get -u github.com/tiancheng92/gf
```

# gf库方法详解

## 字符串相关

| 函数签名                                              | 描述                            |
|:--------------------------------------------------|-------------------------------|
| func StringJoin(variables ...string) string       | 字符串拼接（使用strings.Builder以提升性能） |
| func StringToBytes(s string) []byte               | 字符串转[]byte                    |
| func BytesToString(b []byte) string               | []byte转字符串                    |
| func StringCreateGzip(str string) ([]byte, error) | 对字符串进行gzip压缩                  |
| func StringParseGzip(data []byte) (string, error) | 对gzip压缩后的字符串进行解压              |

* Code：[string.go](https://github.com/tiancheng92/gf/blob/main/string.go)
* Test：[string_test.go](https://github.com/tiancheng92/gf/blob/main/string_test.go)
* Example：[example/main.go](https://github.com/tiancheng92/gf/blob/main/example/main.go)

## 浮点数相关

注意：以下方法均使用泛型实现，可以用于任意底层类型为float64或float32的类型
{:.warning}

| 函数签名                  | 描述                                          |
|:----------------------|---------------------------------------------|
| FloatRound[F ~float64 | ~float32](floatValue F, decimalCount int) F | 保留浮点数指定位数的小数 |

* Code：[float.go](https://github.com/tiancheng92/gf/blob/main/float.go)
* Test：[float_test.go](https://github.com/tiancheng92/gf/blob/main/float_test.go)
* Example：[example/main.go](https://github.com/tiancheng92/gf/blob/main/example/main.go)

## 切片相关

注意：以下方法均使用泛型实现，可以用于任意类型的数组。
{:.warning}

| 函数签名                                                                           | 描述                                  |
|:-------------------------------------------------------------------------------|-------------------------------------|
| func ArrayDeduplicate[T comparable](array []T) []T                             | 数组去重                                |
| func ArrayEqual[T any](array1, array2 []T) bool                                | 数组比较                                |
| func ArrayContains[T any](array []T, val T) bool                               | 查询数组是否包含指定元素                        |
| func ArrayIntersect[T comparable](array1, array2 []T) []T                      | 两个数组取交集                             |
| func ArrayUnion[T comparable](array1, array2 []T) []T                          | 两个数组取并集                             |
| func ArrayDifference[T comparable](array1, array2 []T) []T                     | 两个数组取差集                             |
| func ArrayFilter[T any](array []T, predicate func(T) bool) []T                 | 使用函数对数组进行过滤（过滤函数签名为：func(any) bool） |
| func ArrayReverse[T any](array []T) []T                                        | 数组反转                                |
| func ArrayBubbleSort[T any](array []T, less func(array []T, i, j int) bool)    | 数组进行冒泡排序（仅需实现less方法）                |
| func ArrayInsertionSort[T any](array []T, less func(array []T, i, j int) bool) | 数组进行插入排序（仅需实现less方法）                |
| func ArrayHeapSort[T any](array []T, less func(array []T, i, j int) bool)      | 数组进行堆排序（仅需实现less方法）                 |
| func ArrayQuickSort[T any](array []T, less func(array []T, i, j int) bool)     | 数组进行快速排序（仅需实现less方法）                |
| func ArraySort[T any](array []T, less func(array []T, i, j int) bool)          | 数组进行go原生排序（仅需实现less方法）              |

* Code：[array.go](https://github.com/tiancheng92/gf/blob/main/array.go)
* Test：[array_test.go](https://github.com/tiancheng92/gf/blob/main/array_test.go)
* Example：[example/main.go](https://github.com/tiancheng92/gf/blob/main/example/main.go)

## URL相关

| 函数签名                                          | 描述                                    |
|:----------------------------------------------|---------------------------------------|
| func UrlFormat(urlStr string) (string, error) | 将url格式化（会进行URL合规性检测、冗余字符删除、query参数排序） |

* Code：[url.go](https://github.com/tiancheng92/gf/blob/main/url.go)
* Test：[url_test.go](https://github.com/tiancheng92/gf/blob/main/url_test.go)
* Example：[example/main.go](https://github.com/tiancheng92/gf/blob/main/example/main.go)
