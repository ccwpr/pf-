|Sr.No|	Task |
|:-----:|:------:|
|1	|Install Centos 8 on Base machine|
|2	|Make a bridge connection on base machine| 
|3	|Download and install KVM Machine |
|4	|Download pfsense and then create a primary-pfsense machine on KVM Machine|
|5	|configure primary-pfsense|
|6	|create a pfsense client machine|
|7	|Go through pfsense Dashboard and block the website|
|8	|create seconpfsense | 
|9	|pfsense Documentation|


# Introduction
PFSENSE 


What is pfsense ?

pfSense is a free and open source firewall and router that also features unified threat management, load balancing, multi WAN, and more.

pfsense has many features and advanced capabilities that ensure it always follows either default or custom rules. It also filters traffic separately whether it's coming from your internal network of devices or the open internet, allowing you to set different rules and policies for each

First we have to do Installation of Centos 7/8 in Base Machine

>After installation we Have to make a Bridge Connection

Steps for make a Bridge connection :

[root@station27 ~]# yum install bridge-utils*

[root@station27 ~]# /etc/init.d/NetworkManager stop

[root@station27 ~]# chkconfig NetworkManager off

[root@station27 ~]# /etc/init.d/network restart

[root@station27 ~]# cd /etc/sysconfig/network-scripts/

[root@station27 ~]# vim ifcfg-br0

################################################################

DEVICE=br0

TYPE=Bridge

BOOTPROTO=static

GATEWAY=192.168.0.1

IPADDR=192.168.0.100

NETMASK=255.255.255.0

ONBOOT=yes

################################################################

[root@station27 ~]# vim ifcfg-eth0

##############################################################

DEVICE=eth0

HWADDR=00:1e:90:f3:f0:02

ONBOOT=yes

TYPE=Ethernet

IPV6INIT=no

USERCTL=no

BRIDGE=br0

##############################################################

[root@station27 ~]# vim /boot/grub/grub.conf

No Reboot The Xen Kernel

[root@station27 ~]# init 6

> Download and Install KVM Machine :

> Download pfsense 

![pfsense iso](https://user-images.githubusercontent.com/108870766/181179624-bc71c3a7-33a6-455e-88b8-531305e1f9b0.png)

 Click on start and then process as per Below
 
 Create a primary-pfsense on KVM machine
 
 click on Monitor logo for creation of new VM
 
 Choose how you would like to install the operating system : select on Local install media (ISO image or CDROM)
 
 ![kv1 1](https://user-images.githubusercontent.com/108870766/181465133-9774d4ac-0c56-4ca5-bcab-fc64bba0589c.png)
 
 Create a new virtual machine : Use ISO image and Click on browser 

![kv2 1](https://user-images.githubusercontent.com/108870766/181465429-6b5828fc-9f8a-4d20-94ce-7c1e1faa0cdb.png)

select ISO file 

![kv3 1](https://user-images.githubusercontent.com/108870766/181465517-4d03e62f-4237-4571-8d52-3e9ce87ecd05.png)

Choose Memory and CPU setting 

![kv4 1](https://user-images.githubusercontent.com/108870766/181465587-67a1403f-558f-4911-99ef-397160f9c698.png)

Click on Enable storage for this virtual machine 

![kv5 1](https://user-images.githubusercontent.com/108870766/181465656-a098b08f-097f-488e-8004-8527fd1eb30b.png)

Enter the Name : primary-pfsense 

![kv6 1](https://user-images.githubusercontent.com/108870766/181465725-77edbb35-df94-49bd-a750-e9243212dccd.png)


Network source : Bridge br0: Host device eno1

Device module : virtio 


![kv6a 1](https://user-images.githubusercontent.com/108870766/181465807-fad629a1-bb07-49c5-83d0-fa010863185f.png)

Click on Add hardware and then Click on Network 

Network source : Virtual network 'default' :NAT

MAC addres : it will be default

Device module : e1000 or according to you select anyone 

![kv7 1](https://user-images.githubusercontent.com/108870766/181465867-55821687-eeeb-4b5a-abbc-3f074f48fff3.png)

Once conpleted click on Finish and start your machine

![l1](https://user-images.githubusercontent.com/108870766/181205066-efb86c16-3706-4da0-a8f6-75dfec726c96.png)

Continue with Default keymap and then click on select

![l2](https://user-images.githubusercontent.com/108870766/181205225-aa16ce39-4f7f-4980-aa00-3c0338a910a1.png)

Here we are going to Select **Auto (UFS) BIOS**    **Guided Disk Setup using BIOS boot method** And then Click on **OK**

![l3](https://user-images.githubusercontent.com/108870766/181205298-d2186071-6d59-408d-adc7-345bc2804670.png)

Now we can see that it is extracting distributions file 

![l4](https://user-images.githubusercontent.com/108870766/181205347-dc66f65a-f52a-4997-b226-34d992b3a85f.png)

Here we are going to Select **no** and we will proceed further

![l5](https://user-images.githubusercontent.com/108870766/181205416-d6edaff0-0f02-4e82-8c8d-78de4add675d.png)

Here we all can see that we got a REBOOT option,

Wait, here we have to remove the ISO file which we have inserted in the beginning because if we are not removing it, it keeps on reebooting.

![l6](https://user-images.githubusercontent.com/108870766/181205482-185c67e9-ed81-44a7-a0a0-8bd73219bfc2.png)

Once ISO file is removed normally or forcefully then Click on REBOOT

**Now we are ready to use primary-pfsense machine**
 

## Access and configure pfSense Firewall

In the Below image we can see that it has already feth the ip for both LAN and WAN

![pfsense status ](https://user-images.githubusercontent.com/108870766/181241280-888f61db-5000-469e-a677-cfd70accf6d1.png)

 Click on 2) Set interface(s) IP address

 The script to set an interface IP address can set WAN, LAN, or OPT interface IP addresses
 
 Here first we are setting WAN interface 

Enter the Number of the interface you wish to configure :1 
Configure IPv4 address WAN interface via DHCP (y/n) n  (Because we have to set static ip )

 setup the WAN IP as per your requirement :

 
![set interface ](https://user-images.githubusercontent.com/108870766/181503795-d9f7aa27-7d15-4db1-ad53-69ab5b94baf0.png)

 we have to Assign the Gateway address for WAN, and then  IPv6 WAN interface via DHCP6 (y/n) we have to select n.
 
 ![rr2](https://user-images.githubusercontent.com/108870766/181506099-0e0cdaf2-5f7c-477a-b50a-b8c13c43a475.png)
 
 Press ENTER 
 Now proceed for LAN interface and assign the IP and also provide the Range of LAN IP
 
 
 ![r3](https://user-images.githubusercontent.com/108870766/181506399-ee620f0b-b828-4544-b8a2-0994818edb93.png)
 
 ![r4](https://user-images.githubusercontent.com/108870766/181506508-c8714ef0-2820-47df-a247-403f8768c4f7.png)
 
 


 




 
 











