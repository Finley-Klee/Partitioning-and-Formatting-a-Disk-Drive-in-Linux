# Partitioning-and-Formatting-a-Disk-Drive-in-Linux
My work on the lab from the Google IT Support Specialist certification in module 4 of Operating Systems and You: Becoming a Power User

<h2>Description</h2>
In this lab, I learned how to partition and format a disk drive in Linux. Partitions are important because a file system can't function without one. When you acquire a new disk drive, at least one partition is required in order to be able to write files to the file system. Different partitions can then have different file formats, depending on their purpose. For example, a disk partition that acts as a swap for your main memory may have a different file format than the default user-facing file systems. Partitions, like those used for system recovery, may also have different file formats.

<h2>Utilities Used</h2>

- <b>fdisk</b> 

<h2>Environments Used </h2>

- <b>Linux</b>

<h2>Project walk-through:</h2>

- <b>Understanding the Disk's Structure</b>
<p>Before I can partition or format the disk, I first need to familiarize myself with the current disk structure. To do this, I used the lsblk command, the df command, and the fdisk utility.</p>
<br>
<p align="center">The first thing I did to explore the current disk structure was use the lsblk command to show the information about all of the devices attached to the system. I can see from this that there are two block devices, or disks, listed: sda and sdb, and that each one currently has 12 partitions.<br/>
  <img src="https://github.com/user-attachments/assets/51f2d4f0-14c8-499e-b827-c8b0d81604ee" height="80%" width="80%" alt="a black screen with white text. The lsblk command is written at the top, followed by a table with 7 columns which read Name, maj min, RM, size, RO, type, and mountpoint. Below the table headers is information about the two disks, sda and sdb, which each have 12 partitions. Sdb is mounted to /etc/hosts."/>
  <br />
  <br />
Although we can already see from the lsblk command that sda doesn't have anything listed in the mountpoint column, I confirmed which disks were mounted to the system using the df command and appending the -h flag to make the output human readable. I can see, once again, that the sdb disk is mounted to the system and the sda disk is not.<br />
  <img src="https://github.com/user-attachments/assets/e32e4949-e107-44e0-b33c-92bd1d7452ab" height="80%" width="80%" alt="a black screen with white text. The df command with the -h flag is written on the first line followed by a table with 6 columns which read filesystem, size, used, avail, use%, and mounted on. The last row of the table reads /dev/sdb1 and indicates that the sdb disk is mounted to the /etc/hosts location."/>
  <br />
  <br />
 To gather more information about the partitions currently on the disks, I used fdisk with the -l flag to list the partitions in the block. <br />
  <img src="https://github.com/user-attachments/assets/7f293cc5-dd74-4189-9c16-57697c4a34d1" height="80%" width="80%" alt="black screen with white text showing information about the 12 partitions of the sda disk."/>
  <img src="https://github.com/user-attachments/assets/3531137b-c035-4a91-842a-380b7db30be3" height="80%" width="80%" alt="black screen with white text showing information about the 12 partitions of the sdb disk."/>
</p>
<br />
<br />
- <b>Using fdisk to partition the unmounted device</b>
<p>Now that I have gathered information about the current state of the disks, I can proceed to making changes.</p>
<br>
<p align="center">To start, I opened fdisk in interactive mode and passed in the name of the disk I want to modify, in this case sda, which is the unmounted disk.<br/>
  <img src="https://github.com/user-attachments/assets/5072bf67-b947-436e-8eed-cea986c90780" height="80%" width="80%" alt="a black screen with white text, the command at the top reads sudo fdisk /dev/sda. The following lines show the welcome message from the fdisk utility and a new command line within the utility which also reminds you that you can use the command m to show the help page."/>
  <br />
  <br />
To get familiarized with the fdisk utility, I typed the letter m and then enter to dispaly the help page. This printed a list of the commands available for the utility.<br />
  <img src="https://github.com/user-attachments/assets/682ede20-4bc2-44a9-bf3f-f4acc762c028" height="80%" width="80%" alt="a black screen with white text showing the help page of the fdisk utility."/>
  <br />
  <br />
