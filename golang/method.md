# 方法

``` go
type DefInt int
func(d DefInt) Print() {}
type DefFloat float64
func(d DefFloat) Print() {}
type DefString string
func (d DefString)Print() {}
type DefArray [30]DefMap
func (d DefArray) Print() {}
type DefChan chan struct{}
func (d DefChan) Send() {}
type DefSlice []DefChan
func (d DefSlice) Print() {}
type DefMap map[string]DefSlice
func (d DefMap) Print() {}
type DefStruct struct {
    DefArray
}
func (d DefStruct) Print() {}

// 不可以为指针类型、接口类型定义方法，compile error
// type DefInt *int
// func (d DefInt) Print() {}
// type DefInterface interface{}
// func (d DefInterface) Print() {}
```

## 结构体内嵌匿名类型

## 方法变量和方法表达式

## 封装
