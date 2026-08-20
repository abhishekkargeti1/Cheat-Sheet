#  		  Linux Cheat Sheet





sudo usermod -aG docker $USER \&\& newgrp docker

###### &#x20;



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





To create a ssh private and public key



cd \~/.ssh

ssh-keygen (To create public /private key )





&#x20;(To check CPU Usage) Top



&#x20;(To check Ram Usage) free -h



&#x20;(To check Disk Usage ) df -h



&#x20;(To check Date) date



&#x20;(TO make folder ) mkdir folder name



&#x20;(To check folder permission and details ) ls -l



&#x20;(To check present path ) pwd



&#x20;(To create new file ) touch filename.extension



&#x20;(To remove any file ) rm filename.extension



&#x20;(To delete any folder which is not empty ) rm -r folder name



&#x20;(To delete any folder which is empty) rmdir folder name



&#x20;(To print any thing on screen and also in file ) echo "Hello World" / echo "Hello World " > filename.extension

&#x20;

&#x20;(To run two commands together ) first command | second command



&#x20;(To print first 5 lines form the file ) head filename.extension



&#x20;(To print last 5 line from the file) tail filename.extension



&#x20;(To monitor file change ) tail -f filename.extension



&#x20;(To see file in paginated form ) less / more filename.extension



&#x20;(TO copy any file from location to another ) cp filename.extension location path/name of the destination



&#x20;(To transfer  empty folder) cp -r source folder location path/name of the destination



&#x20;(TO CUT PASTE file/folder from one place to another) mv source file/folder location path/name of the destination



&#x20;(TO rename any file or folder) mv old filename/foldername new filename/foldername



&#x20;(To create any softlink of any file) ln -s file location  name of the softlink  (Softlink is delete if source is delete)



&#x20;(To create any hardlink of any file) ln  file location  name of the hardlink  (Hardlink is not delete if source is delete)



&#x20;(To get some bit from the file ) cut -b amount of bit you want filename



&#x20;(To append any thing in file and the same you want to print it on the screen ) echo "Heelo World 1" | tee helloworld.txt



&#x20;(TO check diffrence between two files ) diff first filename.extension second filename.extension



&#x20;(To store any log in to file ) nohup command

&#x20;

&#x20;(To check info about RAM ) vmstat / vmstat -a



&#x20;(To check the type of OS ) uname



&#x20;(To check the running time of the system) uptime



&#x20;(To check the login of the user) who



&#x20;(To check the current user) whoami

&#x20;

&#x20;(To check the location of the installed software ) which software name



&#x20;(To check the id of the current user ) id



&#x20;(To shutdown the system) sudo shutdown



&#x20;(To restart the system) sudo restart

&#x20;

&#x20;(To install any software) sudo apt install software name



&#x20;(To remove any software) sudo apt remove software name



&#x20;(To make a new user ) sudo useradd -m username



&#x20;(TO set the password for the user ) sudo passwd username

&#x20;

&#x20;(To switch user ) su usename



&#x20;(To check the list of user) sudo cat /etc/passwd

&#x20;

&#x20;(To create a group ) sudo groupadd group name

&#x09;

&#x20;(To check the list of group) sudo cat /etc/group



&#x20;(To add new user in a group) sudo gpasswd -a username groupname



&#x20;(To add multiple new user in a group) sudo gpasswd -m username groupname



&#x20;(To delete the group) sudo groupdel groupname

File Permission chart



| Binary | Octal     | String Representation | Permissions            |

| ------ | --------: | --------------------- | ---------------------- |

| 000    | 0 (0+0+0) | `---`                 | No Permission          |

| 001    | 1 (0+0+1) | `--x`                 | Execute                |

| 010    | 2 (0+2+0) | `-w-`                 | Write                  |

| 011    | 3 (0+2+1) | `-wx`                 | Write + Execute        |

| 100    | 4 (4+0+0) | `r--`                 | Read                   |

| 101    | 5 (4+0+1) | `r-x`                 | Read + Execute         |

| 110    | 6 (4+2+0) | `rw-`                 | Read + Write           |

| 111    | 7 (4+2+1) | `rwx`                 | Read + Write + Execute |



(To check the default permission of the system) umask



(To change the ownership of the file ) chown ownername filename.extension



(To change the group of the file ) chgrp groupname filename.extension



(To zip any non empty  folder) zip -r zipfoldername actualfoldername



(To zip any file) zip  zipfilename actualfilename



(To compress tar file/folder) tar -cvzf zipedfile/foldername actualfolder/filename



(To decompress tar file/folder) tar -xvzf  zipedfile/foldername



(To send file from local system to server )  scp -i "Private Key Path " "Source File" public DNS:"Destination of the File"(in local system)



(To send file from server to local system )  scp -i "Private Key Path "  public DNS: "Source File Location" "Destination path"(in local system)



(To connect server folder and local folder together) rsync -e "ssh -i Private Key Path " -avz FolderName public DNS:Location of the source folder



(To beautify json ) command | jq

Networking Commands



(To check internet or server is working ) ping Ip address/domain name if the website



(TO check the network status )netstat or ss



(To check the net interface) ifconfig



(To check the route of the ping ) traceroute Ip address/domain name of the website



(To check the route of the ping ) tracepath Ip address/domain name of the website



(This command is the combination of ping and tracepath ) mtr Ip address/domain name of the website



(To get the details of the domain ) nslookup domain name of the website



(To get the details of the domain with port ) telnet domain name of the website port



(To change the hostname of the system) hostname new hostname (Host file path ) /etc/hosts



(TO check the ip of the system) ip address show



(To check the wireless connection ) iwconfig



(To get the host details of the domain ) dig hostname



(To get the host details of the domain ) whois hostname

&#x20;

(To check the mac address of the device) arp



(To check the status of the interface) ifplugstatus



(To hit any endpoint) curl -X GET endpoint | jq



(TO download any thing from internet ) weget url



(To run any command in equal interval ) watch command



(TO check number of ports which are open ) nmap -v ip address



(To check the route table ) route





Advance Command



(To manipulate any file data ) awk  (Awk need Sorted data)



awk '{print $colunm number}' logfilename



awk '/filter(write any key word awk will find that from the file)/{print $colunm number}' logfilename.log



awk '{print $colunm number}' logfilename > newlogfile.log  (To save the extracted log to new file )



awk '/filter/{count++}END{print count++}' logfilename (To find the number of occurrence of a word of a line)



awk '$2 >= "07:00:00" \&\& $2 <= "07:52:07" \&\& $3 =="INFO" {print $1,$2,$3,$4}' log1.log  (We can put condition like this also in awk)







(To manipulate any file data ) sed (Sed doesn't need sorted or structured data)





sed -n /find the word in the file /p log1.log  (To or search any particular word/line in the file)



&#x20;sed 's/Word to be replaced/With new Word/g' newlogfile.log   (To replace any word in the file)







(To manipulate any file data ) grep





grep -i word to find in the file filename.extension (-i is for case insentitive)



grep -i -c word to find in the file filename.extension (-c is for word count)

