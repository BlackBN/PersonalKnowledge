# iptables 知识点

不指定表，默认为filter表

iptables -nL --line-number 数字+列表+规则序号

iptables -F 清楚所有规则

iptables -X 删除用户自定义的链

iptables -Z 链的计数器清零

iptables -A INPUT 2 -t filter -p tcp --dport 22 -j DROP 

-A 添加到链的结尾，最后一条，后面加数字指定第几条

-I 添加到链的开头，第一条

-t 指定表

-m multiport

-p 指定协议 tcp udp all icmp

--dport 目的端口 80 或范围 80:4000

--sport 源端口 80 或范围 80:90

-s 原地址 ip 或 网段

-d 目的地址 ip 或 网段

-i 

-o

! 取反

-j 处理行为（ACCEPT接受 DROP丢弃 REJECT拒绝）

iptables -D INPUT 1 删除filter表INPUT链的第一条规则



![image-20210111163949400](/Users/bn/Library/Application Support/typora-user-images/image-20210111163949400.png)

![image-20210111164300542](/Users/bn/Library/Application Support/typora-user-images/image-20210111164300542.png)

![image-20210111164433675](/Users/bn/Library/Application Support/typora-user-images/image-20210111164433675.png)

