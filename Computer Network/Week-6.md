# TCP Core Functions

1. 进程通信之间，**复用**
2. 可靠传输
	* 回退N（currently selected）
	* 选择性重传
1. 拥塞控制
2. 流控制
# TCP 拥塞控制

拥塞主要发生在网络的核心（路由器）

**AIMD**（加性增，乘性减）:
* 慢开始
* 拥塞避免
* 快重传
* 快恢复

**CUBIC**(`Linux 默认`)

**Delayed-based** 

TCP是否公平?多个connections同时进行，吞吐量的变化
# QUIC Protocol (UDP-based)


