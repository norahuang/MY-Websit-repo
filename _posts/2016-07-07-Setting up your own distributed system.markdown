---
layout: post
title:  "Setting up your own distributed system"
date:   2016-07-07 12:03:30 -0700
categories: Technology
---

Setting up your own distributed system
I use virtual machines to setup my own distributed system.
Components of this example are listed as below. You can choose other components as
to your preference.
1. Host OS: MAC OS
2. Virtualization tool: Virtualbox, it seems to be the most suitable one for MAC, free and
easy to use, if your host is linux or windows, you can have other choices, such as
VMware.
3. Virtual machine OS(guest OS): ubuntu
Steps:
1. Download and install Virtualbox from <a href="https://www.virtualbox.org/">virtualbox</a> 
2. Create several VMs with the default options. (I have master, worker1, worker2)
3. download ubuntu iso for VMs, ubuntu iso can be found in <a href="http://www.ubuntu.com/">ubuntu</a> 
download
4. Start the VMs and install ubuntu on each VM
5. apt install on VMs (when you need to access outside network use NAT network
option in settings->network, otherwise host-only adapter is good for internal
communication between VMs and VM-host.)
1) sudo apt install golang-go
2) sudo apt install nfs-common
3) sudo apt install ssh
4) sudo apt install nfs-kernel-server
6. create a new disk on master
1) power off master vm
2) settings->storage->+->create a sata control disk
3) power on master vm
4) config the disk
- Run
sudo fdisk /dev/sdb
Then type o and press enter # creates a new table
Then type n and press enter # creates a new partition
Then type p and press enter # makes a primary partition.
Then type 1 and press enter # creates it as the 1st partition
(use default setting in this step)
Finally type w # this will write any changes to disk.
Okay now you have a partition, now you need a filesystem.
- Run
sudo mkfs.ext4 /dev/sdb1
- add below line into /etc/fstab
/dev/sdb1 /home/yourname/GO ext4 defaults 0 1
- create a floder /home/yourname/GO on your master
mkdir /home/yourname/GO
- restart master
7. share the folder /home/yourname/GO on master
1) add below line in /etc/export
/home/yourname/GO ipaddress of your workers(rw)
2) sudo exports -r
8. mount the folder on master to the workers, on workers, execute
mkdir /home/yourname/GO
mount -t nfs ipaddressOfmaster:/home/yourname/GO /home/yourname/GO
9. set the GOPATH on all vms, you can add the line below to ~/.bashrc
export GOPATH=/home/yourname/GO
open a new terminal
11. Put your mapreduce code on to one of your vm’s /home/yourname/GO/src as well
as the kjv12.txt file, organize them in below structure will make things easier
- /home/yourname/GO/src
- kjv12.txt
- wc.go
- /home/yourname/GO/src/mapreduce
- common.go
- worker.go
- master.go
- mapreduce.go
12. change the code of you mapreduce using tcp rpc instead of unix one.
13. on master
go run wc.go master kjv12.txt ipaddressOfmaster:7777
14. on workers
go run wc.go worker ipaddressOfmaster:7777 ipaddressOfthisWorker:portyoulike
Then you can see the result….

<p>When I decided to have my own website, I do a little research about all website platform providers. After filtering, there are three for me: <a href="http://www.simplesite.com/">simplesite</a>, <a href="https://wordpress.com/">wordpress</a> and <a href="https://github.com/">github</a>.</p>

<p>And finally I chose github for several reasons:</p>
<p>1. It can refer directly to the projects and material I store on website</p>
<p>2. I can write my own html file which give me a learning opportunity</p>
<p>3. It compile to the way I am working such as using eclispe, terminal and git to manage the website project.</p>

<P>Follow the <a href="http://jmcglone.com/guides/github-pages/">guide</a> to use Jekyll setup your own website</p>

