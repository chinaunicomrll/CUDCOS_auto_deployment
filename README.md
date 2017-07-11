1.	安装ansible  
安装ansible的机器必须可以ssh到集群中节点  
rpm -iUvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm  
yum install -y ansible  

2.	设置key认证登录  
在安装ansible的机器上生成私钥  
ssh-keygen -t rsa -f /root/.ssh/id_rsa -q -P ""  
将 ~/.ssh/id_rsa.pub 的内容拷贝到要添加的节点上的 ~/.ssh/authorized_keys中
(如果没有新建)

3.	部署集群（只是添加节点则不用此步骤）  
下载dcos installer  
curl -O https://downloads.dcos.io/dcos/stable/dcos_generate_config.sh  
将下载完成的dcos_generate_config.sh文件拷贝到dcos_ansible的roles/deploy/files/ 目录下，将此目录下的ssh_key替换为部署集群所需的私钥（与上面ansible生成的id_rsa不同）


配置dcos_ansible的 group_vars/all 文件  
---  
network: enp0s8
clustername: dcos
sshuser: root
agent1: 192.168.56.103
master1: 192.168.56.106

其中network为节点的私网网卡名 clustername为集群名称  
sshuser为ssh的用户名 agent1,master1为部署集群时添加的集群节点

配置hosts文件  
[bootstrap]
192.168.56.104

[masters]
192.168.56.106

[agents]
192.168.56.103

[add]
192.168.56.107

分别配置以上集中角色的ip，将原有ip删去。bootstrap为部署集群的bootstrap节点，masters为master节点，agents为部署时的slave节点。add是后来添加的slave节点，部署时不需要配置。


配置完以上文件后 在dcos_ansible目录下 执行  
ansible-playbook -i hosts deploy.yml –verbose  
开始部署，可以看到ansible执行情况  

4.	添加节点  
若往已有集群添加节点 需要有部署集群时备份的dcos-install.tar文件。  
将dcos-install.tar文件复制到ansible机器上的  
/tmp/{{ clustername }}/dcos-install.tar下。  

配置/root/dcos_ansible-master-5/hosts文件  
将待添加的ip加入[add]下  
	
配置完以上文件后 在dcos_ansible目录下 执行  
ansible-playbook -i hosts add.yml --verbose  
开始部署，可以看到ansible执行情况  
