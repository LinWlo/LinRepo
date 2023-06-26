# linux基础
## 1 systemd
### 1.1 systemd启动过程
- systemd --> default.target --> graphical.target <- multi-user.target <- basic.target <- sysinit.target...
  - default.target是一个链接文件，指向默认target
  - /etc/systemd/system/default.target -> /usr/lib/systemd/system/graphical.target
  
### 1.2 单元（unit）
- 单元是systemd的基本配置单元，用于封装系统服务、监听socket、设备文件...
- `systemctl -t help`：查看单元类型
- `/usr/lib/systemd/system`：unit配置文件所在目录

### 1.3 systemV的运行级别和systemd的target
- `ls -l /usr/lib/systemd/system/runlevel?.target`
  - 0 poweroff.target：关机;
  - 1 rescue.target：单用户模式;
  - 2 multi-user.target：多用户模式，没有NFS;
  - 3 multi-user.target：完整的多用户模式；
  - 4 multi-user.target：未使用;
  - 5 graphical.target：图形界面;
  - 6 reboot.target：重启。

### 1.4 target
- 用于对单元进行分组，或在启动后到达的目标
- `ls /etc/systemd/system/multi-user.target.wants`：该目录下的所有服务会在multi-user.target这个默认target下启动
- `systemctl list-dependencies multi-user.target`：查看unit依赖关系

### 1.5 man bootup man systemd

### 1.6 常用命令
- `systemctl get-default`：默认target
- `systemd-analyze time`：开机启动时间
- `systemctl set-default graphical.target`：设置默认target
- `systemd-analyze blame `：每个unit启动所需的时间
- `systemd-analyze plot > boot.svg`：启动图表
- `file boot.svg`