---
layout: post
title:  "Setting up your own distributed system"
date:   2016-07-07 12:03:30 -0700
categories: Technology
---

<h>Setting up your own distributed system</h>

<p>I use virtual machines to setup my own distributed system.</p>
<p>Components of this example are listed as below. You can choose other components as
to your preference.</p>
<p>1. Host OS: MAC OS</p>
<p>2. Virtualization tool: Virtualbox, it seems to be the most suitable one for MAC, free and
easy to use, if your host is linux or windows, you can have other choices, such as
VMware.</p>
<p>3. Virtual machine OS(guest OS): ubuntu</p>


<h1>Steps:</h1>
<p>1. Download and install Virtualbox from <a href="https://www.virtualbox.org/">virtualbox</a> 
<p>2. Create several VMs with the default options. (I have master, worker1, worker2)
<p>3. download ubuntu iso for VMs, ubuntu iso can be found in <a href="http://www.ubuntu.com/download">ubuntu</a> </p>
<p>4. Start the VMs and install ubuntu on each VM</p>
<p>5. apt install on VMs (when you need to access outside network use NAT network
option in settings->network, otherwise host-only adapter is good for internal
communication between VMs and VM-host.)</p>
<p>1) sudo apt install golang-go</p>
<p>2) sudo apt install nfs-common</p>
<p>3) sudo apt install ssh</p>
<p>4) sudo apt install nfs-kernel-server</p>
<p>6. create a new disk on master</p>
<p>1) power off master vm</p>
<p>2) settings->storage->+->create a sata control disk</p>
<p>3) power on master vm</p>
<p>4) config the disk</p>
<p>- Run</p>
<p>sudo fdisk /dev/sdb</p>
<p>Then type o and press enter # creates a new table</p>
<p>Then type n and press enter # creates a new partition</p>
<p>Then type p and press enter # makes a primary partition.</p>
<p>Then type 1 and press enter # creates it as the 1st partition</p>
<p>(use default setting in this step)</p>
<p>Finally type w # this will write any changes to disk.</p>
<p>Okay now you have a partition, now you need a filesystem.</p>
<p>- Run</p>
<p>sudo mkfs.ext4 /dev/sdb1</p>
<p>- add below line into /etc/fstab</p>
<p>/dev/sdb1 /home/yourname/GO ext4 defaults 0 1</p>
<p>- create a floder /home/yourname/GO on your master</p>
<p>mkdir /home/yourname/GO</p>
<p>- restart master</p>
<p>7. share the folder /home/yourname/GO on master</p>
<p>1) add below line in /etc/exports</p>
<p>/home/yourname/GO ipaddress of your workers(rw)</p>
<p>2) service nfs-kernel-server restart</p>
<p>8. mount the folder on master to the workers, on workers, execute</p>
<p>mkdir /home/yourname/GO</p>
<p>mount -t nfs ipaddressOfmaster:/home/yourname/GO /home/yourname/GO</p>
<p>9. set the GOPATH on all vms, you can add the line below to ~/.bashrc</p>
<p>export GOPATH=/home/yourname/GO</p>
<p>open a new terminal</p>
<p>11. Put your mapreduce code on to one of your vm’s /home/yourname/GO/src as well
as the kjv12.txt file, organize them in below structure will make things easier</p>
<p>- /home/yourname/GO/src</p>
<p>- kjv12.txt</p>
<p>- wc.go</p>
<p>- /home/yourname/GO/src/mapreduce</p>
<p>- common.go</p>
<p>- worker.go</p>
<p>- master.go</p>
<p>- mapreduce.go</p>
<p>12. change the code of you mapreduce using tcp rpc instead of unix one.</p>
<p>13. on master</p>
<p>go run wc.go master kjv12.txt ipaddressOfmaster:7777</p>
<p>14. on workers</p>
<p>go run wc.go worker ipaddressOfmaster:7777 ipaddressOfthisWorker:portyoulike</p>
<p>Then you can see the result….</p>

