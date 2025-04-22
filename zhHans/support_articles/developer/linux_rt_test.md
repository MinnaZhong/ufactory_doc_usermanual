# Raspberry Pi 5 与Intel i5 6300HQ 笔记本的实时性测试

实时性在伺服模式控制机械臂时至关重要。一般来说，UFACTORY 机械臂要求用户使用的操作系统在实时性测中，最大延时不超过 500 微秒。
由于Windows/Mac 系统无法满足实时性要求，不要在Windows/Mac 系统上使用制UFACTORY 机械臂的伺服模式。

推荐的方案是: 
* Raspberry Pi 5, Linux 系统, 无论是否使用实时补丁。
* 使用x86处理器的电脑，Linux 系统，且需要安装Linux 实时补丁。


测试指令
```
cyclictest -t1 -p 80 -i 1000 -l 100000 --mlockall
```

## 测试结果


### Raspberry Pi 5/Ubuntu 24.04, 无实时补丁

| Test Run | Min (us) | Act (us) | Avg (us) | Max (us) |
|----------|----------|----------|----------|----------|
| First    | 3        | 6        | 5        | 16       |
| Second   | 2        | 5        | 5        | 86       |
| Third    | 3        | 5        | 5        | 16       |



### Intel i5 6300HQ 笔记本/Ubuntu 24.04, 无实时补丁

| Test Run | Min (us) | Act (us) | Avg (us) | Max (us) |
|----------|----------|----------|----------|----------|
| First    | 2        | 3        | 3        | 96       |
| Second   | 3        | 3        | 3        | 296      |
| Third    | 2        | 3        | 3        | 3769     |


### Intel i5 6300HQ 笔记本/Ubuntu 24.04，有实时补丁
| Test Run | Min (us) | Act (us) | Avg (us) | Max (us) |
|----------|----------|----------|----------|----------|
| First    | 2        | 2        | 2        | 64       |
| Second   | 2        | 2        | 2        | 69       |
| Third    | 2        | 2        | 2        | 106      |


## 结论

Raspberry Pi 5(Llinux Ubuntu 24.04)，不使用实时补丁的情况下，也有较好的实时性。三次测试最大延时为86微秒，满足伺服模式控制机械臂的要求。
Intel Intel i5 6300HQ (Llinux Ubuntu 24.04) 无实时补丁的情况下，3次测试最大延时为3769微秒，不满足伺服模式控制机械臂的要求。使用实时补丁后，最大延时为106微秒，满足伺服模式控制机械臂的要求。