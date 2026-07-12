#  			       Linux Cheat Sheet







###### sudo usermod -aG docker $USER



lsdlk (To check the disk partition)



lscpu (TO check the cpu config)



top (Task Manager)





To Mount the new Storage 



&#x09;sudo fdisk <Disk Name>

&#x09;sudo mkfs.ext4 <Disk name>

&#x09;	n,p,1,     w

&#x09;sudo mkfs.ext4 /dev/xvdd1  (to format the Partition )

&#x09;sudo mkdir <mount point> ( e.g. /data) (To create a mount point)

&#x09;sudo mount /dev/xvdd1 /data (to mount the new partition)

