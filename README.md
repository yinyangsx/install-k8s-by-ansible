# Kubernetes v1.25 企业级高可用集群自动部署
>### 注：确保所有节点系统时间一致
>### 操作系统要求：Ubuntu22.04

### 1、找一台服务器安装Ansible
```
# apt install ansible -y
```
### 2、下载所需文件

下载Ansible部署文件：

```
# git clone https://github.com/yinyangsx/install-k8s-by-ansible
# cd install-k8s-by-ansible
```

下载准备好软件包，解压到/root目录：

链接：https://pan.baidu.com/s/10zSmZWXfYH6Nz0s4McSIEg 
提取码：7ypa
```
# tar zxf binary_pkg.tar.gz
```
### 3、修改Ansible文件

修改hosts文件，根据规划修改对应IP和名称。

```
# vi hosts
...
```
修改group_vars/all.yml文件，修改软件包目录和证书可信任IP。

```
# vim group_vars/all.yml
software_dir: '/root/binary_pkg'
...
cert_hosts:
  k8s:
  etcd:
```
## 4、一键部署
### 4.1 架构图
单Master架构
![avatar](https://github.com/yinyangsx/install-k8s-by-ansible/blob/master/single-master.jpg)

多Master架构
![avatar](https://github.com/yinyangsx/install-k8s-by-ansible/blob/master/multi-master.jpg)
### 4.2 部署命令
单Master版：
```
# ansible-playbook -i hosts single-master-deploy.yml
```
多Master版：
```
# ansible-playbook -i hosts multi-master-deploy.yml
```

## 5、查看集群节点
```
# kubectl get node
NAME          STATUS   ROLES    AGE   VERSION
k8s-master1   Ready    <none>   9h    v1.25.13
k8s-node1     Ready    <none>   9h    v1.25.13
k8s-node2     Ready    <none>   9h    v1.25.13
```

## 6、其他
### 6.1 部署控制
如果安装某个阶段失败，可针对性测试.

例如：只运行部署插件
```
# ansible-playbook -i hosts single-master-deploy.yml -uroot -k --tags addons
```

### 6.2 节点扩容
1）修改hosts，添加新节点ip
```
# vi hosts
...
[newnode]
192.168.31.75 node_name=k8s-node3
```
2）执行部署
```
# ansible-playbook -i hosts add-node.yml
```
### 6.3 所有HTTPS证书存放路径
部署产生的证书都会存放到目录“install-k8s-by-ansible-master/ssl”，一定要保存好，后面还会用到~

<br/>
<br/>
