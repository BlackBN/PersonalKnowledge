# golang 内存分配器

## 数据结构

mheap

arenas

```go

// mcentral
type mcentral struct {
	spanclass spanClass
	partial [2]spanSet // list of spans with a free object
	full    [2]spanSet // list of spans with no free objects
}

```

mcache
    alloc []*mspan

mspan
    spanClass
    npages

## 分配流程

微对象[0~16B]
tiny

小对象[16B~32KB]

大对象[32KB~...]

## 杂项
