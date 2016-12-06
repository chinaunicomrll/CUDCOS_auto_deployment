Adding Agent Nodes
====
You can add agent nodes to an existing DC/OS cluster.<br> 
Agent nodes are designated as public or private during installation. By default, they are designated as private during GUI or CLI installation.<br>
Prerequisites:
-------


● DC/OS is installed using the custom installation method.<br>
● The archived DC/OS installer file (dcos-install.tar) from your installation.<br>
● Available agent nodes that satisfy the system requirements.<br>
● The CLI JSON processor jq.<br>
● SSH installed and configured. This is required for accessing nodes in the DC/OS cluster.<br>
Install DC/OS agent nodes:
-------
Copy the archived DC/OS installer file (dcos-install.tar) to the agent node. This archive is created during the GUI or CLI installation method.<br>
1.Copy the files to your agent node. For example, you can use Secure Copy (scp) to copy dcos-install.tar to your home directory:<br>
```Bash
$ scp ~/dcos-install.tar $username@$node-ip:~/dcos-install.tar #Bash
``` 
SSH to the machine:  
```Bash 
$ ssh $USER@$AGENT #Bash
``` 
