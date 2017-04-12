
# Hadoop Multi Node Cluster Setup (Path-A) #  
  

Assuming you have basic knowledge of hadoop we are going ahead with the `cdh 5` installation on `google console`. Here I created 4 instances, 1 NameNode  and  3 DataNodes all having `RHEL 7` Linux Operating System and 60 GB Disk Space with 7 gb Ram.

# Creation of Instamce #  
1. Create an account at [Google Cloud Console](https://console.cloud.google.com)    
2. After Creating an account select compute engine from the left menu and then select Create Instance   
3. Create nodes by giving a node name.  
4. Then select the zone and machine type according to out requirement.   
5. Select your convenient operating system and check the boxes allow http and https and click on create.  
6. Similarly create the number of datanodes you want following the steps 3 to 5.  
7. After creating all nodes open all the nodes and start the configuration as below.  
  
## Configuration of Nodes ##  
### 1. Root Login and Password Setup ###  
Go to root by typing   
```vi
sudo su root	  
```
Then create and set a password by using   
```
passwd 
Enter password *********
Reenter password *******
```
  
Do the same for all the instances and give the same password in all the instances   
NOTE : all nodes must have the same password to create a passwordless ssh   
  

### 2. Disable Firewaall and Iptables: ###  
Go to root home using `cd ~` and check the firewall status and disable it.  
```vi
systemctl stop firewalld
chkconfig firewalld off
```
For Iptables:  
```vi
systemctl stop iptables && chkconfig iptables off
systemctl stop ip6tables && chkconfig ip6tables off
```
Do the same for all the nodes  
### 3.Disable SELINUX ###  
Open the file using   
```
vi /etc/selinux/config
```
change the attribute of SELINUX to disabled  
```
SELINUX=disabled
```
Do the same for all the nodes  
Then reboot all the nodes with  
```
 # reboot
 ```  
   
   
### 4. SSH Passwordless Keygen ###  
```vi
ssh-keygen -t rsa  
```
or
```vi
ssh -keygen 
```
Now that we generated ssh-keygen, we go to sshd_config and  do the following changes   
```vi
vi /etc/ssh/sshd_config
```
```vi
change permitrootlogin to yes 
passwordauthentication to yes
```
  
After setting above attributes we must Restart sshd
```vi
service sshd restart
```
  
Create a file named authorized_keys in namenode as well as datanode  
Copy public key which is in id_rsa.pub to authorized_keys  
```vi
cat id_rsa.pub > authorized_keys
```
  
Secure copy the authorized_keys to all other nodes  
```vi
scp authorized_keys datanode:/root/.ssh/test
```
  
Try  
```vi
Ssh datanode1
```  
  
Do this to all other nodes and you’ll be able to login to other nodes without any prompt for password  
   

  
### 5. Install web Server in all the servers: ###  
```vi
yum install -y httpd
service httpd start && chkconfig httpd on
```  
  
### 6. Got to netwroking/Firewall rules and create firewall rule. Make sure you add the below rule in networking/firewall_rules ###  

```vi
name: my_rule  
source_tag: 0.0.0.0/0   
allowed protocols/ports: tcp:0-65000   
network: default  
```    
  
### 7. CDH INSTALLATION ###  
CDH must be installed only in the namenode  
  
Install wget  
```vi
yum install wget
```
  
Get the bin file from cloudera  
```vi
wget http://archive.cloudera.com/cm5/installer/latest/cloudera-manager-installer.bin
```

The above command downloads the latest installer   
  
Change the permissions using   
```vi
chmod u+x cloudera-manager-installer.bin
```
  
After that run the following to install cloudera manager using local repo  
```vi
sudo ./cloudera-manager-installer.bin
```  
  
Cloudera Wizard will take you through the following steps if above all of the prerequisites are set properly  
Use the ip address of name node and type copy it in the browser search box with `:7180` extension  
```vi
<NameNode ip address>:7180
```  
  
    
![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/cdh.PNG)


#### 8.Login Screen ####  
  
![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/gvc.PNG)
  

![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/1.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/2.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/3.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/4.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/5.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/6.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/7.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/8.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/9.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/10.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/11.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/12.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/13.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/14.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/15.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/16.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/17.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/18.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/19.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/20.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/21.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/22.PNG)  


![GitHub Logo](https://github.com/arya990/Hadoop-Thirst/blob/master/cloudera/23.PNG)  

 


