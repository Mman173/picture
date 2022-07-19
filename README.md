# kubernetes入门

[TOC]

## 配置准备

### 准备镜像

> **准备三台虚拟机：一台master和两台node**

![centos镜像配置](https://starlight173-1311655915.cos.ap-guangzhou.myqcloud.com/2022/07/18/8c1209ca0c424528943cda762f5bfef9.png)



> **注意双网卡配置使其可用**

![双网卡配置k8s镜像.png]( https://starlight173-1311655915.cos.ap-guangzhou.myqcloud.com/2022/07/18/d15901bc7e084688ac2bd560665b62df.png)

### 镜像配置

> **关闭swap分区：**
>
> **进入后注释最后一行即可**

```powershell
vi /etc/fstab
```

> **根据规划设置主机名**

```powershell
hostnamectl set-hostname <hostname>
```

>  **在master添加hosts：**
>
> **比如↓——按照实际情况更改**

```powershell
cat >> /etc/hosts << EOF
10.15.0.3 k8s-master1
10.15.0.4 k8s-node1
10.15.0.5 k8s-node2
EOF
```

> **将桥接的IPv4流量传递到iptables的链：**
>
> **防止IPv4流量丢包以及一些网络问题**

```powershell
cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl --system  # 生效
```

>  **时间同步**

```powershell
yum install ntpdate -y
ntpdate time.windows.com
```

