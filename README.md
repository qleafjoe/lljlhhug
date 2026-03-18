# Linux测试开发面试学习计划

> 适用于测试开发/SDET岗位面试准备

---

## 📋 目录
1. [基础概念](#一基础概念)
2. [常用命令](#二常用命令)
3. [文件系统与磁盘](#三文件系统与磁盘)
4. [进程与线程](#四进程与线程)
5. [网络基础](#五网络基础)
6. [Shell脚本](#六shell脚本)
7. [性能监控与调优](#七性能监控与调优)
8. [容器与虚拟化](#八容器与虚拟化)
9. [实战练习](#九实战练习)
10. [面试常考问题](#十面试常考问题)

---

## 一、基础概念

### 1.1 Linux简介
- Linux内核版本号含义（主版本.次版本.修订号）
- Linux发行版分类（Debian系、RedHat系、Arch系）
- 开源理念与GPL协议

### 1.2 Linux启动流程
```
BIOS/UEFI → Bootloader → Kernel → Init(systemd) → RunLevel
```
- 需掌握：BIOS vs UEFI、GRUB引导、systemd初始化

### 1.3 系统目录结构（重要！）
```
/bin       - 常用命令
/sbin      - 系统管理命令
/etc       - 配置文件
/home      - 用户主目录
/root      - root用户主目录
/var       - 可变数据（日志、缓存）
/tmp       - 临时文件
/usr       - 用户程序
/proc      - 虚拟文件系统（进程信息）
/sys       - 系统内核对象
/dev       - 设备文件
```

### 1.4 权限管理
- 文件权限：rwx（所有者/所属组/其他用户）
- 数字权限：755、644、777
- 特殊权限：SUID、SGID、Sticky Bit
- ACL访问控制列表

---

## 二、常用命令（高频！）

### 2.1 文件操作
| 命令 | 说明 | 常用参数 |
|------|------|----------|
| `ls` | 列出目录 | `-la`, `-lh`, `-lt` |
| `cd` | 切换目录 | `cd -`, `cd ~` |
| `pwd` | 显示当前目录 | - |
| `mkdir` | 创建目录 | `-p`（递归创建） |
| `rm` | 删除文件/目录 | `-rf`（强制递归） |
| `cp` | 复制 | `-r`, `-i`, `-v` |
| `mv` | 移动/重命名 | `-i`, `-f` |
| `touch` | 创建空文件 | - |
| `cat` | 查看文件内容 | `-n`, `-s` |
| `less/more` | 分页查看 | `空格键翻页` |
| `head/tail` | 查看首/尾行 | `-n`, `-f`（实时） |

### 2.2 文本处理
| 命令 | 说明 | 常用参数 |
|------|------|----------|
| `grep` | 文本搜索 | `-i`, `-v`, `-n`, `-r` |
| `sed` | 流编辑器 | `-i`, `s/old/new/g` |
| `awk` | 文本处理 | `-F`, `{print}` |
| `sort` | 排序 | `-n`, `-r`, `-k` |
| `uniq` | 去重 | `-c`, `-i` |
| `wc` | 统计 | `-l`, `-w`, `-c` |
| `find` | 文件查找 | `-name`, `-type`, `-exec` |
| `xargs` | 命令组合 | `-n`, `-i` |

### 2.3 系统管理
| 命令 | 说明 | 常用参数 |
|------|------|----------|
| `ps` | 进程查看 | `-aux`, `-ef` |
| `top/htop` | 实时监控 | - |
| `kill` | 终止进程 | `-9`, `-15` |
| `df` | 磁盘使用 | `-h`, `-T` |
| `du` | 目录大小 | `-sh`, `-h` |
| `free` | 内存使用 | `-h`, `-m` |
| `whoami` | 当前用户 | - |
| `sudo` | 提权执行 | - |

### 2.4 网络命令
| 命令 | 说明 | 常用参数 |
|------|------|----------|
| `ping` | 网络连通性 | `-c`, `-i` |
| `curl` | HTTP请求 | `-X`, `-H`, `-d` |
| `wget` | 下载 | `-O`, `-c` |
| `netstat` | 网络状态 | `-tuln`, `-an` |
| `ss` | socket统计 | `-tuln`, `-s` |
| `ifconfig/ip` | IP配置 | - |
| `ssh` | 远程连接 | `-i`, `-p` |
| `scp` | 远程复制 | `-r`, `-P` |
| `telnet` | 远程登录 | - |

---

## 三、文件系统与磁盘

### 3.1 文件系统类型
- ext4（主流）
- xfs（大文件、高并发）
- btrfs（快照、校验）
- ntfs/fat32（Windows兼容）

### 3.2 磁盘管理
- 分区工具：`fdisk`, `parted`, `gdisk`
- 格式化：`mkfs.ext4`, `mkfs.xfs`
- 挂载：`mount`, `umount`, `/etc/fstab`
- LVM逻辑卷管理

### 3.3 inode与软硬链接
- inode：文件的元数据存储
- 硬链接：同一inode的多个引用
- 软链接：类似快捷方式

### 3.4 磁盘IO
- IO调度算法：deadline、cfq、noop
- SSD优化：TRIM、NCQ
- dd命令测试IO性能

---

## 四、进程与线程（重点！）

### 4.1 进程概念
- 进程生命周期：创建、就绪、运行、阻塞、终止
- 进程状态：R（运行）、S（睡眠）、D（IO等待）、Z（僵尸）、T（停止）
- 进程优先级：nice值、PR值

### 4.2 进程管理命令
```bash
# 查看进程
ps -aux | grep nginx
top -p $(pgrep -d',' nginx)
pstree -p

# 进程树
pstree -ap

# 查看进程打开的文件
lsof -p pid
lsof -i :8080

# 查看进程树
ps -ef --forest
```

### 4.3 进程间通信（IPC）
- 管道（pipe）、命名管道（fifo）
- 信号（signal）
- 消息队列（message queue）
- 共享内存（shared memory）
- 信号量（semaphore）
- 套接字（socket）

### 4.4 线程
- 线程 vs 进程区别
- 多线程编程
- 线程同步：互斥锁、信号量、条件变量
- 线程查看：`top -H`, `ps -eLf`

### 4.5 僵尸进程与孤儿进程
- 僵尸进程：子进程退出但父进程未回收
- 孤儿进程：父进程退出，子进程被init收养

### 4.6 守护进程（Daemon）
- 特征：后台运行、无控制终端
- 创建方法：fork + setsid
- 查看：`ps -ax | grep daemon`

---

## 五、网络基础

### 5.1 TCP/IP协议栈
- 七层模型 vs 四层模型
- TCP三次握手、四次挥手
- TCP状态转换：ESTABLISHED、TIME_WAIT、CLOSE_WAIT
- UDP vs TCP区别

### 5.2 常用服务端口
```
20/21  - FTP
22      - SSH
23      - Telnet
25      - SMTP
53      - DNS
80      - HTTP
443     - HTTPS
3306    - MySQL
6379    - Redis
8080    - Tomcat/Web
```

### 5.3 网络配置
```bash
# IP配置
ifconfig eth0 192.168.1.100/24
ip addr add 192.168.1.100/24 dev eth0

# 路由
route -n
ip route show

# DNS
cat /etc/resolv.conf
```

### 5.4 网络诊断
- `ping`：连通性测试
- `traceroute`：路由追踪
- `nslookup/dig`：DNS查询
- `tcpdump`：抓包分析
- `netstat/ss`：端口监听

---

## 六、Shell脚本（必须掌握！）

### 6.1 基础语法
```bash
#!/bin/bash

# 变量
name="Alice"
echo "Hello, $name"

# 数组
arr=(1 2 3 4)
echo ${arr[0]}

# 条件判断
if [ $a -gt $b ]; then
    echo "a > b"
elif [ $a -eq $b ]; then
    echo "a = b"
else
    echo "a < b"
fi

# 循环
for i in {1..5}; do
    echo $i
done

while read line; do
    echo $line
done < file.txt
```

### 6.2 常用工具
```bash
# 字符串处理
${#str}           # 长度
${str:0:5}        # 截取
${str/old/new}    # 替换

# 命令替换
result=$(command)
date +%Y-%m-%d    # 日期格式化

# 算术运算
$((a + b))
expr $a + $b
```

### 6.3 正则表达式
- 元字符：`.`, `*`, `+`, `?`, `^`, `$`, `[]`, `()`
- 扩展正则：`egrep`
- 应用：`grep`, `sed`, `awk`

### 6.4 测开常用脚本场景
- 日志分析脚本
- 接口测试脚本
- 批量文件处理
- 自动部署脚本
- 性能测试数据收集

---

## 七、性能监控与调优

### 7.1 CPU监控
```bash
# 实时监控
top
htop
mpstat -P ALL 1

# CPU使用率分析
vmstat 1
sar -u 1
```

### 7.2 内存监控
```bash
free -h
vmstat 1
cat /proc/meminfo

# 内存泄漏检测
valgrind --leak-check=full
```

### 7.3 IO监控
```bash
iostat -x 1
iotop
df -h
du -sh /*
```

### 7.4 网络监控
```bash
# 网络流量
iftop
nethogs

# 连接状态
netstat -an | grep ESTABLISHED
ss -s
```

### 7.5 综合监控工具
- `glances`：一站式监控
- `dstat`：系统资源统计
- `prometheus + grafana`：企业级监控

---

## 八、容器与虚拟化

### 8.1 Docker基础
```bash
# 镜像操作
docker images
docker pull ubuntu:20.04
docker rmi image_id

# 容器操作
docker ps -a
docker run -it ubuntu /bin/bash
docker start/stop/restart container_id
docker exec -it container_id /bin/bash

# 日志与监控
docker logs -f container_id
docker stats container_id
```

### 8.2 Docker Compose
```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
```

### 8.3 Kubernetes基础
- Pod、Deployment、Service
- Namespace、Label、Selector
- 基本命令：`kubectl get`, `kubectl describe`, `kubectl logs`

---

## 九、实战练习

### 9.1 练习环境搭建
- 使用VirtualBox/VMware安装Linux虚拟机
- 使用WSL（Windows Subsystem for Linux）
- 使用Docker容器练习

### 9.2 必练项目
1. **搭建LAMP/LEMP环境**
2. **配置Nginx反向代理**
3. **编写自动化部署脚本**
4. **日志分析脚本开发**
5. **搭建Docker私有仓库**
6. **编写接口测试脚本**
7. **性能压测与监控**

### 9.3 在线练习资源
- [LeetCode算法题](https://leetcode.cn/)
- [Linux命令练习](https://linuxjourney.com/)
- [实验楼](https://www.shiyanlou.com/)

---

## 十、面试常考问题

### 10.1 基础概念类
1. Linux启动流程是怎样的？
2. 什么是inode？有什么作用？
3. 软链接和硬链接的区别？
4. Linux如何管理权限？
5. 什么是守护进程？

### 10.2 命令行类
1. 如何查看系统负载？
2. 如何查看端口占用？
3. 如何排查网络故障？
4. 如何查找大文件？
5. 如何统计日志中某个接口的访问量？

### 10.3 进程线程类
1. 进程和线程的区别？
2. 什么是僵尸进程？如何处理？
3. 进程间通信方式有哪些？
4. 如何查看进程的线程数？

### 10.4 网络类
1. TCP三次握手和四次挥手？
2. TIME_WAIT状态是什么？
3. HTTP和HTTPS的区别？
4. 如何查看网络连接状态？

### 10.5 Shell脚本类
1. 写一个脚本统计日志中ERROR出现的次数
2. 写一个脚本查找目录下最大的5个文件
3. 如何实现脚本定时任务？

---

## 📚 推荐学习资源

### 书籍
- 《鸟哥的Linux私房菜》（基础入门）
- 《Linux高性能服务器编程》
- 《Linux命令行与shell脚本编程大全》

### 在线教程
- [Linux Journey](https://linuxjourney.com/)
- [Ryan's Tutorials](https://ryanstutorials.net/)
- [Linux Command Library](https://linuxcommandlibrary.com/)

### 视频课程
- 极客时间《Linux性能优化实战》
- 极客时间《Linux系统调优》

---

## ⏰ 建议学习时间安排

| 阶段 | 内容 | 建议时间 |
|------|------|----------|
| 第1周 | 基础概念、目录结构 | 5天 |
| 第2周 | 常用命令、文本处理 | 7天 |
| 第3周 | 进程与线程 | 7天 |
| 第4周 | 网络基础 | 7天 |
| 第5周 | Shell脚本 | 7天 |
| 第6周 | 性能监控 | 5天 |
| 第7周 | 容器Docker | 5天 |
| 第8周 | 实战+面试题 | 7天 |

---

> 📝 更新说明：此文档将持续更新补充
>
> 🌐 欢迎提交PR：https://github.com/
