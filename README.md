# VPS折腾总结

## 一、 登录
### 默认使用私钥直接登录，但通过以下配置改成`root`加密码登录。  

### 1. 使用密钥登录。RedHat/CentOS默认用户为`opc`，Ubuntu/Debian为`root`。
![](pic/1.png)  
### 点击连接后选择导入的密钥
![](pic/2.png)
### 2. 切换到`root`用户并修改初始密码
### 使用以下命令：
> `sudo su`  
> `passwd`  
### 3. 修改登录方式
### **更简单的方法：**
> ` sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config;`  
> ` sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config;`  
### **修改第一处：**
> ` vi /root/.ssh/authorized_keys`  
### 找到 `ssh-rsa`字样， 按键盘`i`进入编辑模式，将`ssh-rsa`前面的内容全部删除。
![](pic/3.png)
### 修改以后按`Esc`键退出编辑模式，`:wq`回车保存。
### **修改第二处：**
> ` vi /etc/ssh/sshd_config`  
### 找到`#PermitRootLogin`字样，去掉前面的`#`号，如下：
![](pic/4.png)
### 找到`PasswordAuthentication no`字样，把`no`改成`yes`如下:
![](pic/5.png)
### 4. 重启
> `reboot`
### 
## 二、 端口开放
### 1. 首先安装
> `iptables-persistent`  
> `apt-get install iptables-persistent`
### 2. 添加规则
> `iptables -P INPUT ACCEPT`  
> `iptables -P FORWARD ACCEPT`  
> `iptables -P OUTPUT ACCEPT`  
> `iptables -F`  
> `iptables-save`
### 3. 持久化
> `netfilter-persistent save`  
> `netfilter-persistent reload`
## 三、 证书申请
### 1. 拉取脚本并执行
> `apt update -y`  
> `apt install -y curl`  
> `apt install -y socat`  
> `curl https://get.acme.sh | sh`  
> `~/.acme.sh/acme.sh --register-account -m xxx@xxx.xxx`   
### 2. 申请证书  
> `~/.acme.sh/acme.sh --issue -d xxxx.xxxx.xxx --standalone`  
> `~/.acme.sh/acme.sh --installcert -d xxxx.xxxx.xxx --key-file /root/private.key --fullchain-file /root/cert.crt`
