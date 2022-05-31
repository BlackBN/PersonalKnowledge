# 杂项

开源框架

```bash

https://github.com/golang/groupcache #groupcache是go语言版的memcached，目的是在某些特定场合替代memcached。

https://github.com/go-gorm/gorm # Go语言中使用比较广泛ORM框架
https://github.com/go-xorm/xorm 

```

缓存雪崩：
当缓存在同一时刻全部失效，导致DB瞬时请求量大、压力骤增，引起雪崩。通常因为缓存服务器宕机、缓存的key设置了相同的过期事件导致的。

缓存击穿
一个存在的key，在缓存过期的一刻，同时有大量的请求，这些请求都会击穿到DB，造成瞬时DB请求量大、压力骤增。

缓存穿透
查询一个不存在的数据，因为不存在则不会写到缓存中，所以每次都会去请求DB，如果瞬间流量过大，穿透到DB，导致宕机。

go数据库驱动: https://github.com/golang/go/wiki/SQLDrivers
