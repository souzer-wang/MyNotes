# top 指令

举栗：

```shell
top -bn l -i -c

top - 14:19:51 up 138 days, 7:15, 1 user, load average: 0.20, 0.33, 0.39 
Tasks: 115 total, 1 running, 114 sleeping, 0 stopped, 0 zombie
Cpu(s): 4.5%us, 3.8%sy, 0.0%ni, 91.0%id, 0.6%wa, 0.0%hi, 0.0%si, 0.0%st
Mem: 1014660k total, 880512k used, 134148k free, 264904k buffers
Swap: 262140k total, 34788k used, 227352k free, 217144k cached
PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND 
12760 root 20 0 15084 1944 1632 R 2.0 0.2 0:00.01 top -bn 1 -i -c
```

top 指令可以看到总体的系统运行状态和 cpu 的使用率

%us: 表示用户空间程序的 cpu 使用率（没有通过 nice 调度）
%sy: 表示系统空间的 cpu 使用率，主要是内核程序
%ni: 表示用户空间且通过 nice 调度过的程序的 cpu 使用率
%id: 空闲 cpu
%wa: cpu 运行时在等待 io 的时间
%hi: cpu 处理硬中断的数量
%si: cpu 处理软中断的数量
%st: 被虚拟机偷走的 cpu