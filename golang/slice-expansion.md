# slice扩容机制

``` go
func printCap2() {
	s := []int{1, 2, 4}
	x := []int{4, 5, 6, 4}
	s = append(s, x...)
	fmt.Printf("len=%d, cap=%d\n", len(s), cap(s))
	s = append(s, []int{3, 2, 3, 4, 4, 5, 5, 5, 6, 6}...)
	fmt.Printf("len=%d, cap=%d\n", len(s), cap(s))
}
//输出 
//len=7, cap=8
//len=17, cap=18
```

``` go
func growslice(et *_type, old slice, cap int) slice {
    ......

	newcap := old.cap
	doublecap := newcap + newcap
	if cap > doublecap {
		newcap = cap
	} else {
		if old.cap < 1024 {
			newcap = doublecap
		} else {
			// Check 0 < newcap to detect overflow
			// and prevent an infinite loop.
			for 0 < newcap && newcap < cap {
				newcap += newcap / 4
			}
			// Set newcap to the requested cap when
			// the newcap calculation overflowed.
			if newcap <= 0 {
				newcap = cap
			}
		}
	}

    ......
    capmem = roundupsize(uintptr(newcap) * sys.PtrSize)   
}
```

``` go
func roundupsize(size uintptr) uintptr {
	if size < _MaxSmallSize {
		if size <= smallSizeMax-8 {
			return uintptr(class_to_size[size_to_class8[divRoundUp(size, smallSizeDiv)]])
		} else {
			return uintptr(class_to_size[size_to_class128[divRoundUp(size-smallSizeMax, largeSizeDiv)]])
		}
	}
	if size+_PageSize < size {
		return size
	}
	return alignUp(size, _PageSize)
}


```

https://jodezer.github.io/2017/05/golangSlice%E7%9A%84%E6%89%A9%E5%AE%B9%E8%A7%84%E5%88%99
