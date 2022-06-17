# go协程

## goroutine 泄露

## 通道 channel

无缓冲通道
    未指定容量的channel:
        ch := make(chan int)
    用作同步
    当做管道
单向通道
    <-chan int 接受单向通道
    chan<- int 发送单向通道
缓冲通道
    指定了容量的channel:
        ch := make(chan in, 10)

多路复用器 select {
    case: 接受或者发送操作
    default: //可以做其他操作
}

channel 如果未初始化，零值为 nil
