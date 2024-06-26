# Ubuntu

## 虚拟机安装

参考：[VirtualBox 安装 Ubuntu20.04 图文教程](https://zhuanlan.zhihu.com/p/504251587)

> 注意：如果是AMD平台，可能需要在BIOS中启用AMD-V或SVM来启用硬件虚拟技术



## 虚拟机设置

- 网络-桥接网卡

- 共享粘贴板

  1. 系统-高级-共享粘贴板-双向

  2. 虚拟机安装 make 和 gcc

     ```sh
     sudo apt update
     sudo apt install make
     make -v
     sudo apt install gcc
     gcc -v
     ```

  3. 虚拟机窗口左上角-设备-安装增强功能（Virtual Guest Additions）

  4. 重启虚拟机



## 文件系统

- 进入某个目录：`cd xxx`
- 查看当前目录下文件：`ls`（追加 `-a` 参数可以查看隐藏文件）
- 查看当前目录下文件详细信息：`ll` 或 `ls -l` 
- 查看当前绝对路径：`pwd`
- 创建目录：`sudo mkdir xxx`
- 删除文件：`sudo rm [-f] xxx`（`-f` 表示强制删除）
- 递归删除文件夹：`sudo rm -rf xxx`
- 拷贝文件：`sudo cp <file-path> <target-path>`
- 拷贝文件夹：`sudo cp -r <file-path> <target-path>`
- 文件重命名：`sudo mv <原文件名> <新文件名>`
- 移动文件/重命名：`sudo mv <原文件路径> <目标文件路径>` （如果目标路径存在，则移动；如果目标路径不存在，则重命名）
- 全局查找某个文件：`sudo find / -name xxx`
- 建立软连接：`ln -s <原始文件或目录路径> <软连接名称或路径>`
- 查看当目录下各个文件/文件夹空间占用：`du -sh *`
- 压缩：`zip [-r] xxx.zip xxx`（`-r` 用于递归压缩文件夹）或  `tar -zcf xxx.gz xxx`
- 解压缩：`unzip xxx.zip` 或 `tar -zxf xxx.gz`
- 向另一台主机发送文件：`sudo scp [-r] <本机文件路径> <目标主机用户>@<目标主机ip>:<目标目录>` （`-r` 用于递归传输文件夹）
- 查看某个文件内容：`cat <文件路径>`
- 实时查看文件尾部内容（实时查看日志）：`tail -f <文件路径>`
- 查看文件头部前 n 行：`head -n 100 <文件路径>`
- 在某个文件中查找某个字符串：`grep <字符串> <文件名>`
- vim 打开文件后，查找字符串：非编辑模式下输入 `/pattern`  并回车，按 `n` 键跳转到下一个匹配项，按 `N` 键跳转到上一个匹配项（还可以通过 `:set hlsearch` 和 `:set nohlsearch` 来开启/关闭高亮）



## 软件管理

- 更新软件包列表：`sudo apt update`
- 下载软件：`sudo apt install xxx`
- 删除软件：`sudo apt remove xxx`
- 删除软件依赖残留：`sudo apt autoremove`
- 查看已安装软件：`apt list --installed`（通过 `| grep <软件名称>` 可以查找特定软件）



## 服务/进程管理

- 查看服务状态：`service xxx status`
- 启动服务：`sudo service xxx start`
- 停止服务：`sudo service xxx stop`
- 重启服务：`sudo service xxx restart`
- 设置服务开机自启动：`sudo systemctl enable xxx`
- 关闭服务开机自启动：`sudo systemctl disable xxx`
- 查看某个服务：`ps -ef | grep xxx`
- 根据pid查找进程：`ps -p <pid>` 或 `top -p <pid>`
- 强制杀死某个进程：`sudo kill -9 <pid>`
- 强制杀死相关进程：`sudo killall -9 xxx`
- 查看某个端口是否被占用：`sudo lsof -i:8080`
- 查看所有被占用的端口：`sudo ss -tuln`



## 用户权限

### 用户管理

创建新用户：

```sh
sudo adduser <新用户名>
```

修改用户密码：

```sh
sudo passwd xxx
```

命令行切换到某个用户：

```sh
sudo su xxx
```

### 查看文件访问权限

```sh
ls -l
# 或
ll
```

`-l` 参数可以查看访问权限，后面可以添加路径，默认当前路径

![file-auth](./docs/assets/file-auth.jpg)

第 1 列表示访问权限，其中：

第 1 位表示文件类型，`d` 表示目录，`-` 表示文件，`l` 表示文件链接；

后面 9 位分为 3 组，每 3 位为 1 组，第 1 组表示文件所有者权限（`u`），第 2 组表示所属组权限（`g`），第 3 组表示其他用户权限（`o`）；

`r` （read）表示读权限，`w`（write）表示写权限，`x`（execute）表示执行权限

### 修改用户访问权限

通过 `chmod [用户类型]操作模式[权限] 文件路径`

- 用户命令：`u`（所有者） | `g`（所属组） | `o`（其他用户） | `a`（所有用户）
- 操作模式：`+`（添加权限） | `-`（删除权限） | `=`（直接修改权限）
- 权限：`r`（读） | `w`（写） | `x`（执行） | `-`（无权限，适用于 `=` 操作模式）

例如：

```sh
sudo chmod a+rwx /home
```

表示为所有用户添加`/home` 目录的读、写和执行权限

```sh
sudo chmod o-w /home
```

表示删除其他用户对 `/home` 目录的写权限



### 修改文件所有者

```sh
sudo chown <新文件所有者>:<新文件组> <文件路径>
```



## 系统信息

| 信息     | 命令                |
| -------- | ------------------- |
| 操作系统 | `cat /proc/version` |
| CPU      | `lscpu`             |
| 内存     | `watch free -h`     |
| 硬盘     | `df -h`             |
| 系统占用 | `top`               |



## 硬盘管理

- 查看硬盘分区：

  ```sh
  sudo fdisk -l
  # 或
  lsblk
  ```

- 挂载硬盘（临时）：

  ```sh
  sudo mkdir /mnt/mydisk
  sudo mount /dev/sda1 /mnt/mydisk
  ```

- 取消挂载：

  ```sh
  sudo umount /mnt/mydisk
  ```

- 格式化硬盘（需要先取消挂载）：

  ```sh
  sudo mkfs.ext4 /dev/sda1
  ```

- 挂载硬盘（开机自动挂载）：

  ```sh
  sudo vim /etc/fstab
  ```

  在文件中添加以下内容：

  ```sh
  /dev/sdb1   /mnt/mydisk   ext4   defaults   0   0
  ```

  保存后验证：

  ```sh
  sudo mount -a
  ```

- LVM 查看卷组信息：

  ```sh
  sudo vgdisplay <卷组名称>
  sudo vgdisplay ubuntu-vg
  ```

  其中 `Free  PE / Size` 字段表示卷组空闲空间

- LVM 创建新的逻辑卷：

  ```sh
  sudo lvcreate -L <空间大小>G -n <新逻辑卷名称> <卷组名称>
  sudo lvcreate -L 100G -n new-lv ubuntu-vg 
  ```

  新建的逻辑卷（分区）为 `/dev/ubuntu-vg/new-lv`，格式化后就可以挂载了



## 网络

- 查看所有 IP 地址：`ip addr`

- 查看路由信息：

  ```sh
  ip route
  ```

  某个实例：

  ```sh
  default via 192.168.191.5 dev wlo1 proto dhcp metric 600 
  169.254.0.0/16 dev docker0 scope link metric 1000 linkdown 
  172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown 
  192.168.191.0/24 dev wlo1 proto kernel scope link src 192.168.191.105 metric 600 
  ```

  其中，以 `default` 开头的 `192.168.191.5` 表示网关地址；最后一行 `192.168.191.0/24` 表示局域网地址，后面的 `192.168.191.105` 表示本机 IP

- 使用 nmap 扫描局域网：

  ```sh
  sudo apt install nmap
  nmap -sn 192.168.1.0/24 # 192.168.1.0/24为局域网网络地址
  ```

- 设置内网 IP（方法一）：

  设置内网 IP 和子网掩码：

  ```sh
  # sudo ip addr add <IP地址>/<子网掩码> dev <网络名称>
  sudo ip addr add 192.168.4.93/24 dev enp2s0
  ```

  设置默认路由（网关）：

  ```sh
  sudo ip route | grep default # 查看网关地址
  # sudo ip route add default via <网关地址> dev <网络名称>
  sudo ip route add default via 192.168.4.1 dev enp2s0
  ```

  应用修改：

  ```sh
  sudo netplan apply
  ```

- 设置内网 IP（方法二）：

  Ubuntu17.10 以后，默认采用 netplan 管理，配置文件为 `/etc/netplan` 目录下的 `*.yaml`，不同系统版本的名称可能会有差异，以 Ubuntu20.04 为例，使用管理员权限打开：

  ```sh
  sudo vim /etc/netplan/01-network-manager-all.yaml
  ```

  ```sh
  network:
    ethernets:
      enp0s3:
        dhcp4: no # 是否动态分配IP
        addresses: [192.168.1.100/24] # IP地址和子网掩码
        gateway4: 192.168.1.1 # 网关地址
        nameservers:
          addresses: [8.8.8.8, 8.8.4.4] # DNS服务器地址
  ```

  - 找到 `network` 下的 `ethernets` 字段，没有则添加
  - `enp0s3`：网络名称，可以通过 `ip addr` 查看，通常是 `enp0s3`，如果是 VirtualBox 虚拟机，通常需要改成 `eth0`
  - `dhcp4`：是否使用 DHCP 协议，改成 `no` 取消动态分配 IP
  - `addresses`：IP 地址和子网掩码
  - `gateway4`：网关地址，通过 `ip route` 查询，以 `default` 开头的 IP 就是
  - `nameservers.addresses`：DNS 服务器地址，通过 `cat /etc/resolv.conf` 查询，以 `nameserver` 开头的 IP 就是

​	保存修改后，运行 `sudo netplan apply` 应用修改



## 环境变量

- 查看系统环境变量：`cat /etc/profile` 
- 查看用户环境变量：`export`
- 查看 `PATH` 环境变量：`echo $PATH` 
- 添加系统环境变量：使用管理员权限打开 `/etc/profile` 文件，在末尾添加 `PATH=$PATH:xxx`（`$PATH` 表示已有的环境变量，`:` 用于拼接变量，`xxx` 表示新变量的路径），保存后运行 `source /etc/profile` 更新配置
- 添加用户环境变量：打开 `~/.bashrc` 文件，在末尾添加 `PATH=$PATH:xxx`（`$PATH` 表示已有的环境变量，`:` 用于拼接变量，`xxx` 表示新变量的路径），保存后运行 `source ~/.bashrc` 更新配置



## NVIDIA 显卡驱动

```sh
sudo apt update
sudo apt install ubuntu-drivers-common
# 查看可用的驱动程序
ubuntu-drivers list
# 安装指定版本驱动
sudo apt install nvidia-driver-<version>
# 重启
sudo reboot
# 验证
nvidia-smi
```

> 注意：BIOS 需要禁用 Secure Boot，否则驱动可能无法加载



## CUDA

从 [NVIDIA官网](https://developer.nvidia.com/cuda-toolkit-archive) 下载对应版本的 CUDA runfile 并安装：

```sh
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
sudo sh cuda_11.8.0_520.61.05_linux.run
```

> 注意：安装 CUDA 时取消勾选 NVIDIA 驱动，上一步已经安装，如果直接通过 CUDA 安装驱动，可能与系统不兼容

然后设置环境变量：

```sh
vim ~/.bashrc
```

```sh
export PATH=/usr/local/cuda-11.8/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH ~/.bashrc
```

修改后执行以下命令生效：

```sh
source ~/.bashrc
```

验证：

```sh
nvcc --version
```



## SSH

想要其他机器访问本机，就需要配置 ssh 服务

> 注意：配置 SSH 服务前，建议先固定本机 IP，否则 IP 变化之后其他机器就无法通过之前的 IP 进行访问

- 安装：

```sh
sudo apt install openssh-server
```

- 检查状态：

```sh
systemctl status ssh
```



## 防火墙

- 查看防火墙状态

```sh
systemctl status firewalld
```

- 如果提示服务未找到，则需要先安装防火墙：

```sh
sudo apt-get install firewalld firewall-config
```

- 关闭防火墙

```sh
systemctl stop firewalld
```

- 重启防火墙

```sh
systemctl restart firewalld
```

- 查看防火墙开放端口

```sh
sudo firewall-cmd --zone=public --list-ports
```

- 开放指定端口

```sh
sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
sudo firewall-cmd --reload
```

`permanent` 参数表示长期生效。

开放端口后需要重新加载配置才能生效。



## git

- 安装

```sh
sudo apt install git
```

- 检查是否安装完成

```sh
git
```

- 设置用户名邮箱

```sh
git config --global user.name "username"
git config --global user.email "example@email.com"
# 然后查看是否配置完成：
git config user.name
git config user.email
```

- 配置 ssh key

```sh
ssh-keygen -t rsa -C "example@email.com"
```

打开公钥进行复制、并添加到 Github/Gitlab/gitee 的 ssh key 设置页面

```sh
cat xxx/.ssh/id_rsa.pub
```



## gitlab

- 下载 gitlab-ce（community-edition）

  ```sh
  curl -LO https://packages.gitlab.com/gitlab/gitlab-ce/packages/ubuntu/bionic/gitlab-ce_13.12.3-ce.0_amd64.deb/download.deb
  ```

- 安装

  ```sh
  sudo dpkg -i download.deb
  ```

- 修改配置文件

  ```sh
  vim /etc/gitlab/gitlab.rb
  ```

  ```diff
  - external_url 'http://gitlab.example.com'
  + external_url 'http://ip地址或域名'
  ```

- 配置

  ```sh
  sudo gitlab-ctl reconfigure
  ```

  第一次配置会自动安装对应的依赖

- 访问：`http://ip地址或域名`，其他机器访问注意 ssh 服务和防火墙设置

- 其他常用指令：

  ```sh
  sudo gitlab-ctl status
  sudo gitlab-ctl start
  sudo gitlab-ctl stop
  sudo gitlab-ctl restart
  ```




## Node

### 安装

首先从 [nodejs官网](https://nodejs.org/en/download) 下载对应版本的 node.js

然后解压缩：

```sh
tar -xf <文件名.tar.xz>
```

移动文件夹：

```sh
sudo mv node-xxx /usr/bin/node
```

设置环境变量：

```sh
sudo vim ~/.bashrc
```

在文件最后添加：

```sh
PATH=/usr/bin/node/bin:$PATH
```

保存后执行以下命令生效：

```sh
source ~/.bashrc
```

验证：

```sh
node -v
npm -v
```

安装 pnpm：

```sh
npm install -g pnpm
pnpm -v
```



### node 服务自启动

安装 pm2：

```sh
npm install -g pm2
```

启动某个服务：

```sh
pm2 start app.js [--name "app_name"]
```

创建 pm2 启动脚本：

```sh
pm2 startup
```

根据提示输入命令，例如：

```sh
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u your-username --hp /home/your-username/.pm2
```

最后保存当前 pm2 列表，保存的进程会开机自启动：

```sh
pm2 save
```

其他命令：

```sh
pm2 list
pm2 stop <名称>
pm2 delete <名称>
```



## Conda

从 [Conda官网](https://docs.conda.io/projects/conda/en/stable/user-guide/install/linux.html) 下载对应版本并安装。安装完成后，打开新的终端进行验证：

```sh
conda --version
```

常见命令：

```sh
# 查看所有环境
conda list -e
# 创建新的Python环境
conda create -n <环境名称> python=<python版本>
# 切换到某个环境
conda activate <环境名称>
# 删除某个环境
conda env remove -n <环境名称>
```



## Docker

```sh
sudo apt install curl
sudo curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

验证是否安装完成：

```sh
docker -v
```



## nginx

### 安装

```sh
sudo apt install nginx
```

检查是否安装完成

```sh
nginx -v
```

浏览器访问 `localhost`，可以看到 nginx 的默认欢迎页



### 常用命令

检查 nginx 配置文件是否正确

```sh
nginx -t
```

查看服务状态

```sh
service nginx status
```

关闭服务

```sh
sudo service nginx stop
```

重启服务

```sh
sudo service nginx restart
```

设置任务开机自启动

```sh
sudo systemctl enable nginx
```



### 配置

首先，定位 nginx 的安装目录，`nginx.conf` 就是 nginx 的配置文件

```sh
sudo find / -name nginx.conf
```

修改配置文件：

```sh
http {
	server {
		listen		80;
		server_name  localhost; # 如果需要外部访问，则改为本机ip
		
		location /vite-project {
			alias	/home/web/vite-project;
			index	index.html index.htm;
			try_files $uri $uri/ /index.html; # 设置文件查找规则，否则单页应用刷新/前进/后退会返回404页面
		}
	}
}
```

> 注意：修改 nginx 配置文件之后，需要重启 nginx 服务才能生效

> 注意：外部访问需要关闭防火墙或者开放对应端口

如果 `location` 设置了子路径，则前端项目需要设置公共基础路径和路由路径（以 vite+Vue3 项目为例）：

```typescript
// router/index.ts
import { createWebHistory } from 'vue-router';

// ...

const router = createRouter({
  routes,
  history: createWebHistory(import.meta.env.VITE_BASE_URL), // 设置路由基础路径
});
```

```typescript
// vite.config.ts
import { defineConfig, ConfigEnv, loadEnv } from 'vite';
import vue from '@vitejs/plugin-vue';

export default defineConfig(({ mode }: ConfigEnv) => {
  const env = loadEnv(mode, process.cwd());

  return {
    base: env.VITE_BASE_URL, // 设置公共基础路径
    plugins: [vue()],
  };
});
```

```sh
# .env.production
VITE_BASE_URL = /vite-project/
```

```sh
# .env.development
VITE_BASE_URL = /
```



## FFmpeg/ffplay

[FFmpeg](https://ffmpeg.org/download.html) 是一款强大的开源音视频编解码程序

- 播放视频：`ffplay <视频文件/视频流>`
- 拉取视频流，保存为视频文件：`ffmpeg -i <视频流> -c copy output.mp4`（`-c copy` 表示复制原始视频和音频，而不重新进行编码）