I learned from the help page that I can use the command p to list details about the partitions on the disk.<br />
  <img src="https://github.com/user-attachments/assets/433628d9-6575-43ae-a185-269ee974c41d" height="80%" width="80%" alt="a black screen with white text. The output from the p command shows information about the sda disk and its current partitions including that it is 10 GiB in size and has 12 partitions right now."/>
   <br />
  <br />
 For this project, I want to create 2 partitions on the sda disk. The first will be a swap partition of 1 GB and the other will be an ext4 file system partition of 9 GB. In order to create these partitions, I first need to remove the partitions already on the disk. I used the d command to delet the partitions one by one going backwards from the 12th partition to the first.<br />
  <img src="https://github.com/user-attachments/assets/68cbc372-46a2-4d51-bd46-c545679c7cd3" height="80%" width="80%" alt="a black screen with white text showing the partitions 12 through 7 being deleted one at a time."/>
  <img src="https://github.com/user-attachments/assets/ba2cd327-06c3-4bdc-aed7-b1d1b202fe9d" height="80%" width="80%" alt="a black screen with white text showing the partitions 6 through 1 being deleted one at a time."/>
   <br />
  <br />
Next, I was able to create new partitions using the n command. I left the starting sector of the partition unchanged, and specified the size by typing in 2097200 to make the 1 GB partition.<br />
  <img src="https://github.com/user-attachments/assets/110cbd8d-5589-45a5-8c51-d17a9039c19b" height="80%" width="80%" alt="a black screen with white text showing the n command, partition 1, the default first sector, and the last sector as 2097200. The last line confirms the creation of a new partition 1 with the type Linux Filesystem and the size 1023 MiB."/>
   <br />
  <br />
Then, I created the second partition, also using the n commnad, but this time I left both the start and end of the partition as the default to assign all of the remaining space in the disk to partition 2.<br />
  <img src="https://github.com/user-attachments/assets/bc59faab-e32b-40d6-aa66-6f484ad103e6" height="80%" width="80%" alt="a black screen with white text showing the n command, partition 2, the default first sector, and last sector. The last line confirms the creation of a new partition 2 with the type Linux Filesystem and the size 9 GiB."/>
</p>
<br />
<br />

- <b>Changing the partition types</b>
<p>Now that I have created my two new partitions, I can change the type of my first partition to be a linux swap.</p>
<br>
<p align="center">I used the t command in the fdisk utility to specify the type I want to assign to the partition, and typed in 1 for the partition number because I want to change the type of my first partition. I brought up the list of types with L and found that linux swap is number 19, so where the utility prompts for partition type I typed in 19 and hit enter.<br/>
  <img src="https://github.com/user-attachments/assets/cbbae224-36d1-4141-b49d-a292c3d3d029" height="80%" width="80%" alt="a black screen with white text showing the t command followed by prompts for the partition number and partition type. In the line for parition type the letter L is witte followed by a list showing all of the different available types."/>
  <img src="https://github.com/user-attachments/assets/f5cb946e-5c19-416a-b34f-c2f070319ec3" height="80%" width="80%" alt="a black screen with white text which reads partition type 19 and the next line reads changed type of partition linux filesystem to linux swap."/>
  <br />
  <br />
 Now that I have made the changes that I want to make, I need to write those changes to the disk using the v command to verify the changes and the w command to write them. <br />
  <img src="https://github.com/user-attachments/assets/676e5160-a1ff-4adb-97bd-663b28c4de78" height="80%" width="80%" alt="a black screen with white text showing the command v followed by lines reading no errors detected, header version 1.0, using 2 out of 128 partitions, A total of 4013 free sectors is available in 2 segments the largest is 1007 KiB. Then the next block of text shows the command w followed by confirmation reading the partition table has been altered, calling ioctl to re-read partition table, sycning disks."/>
</p>
<br />
<br />

- <b>Using mkfs to format partitions and mounting the disk</b>
<p>The last step in the process is to update the file system on the 9 GB partition to ext4 and mount the sda disk to the system.</p>
<br>
<p align="center"><br/>
  <img src="" height="80%" width="80%" alt="image one"/>
  <br />
  <br />
  Step Two: <br />
  <img src="" height="80%" width="80%" alt="image two"/>
  <br />
  <br />
  Step Three: <br />
  <img src="" height="80%" width="80%" alt="image three"/>
   <br />
  <br />
  Step Four: <br />
  <img src="" height="80%" width="80%" alt="image four"/>
   <br />
  <br />
  Step Five: <br />
  <img src="" height="80%" width="80%" alt="image five"/>
</p>

