# 锁定分类

## 基础锁

互斥锁
自旋锁

## 高级锁

读写锁

公平锁

## 悲观锁 乐观锁

1. 悲观锁，即看待事情比较悲观，认为 多进程/多进程/多协程 同时修改共享资源的概率比较高，比较容易发生冲突，所以访问共享资源的时候会先上锁。上述互斥锁、自旋锁、读写锁属于悲观锁。
2. 乐观锁认为 多进程/多进程/多协程 同时修改共享资源的概率比较低，不容易发生冲突，它的工作方式是：先修改完共享资源，再验证这段时间内有没有发生冲突，如果没有其他进程在修改资源，那么操作完成；如果发现有其他进程程已经修改过这个资源，就放弃本次操作，类似git。全程是没有加锁操作，通过原始版本号来判断有没有发生冲突。
