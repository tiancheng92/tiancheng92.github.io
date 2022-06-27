---
layout: article
title: 性能测试之bytes与string转化
key: golang_benchmark_bytes_string_conversion
tags: [golang, benchmark]
sharing: true
show_subscribe: true
modify_date: 2020-06-27 12:57:12 +0800
---

## Golang Json解析性能对比
* 测试平台: M1 Macbook Pro
* golang版本: 1.18.3

<!--more-->

## 测试代码
```golang
package byte_string_conversion

import (
	"testing"
	"unsafe"
)

const text = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

func stringToByte(s string) []byte {
	return []byte(s)
}

func byteToString(b []byte) string {
	return string(b)
}

func unsafeStringToByte(s string) []byte {
	x := (*[2]uintptr)(unsafe.Pointer(&s))
	h := [3]uintptr{x[0], x[1], x[1]}
	return *(*[]byte)(unsafe.Pointer(&h))
}

func unsafeByteToString(b []byte) string {
	return *(*string)(unsafe.Pointer(&b))
}

func BenchmarkForByteToString(b *testing.B) {
	tmpByte := []byte(text)

	b.ResetTimer()
	for i := 0; i < b.N; i++ {
		res := byteToString(tmpByte)
		_ = res
	}
}

func BenchmarkForUnsafeByteToString(b *testing.B) {
	tmpByte := []byte(text)

	b.ResetTimer()
	for i := 0; i < b.N; i++ {
		res := unsafeByteToString(tmpByte)
		_ = res
	}
}

func BenchmarkForStringToByte(b *testing.B) {
	for i := 0; i < b.N; i++ {
		res := stringToByte(text)
		_ = res
	}
}

func BenchmarkForUnsafeStringToByte(b *testing.B) {
	for i := 0; i < b.N; i++ {
		res := unsafeStringToByte(text)
		_ = res
	}
}

```

## 测试结果
```bash
go test -bench=. -benchmem -run=none

goos: darwin
goarch: arm64
pkg: golang_benchmark/byte_string_conversion
BenchmarkForUnsafeByteToString-8        1000000000             0.3192 ns/op            0 B/op          0 allocs/op
BenchmarkForByteToString-8              18787170                68.14 ns/op          640 B/op          1 allocs/op
BenchmarkForUnsafeStringToByte-8        1000000000             0.3140 ns/op            0 B/op          0 allocs/op
BenchmarkForStringToByte-8              17581096                67.78 ns/op          640 B/op          1 allocs/op
PASS
ok      golang_benchmark/byte_string_conversion 3.815s

```

## 测试结论
* unsafe的转换性能远远优于默认的转换方式，但会丢失部分基础属性，请酌情使用。