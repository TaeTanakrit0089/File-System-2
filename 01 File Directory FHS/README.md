# File and Directory

ใน Linux System ทุกอย่างคือไฟล์ ไม่ว่าจะเป็น Partition, Hardware, อุปกรณ์ต่างๆ, Driver และ Directories 

การดำเนินการต่างๆเกี่ยวกับไฟล์มีหลายเรื่องที่ต้องคำนึงมากมาย เช่น กรณีที่ตั้งชื่อไฟล์เหมือนกัน แต่ต่างกันที่ตัวพิมพ์เล็กหรือตัวพิมพ์ใหญ่  ก็ถือว่าทั้งสองไฟล์นั้นเป็นคนละไฟล์กัน

<br>
<hr>

## ประเภทของไฟล์
#### 1. Regular files
   เป็นไฟล์ทั่วไปพวก Media file, Program และ Executable file ไฟล์ชนิดนี้สามารถอยู่ในรูปของ ASCII หรือ Binary

#### 2. Directories
   สำหรับ linux ถือเป็นไฟล์ที่ใช้ในการจัดเก็บไฟล์อื่นๆและ Subdirectories

_คำสั่งที่ใช้ในการดู Directories :_

    ls -l /home/ubuntu/ | grep ^d

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-02.png)

#### 3. Special files หรือ Device files 
   เป็นพวกไฟล์อุปกรณ์ที่เชื่อมโยงกับอุปกรณ์ Hardware ในระบบ สามารถแบ่งเป็น 5 ประเภท ดังนี้

- Block file (b) : เป็นไฟล์ที่ทำหน้าที่เป็น Direct interface สำหรับ Block devices เช่น Hard drive โดยเป็นไฟล์ที่แทนอุปกรณ์ที่ถ่ายโอนข้อมูลเป็น Block โดยไฟล์เหล่านี้จะถูกเก็บอยู่ใน /dev

_คำสั่งที่ใช้ในการดู Block file :_

    ls -l /dev | grep ^b

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-03.png)

<br>

- Character device file (c) :
  เป็น hardware file ที่อ่าน/เขียนข้อมูลทีละ 1 ตัวอักษร โดยใช้รูปแบบในการ Input/Output แบบ Serial stream และให้การเข้าถึงโดยตรงกับ Hardware ตัวอย่าง Hardware เช่น Terminal, Serial port

_คำสั่งที่ใช้ในการดู Character device file :_

    ls -l /dev | grep ^c

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-04.png)

<br>

- Named pipe file หรือ just a pipe file (p) : ชื่อของ Named pipe เป็นชื่อไฟล์ในระบบไฟล์ โดยทำหน้าที่ส่งข้อมูลจาก Process หนึ่งไปยังอีก Process

_คำสั่งที่ใช้ในการดูไฟล์ Named pipe file :_

    ls -l /dev | grep ^p

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-05.png)

<br>

- Symbolic link file (l) : ทำหน้าที่ชี้ไปยังไฟล์หรือ Folder อื่นๆ ทำให้มีความยืดหยุ่นในการใช้ชื่อไฟล์ต่างกันหรืออยู่ใน Location ที่ต่างกัน 
ซึ่ง link มีอยู่ 2 ประเภท ดังนี้
   - Hard link สำหรับคัดลอกไฟล์ต้นฉบับ โดยจะไม่สามารถสร้าง Directory หรือ File ในระบบไฟล์อื่นได้
   - Soft link สำหรับชี้ไปยังไฟล์ฉบับ ซึ่งสามารถสร้าง Directory หรือ File ในระบบไฟล์อื่นได้

_คำสั่งที่ใช้ในการดูไฟล์ Symbolic link file :_

    ls -l /dev | grep ^l

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-06.png)

<br>

- Socket file (s) :  อนุญาตให้มีการแลกเปลี่ยนข้อมูลโดยไม่ต้องใช้กระบวนการที่ซับซ้อนของเครือข่ายและ Sockets โดยใช่ชื่อไฟล์เป็นที่อยู่ แทนการใช้ IP Address และ Port Number

_คำสั่งที่ใช้ในการดู Socket file :_

    ls -l /dev | grep ^s

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-07.png)

<br>
<hr>

### วิธีดูประเภทของไฟล์

- File command
คำสั่งนี้จะแสดงเฉพาะชนิดของไฟล์แต่ไม่แสดงชนิดของเนื้อหาข้างใน

_ตัวอย่างคำสั่ง_

    file fail.txt
    file /dev/*

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-08.png)

<br>

- ls -l command
คำสั่งนี้จะแสดงเนื้อหาภายใน Directory ปัจจุบันด้วย โดยตัวอักษรแรกใน List แต่ละอันบอกถึงชนิดของ File

_ตัวอย่างคำสั่ง_
    
    ls -l /dev

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-09.png)

<br>

- stat command
คำสั่งนี้แสดงข้อมูลของ File system, ขนาดของไฟล์, สิทธิ์การเข้าถึง, User และ Group IDs

_ตัวอย่างคำสั่ง_

    stat records.csv

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-10.png)

<br>
<hr>

## สรุปเรื่องประเภทของไฟล์


| ชนิด      | คำอธิบาย                                          | คำสั่งที่่ใช้ในการสร้าง | ที่อยู่ของไฟล์     | ls -l |
|-----------|---------------------------------------------------|-------------------------|--------------------|-------|
| Regular   | ประกอบไปด้วย ข้อมูลพวก text, image, video, script | touch                   | Directory ไหนก้ได้ | -     |
| Directory | ประกอบไปด้วยชื่อและที่อยู่ของไฟล์อื่นๆ            | mkdir                   | Directory          | d     |
| Block     | ทำให้เข้าถึง block device I/O                     | fdisk                   | /dev               | b     |
| Character | เกี่ยวข้องกับการเข้าถึงอุปกรณ์ด้วยตัวอักษร        | mknod                   | /dev               | c     |
| pipe      | อนุญาตให้มีการรับส่งข้อมูลระหว่างกัน              | mkfifo                  | /dev               | p     |
| Symbol    | เป็นตัวชี้หรือคัดลอกไฟล์อื่นๆ                     | ln                      | /dev               | l     |
| Socket    | ให้การแลกเปลี่ยนสื่อสารระหว่างกัน                 | socket() System call    | /dev               | s     |


## ตัวอย่างเพิ่มเติมเกี่ยวกับ Regular files
- เริ่มจากการสร้างไฟล์ด้วยคำสั่ง : touch [file name]
      
      touch testfile

- สามารถเช็คว่าไฟล์ถูกสร้างขึ้นเรียบร้อยแล้วหรือไม่ได้ : ls [option] [file name]

      ls testfile
      testfile

- เพิ่มเนื้อหาเข้าไปในไฟล์ด้วยคำสั่ง : echo

      echo "all cat are so cute" > testfile

- แสดงเนื้อหาในไฟล์ด้วยคำสั่ง : cat

      cat testfile
      all cat are so cute

- ลบไฟล์ด้วยคำสั่ง : rm [file_name]

      rm testfile

> [!NOTE]
> 
> - คำสั่ง ls และ rm สามารถใช้กับ Directory ได้
> - หากใช้คำสั่ง echo แล้วเขียนชื่อไฟล์เป็นไฟล์ที่ยังไม่ถูกสร้าง จะเป็นการสร้างไฟล์ใหม่ที่มีเนื้อหาตามที่เขียนไว้
> - ดังนั้นจึงควรระวังในการทำงานเกี่ยวกับไฟล์


## ตัวอย่างเพิ่มเติมเกี่ยวกับ Directories
- ดู Path ของ Directory ปัจจุบันที่เราอยู่ด้วยคำสั่ง : pwd

      pwd
      /home/maisan

- เข้าไปใน subdirectory ด้วยคำสั่ง : cd [directory name]

   cd a1
   pwd
   /home/maisan/a1

- ต้องการออกจาก directory ปัจจุบัน ใช้คำสั่ง : cd ..

  cd ..
  pwd
  /home/maisan

- ออกไปยัง Home directory ใช้คำสั่ง : cd ~ หรือ cd

  cd a1
  cd a2
  pwd
  /home/maisan/a1/a2

  cd
  /home/maisan
  
> [!TIP]
>
> หากต้องการดูว่า Path ของ Directory ที่เราอยู่ สามารถใช้คำสั่ง : pwd

[See another command file management here!](https://contabo.com/blog/linux-navigation-and-file-management/)
[See another file command options here!](https://phoenixnap.com/kb/linux-file-command)

<br>
<hr>

## สิทธิ์การเข้าถึงไฟล์

### 1. ประเภทของ Permissions

| ตัวอักษร | ความหมาย                                    |
|:--------:|:--------------------------------------------|
|    r     | Read : ไฟล์นี้สามารถเปิดอ่านข้อมูลในไฟล์ได้ |
|    w     | Write : ไฟล์นี้สามารถแก้ไขข้อมูลในไฟล์ได้   |
|    x     | Execute : สามารถเข้าไป execute ไฟล์ได้      |

_คำสั่งที่ใช้แสดง Permission_

    ls -l
    total 4
    -rwxrwxrwx 1 kali pentest 21 Dec 18 20:37 note

<br>

แต่ล่ะ permission สามารถแสดงในรูปแบบ **Numberic Mode** ได้ ดังนี้

| ตัวอักษร |                       Numberic Mode                        |
|:--------:|:------------------------------------------------------:|
|    r     |      4      |
|    w     |2 |
|    x     |1 |


  ตัวอย่างการใช้งาน

- Owner: rwx  = 4+2+1 = 7
- Group: r--  = 4+0+0 = 4
- Others: r-- = 4+0+0 = 4

ได้ผลลัพธ์เป็นเลขสามตัวต่อกัน คือ `744`

ตาราง **Octal permissions**

| binary | octal | permission |
|:------:|:-----:|:---------:|
|  000   |   0   |    ---    |
|  001   |   1   |    --x    |
|   010   |   2   |    -w-    |
|    011     |   3   |    -wx    |
|    100     |   4   |    r--    |
|    101     |   5   |    r-x    |
|    110    |   6   |    rw-    |
|    111     |   7   |rwx |



### Advanced file permissions

|  | ความหมาย   | ตัวอย่างการใช้งาน                                                                                                                                                                                             |
|:--------:|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    s    | 1) `SUID` ไม่ว่าคนที่เรียกใช้งานไฟล์นี้จะเป็นสิทธิ์อะไรก็ตาม แต่ไฟล์จะทำงานด้วยสิทธิ์ของเจ้าของไฟล์<br/>2) `SGID` ไม่ว่าคนที่เรียกใช้งานไฟล์นี้จะเป็นสิทธิ์อะไรก็ตาม แต่ไฟล์จะทำงานด้วยสิทธิ์ของ Group owner | `SUID` : การแก้ไข password ใน /usr/bin/psswd<br/>`SGID` สำหรับการ Share directory ให้ทุกคนใน group ที่เข้ามาสร้างไฟล์ใน Directory นี้ มี Group owner เป็น Parent directory เพื่อให้สมาชิกในกลุ่มอ่าน/เขียนได้ |
|    t     | `Sticky Bit` Directory พิเศษที่ทุกคนสามารถอ่านและเขียนได้ แต่ไม่สามารถลบได้ มีแค่เจ้าของไฟล์ (User/Owner) หรือ root เท่านั้นที่สามารถลบได้ มักใช้กับ Share Directory                                         | /tmp                                                                                                                                                                                                          |


- [ตัวอย่างเพิ่มเติมในการทำ SUID และ SGID](https://medium.com/@kanompung/%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B8%97%E0%B8%B3-suid-%E0%B9%81%E0%B8%A5%E0%B8%B0-sgid-5f9ac6011c5a)
- [รายละเอียดและตัวอย่างเพิ่มเติมเกี่ยวกับ Advanced file permissions](https://datafarm-cybersecurity.medium.com/file-permissions-in-linux-4e671dee72ca)

<br>
<hr>

### วิธีการแก้ไข File Permissions

ใช้คำสั่ง chmod โดยมีรูปแบบดังนี้


- chmod u : 

| คำสั่ง  | คำอธิบาย                                                    |
|:-------:|-------------------------------------------------------------|
| chmod u | สำหรับแก้ permission ของ user owner                         |
| chmod g | สำหรับแก้ permission ของ user ที่อยู่ใน file's group.       |
| chmod o | สำหรับแก้ permission ของ user ที่ไม่ได้อยู่ใน file's group. |
| chmod a | สำหรับ users ทุกคน                                          |

ตัวอย่างคำสั่ง

    chmod ug+rwx example.txt
    $ chmod o-r example2.txt

จากตัวอย่างเป็นการเพิ่ม Permission read, write, execute ให้กับ user และ group สำหรับไฟล์ example.txt และลบ Permission read ของ other user สำหรับไฟล์ example2.txt

<br>
<hr>

### 2. user owner and group owner

สามารถดูได้ว่าใครเป็น user owner หรือ group owner โดยใช้คำสั่งต่อไปนี้

    paul@rhel65:~/owners$ ls -lh
    total 636K
    -rw-r--r--. 1 paul snooker 1.1K Apr 8 18:47 data.odt
    -rw-r--r--. 1 paul paul 626K Apr 8 18:46 file1
    -rw-r--r--. 1 root tennis 185 Apr 8 18:46 file2
    -rw-rw-r--. 1 root root 0 Apr 8 18:47 stuff.txt
    paul@rhel65:~/owners$

หรือ
     
    paul@rhel65:~/owners$ ls -l

รายละเอียดในไฟล์ data.odt

 -rw-r--r-- แบ่งได้เป็น 4 ส่วน `-|rw-|r--|r--` โดยอธิบายตามลำดับดังนี้
- ชนิดของไฟล์ : - (Regular file)
- สิทธิ์ของ User/Owner หรือเจ้าของไฟล์ : `rw-` read ได้, write ได้, execute ไฟล์ไม่ได้
- สิทธิ์ของ Group หรือกลุ่ม : `r--` read ได้, write ไฟล์ไม่ได้, execute ไฟล์ไม่ได้
- สิทธิ์ของ Other หรือคนอื่นๆที่ไม่ใช่เจ้าของ : `r--` read ได้, write ไฟล์ไม่ได้, execute ไฟล์ไม่ได้

จะเห็นว่าใน data.odt มี user paul เป็น `user owners` และมี `group owner` คือ group snooker

### คำสั่งแสดงผู้ใช้ทั้งหมด

    paul@debian7~$ cut -d: -f1 /etc/passwd | column

### เปลี่ยน group owner และ user owner  

สามารถทำได้โดยใช้ในผู้ใช้ระดับ root โดยใช้คำสั่ง

- **chgrp**

        root@rhel65:/home/paul/owners# ls -l file2
        -rw-r--r--. 1 root tennis 185 Apr 8 18:46 file2
        
        root@rhel65:/home/paul/owners# chgrp snooker file2
        root@rhel65:/home/paul/owners# ls -l file2
        -rw-r--r--. 1 root snooker 185 Apr 8 18:46 file2
        root@rhel65:/home/paul/owners#

- **chown**

  กรณีเปลี่ยนแค่ user owner

      root@laika:/home/paul# ls -l FileForPaul
      -rw-r--r-- 1 root paul 0 2008-08-06 14:11 FileForPaul

      root@laika:/home/paul# chown paul FileForPaul
      root@laika:/home/paul# ls -l FileForPaul
      -rw-r--r-- 1 paul paul 0 2008-08-06 14:11 FileForPaul
  
  กรณีต้องการเปลี่ยนทั้ง user owner และ group owner

      root@laika:/home/paul# ls -l FileForPaul
      -rw-r--r-- 1 paul paul 0 2008-08-06 14:11 FileForPaul

      root@laika:/home/paul# chown root:project42 FileForPaul
      root@laika:/home/paul# ls -l FileForPaul
      -rw-r--r-- 1 root project42 0 2008-08-06 14:11 FileForPaul


[//]: # ()
[//]: # (| Option  | Function           |)

[//]: # (|---------|--------------------|)

[//]: # (| file -s | ใช้สำหรับไฟล์พิเศษ |)

<br>
<hr>
    
# File System Hierarchy Standard (FHS)
FHS (File System Hierachy Standard) เป็นโครงสร้าง file system แบบ tree โดยเริ่มจาก root directory ไล่ลงมาเป็น system directory และ user directory โดยใช้เพื่อการนิยามชื่อ ที่อยู่ และสิทธิ์การเข้าถึงของไฟล์ชนิดต่างๆและ Directory เป็นแบบลำดับขั้น (hierarchy) หากต้องการเข้าถึงทุก directory ต้องเข้าถึงในฐานะ root user หรือผู้ใช้ที่มีสิทธิ์สูงสุด

### FHS Standard ได้จัดหมวดหมู่ของแต่ละ System Directory ดังนี้

- จัดในหมวด `Shareable or Not` ก็คือ Directory นั้นสามารถ Shared ผ่าน Network และใช้บนเครื่องคอมพิวเตอร์ได้หลายเครื่อง
- จัดในหมวด` Static` ไม่สามารถแก้ไขได้หรือ `Variable` สามารถแก้ไขได้
<br>

|  | Not Shareable | Shareable |
|---------|--------------|-----------|
| **Variable** | /var/lock    | /var/mail |
| **Static**   | /etc             | /opt      |

![](https://ndg-content-dev.s3.amazonaws.com/media/images/linux-essentials-v2/LEv2_13_2.png)
- **The /boot/ Directory**

  ประกอบไปด้วยไฟล์ที่จำเป็นในการ boot ระบบ อย่างพวก linux karnel เพื่อช่วยให้การ boot เป็นไปได้อย่างถูกต้อง


- **The /dev/ Directory(dev : device)**

  มีไว้สำหรับเก็บไฟล์อุปกรณ์(device file) ที่ใช้เพื่อเชื่อมต่อกับอุปกรณ์ต่างๆในระบบ ไฟล์ที่จำเป็นเพื่อให้ระบบทำงานได้อย่างถูกต้อง


- **The /etc/ Directory (etc : et cetera)**

  เก็บพวกไฟล์การตั้งค่าที่สำคัญต่อโปรแกรมทั้งหมด


- **The /lib/ Directory (lib : library)**

  เป็นส่วนสำคัญสำคัญการ booting ระบบและการทำงานของคำสั่งภายระบบ root file


- **The /media Directory**

  ประกอบไปด้วย subdirectories ที่ใช้เป็น mount point สำหรับสื่อที่สามารถถอดได้ เช่น CD-ROM


- **The /mnt Directory (mnt : mount)**

  โดยทั่วไปใช้เป็นจุดติดตั้งสำหรับระบบไฟล์ที่ติดตั้งเพิ่มเติม (การ mount ใน linux คือการทำให้ระบบของ linux มีความพร้อมในการอ่าน/เขียนไฟล์ที่ถูกติดตั้งเข้ามาใหม่ได้)


- **The /opt/Diectory (opt : optional)**

  เก็บพวก package ที่ติดตั้งมาจากภายนอก


- **The /proc/Directory (proc : process)**

  เป็นไฟล์พิเศษที่ถูกสร้างขึ้นใน system memory โดยจะไม่มีการปรากฏอยู่บน disk โดยในส่วนนี้จะประกอบด้วยข้อมูลสำคัญเกี่ยวกับสถานะของระบบ  ซึ่งมีไว้ควบคุมและจัดการทรัพยากรต่างใน kernal โดย /proc ต้องมีการ mounted ก่อน


- **The /sbin/Directory (sbin : system binary)**

  เก็บโปรแกรมที่สามารถให้ผู้ใช้ระดับสิทธิ์สูงสุด (root user) เรียกใช้ได้


- **The /srv/Directory (sys : system)**

  เก็บข้อมูลที่เฉพาะเจาะจงสำหรับบริการที่ระบบของผู้ใช้กำลังให้บริการ ตัวอย่างเช่น FTP, WWW, หรือ CVS. ไดเรกทอรีนี้จะแสดงตำแหน่งของไฟล์ข้อมูลสำหรับบริการที่ระบบบริการนั้น ๆ โดยเฉพาะ


- **The /sys/Directory (sys : system)**

  ใช้ระบบไฟล์เสมือน (virtual file system) แบบใหม่ที่เรียกว่า sysfs มีเฉพาะกับ karnel เวอร์ชัน 2.6 กับการ support จากอุปกรณ์  hot plug ในเคอร์เนลเวอร์ชัน 2.6, ไดเรกทอรี /sys/ ประกอบด้วยข้อมูลที่คล้ายกับที่อยู่ใน /proc/ แต่แสดงผลในรูปแบบลำดับชั้นข้อมูลเฉพาะเกี่ยวกับอุปกรณ์  hot plug .


- **The /usr/Directory**

  ประกอบด้วยไดเรกทอรีย่อยมากมายที่สำคัญเพื่อรองรับผู้ใช้ โดยให้แอปพลิเคชัน, ไลบรารี, และเอกสาร.


- **The /var/Directory (var: variable)**

  ใช้เก็บ subdirectory สำหรับงานที่ไฟล์มีการเปลี่ยนแปลงบ่อย



| Directory   | Contents                                                                                                                                                     |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| / | เป็นฐานของโครงสร้างหรือ root ของ เป็น Directory ที่รวมทุก Directory ไม่ว่าจะเป็น Partition ที่ติดตั้งในเครื่อง, อุปกรณ์ที่สามารถถอดออกได้หรือ network shares |
| /root  | เป็น home directory ของ root user                                                                                                                            |
| /bin   | binary ที่จำเป็นเช่น ls, rm โดยเป็นส่วนหนึ่งของ root file system                                                                                             |
| /home       | directory ที่ใช้เก็บข้อมูลและไฟล์ส่วนตัวของผู้ใช้                                                                                                            |
| /lib64      | libraries ที่สร้างขึ้นสำหรับบางสถาปัตยกรรม มีไว้เพื่อให้ระบบสามารถรองรับหลายๆสถาปัตยกรรมได้อย่างยืดหยุ่น.                                                    |
| /var/cache  | ไฟล์ที่ใช้เพื่อเก็บข้อมูลที่ถูก cache โดยแอปพลิเคชัน                                                                                                         |
| /var/log    | พวก log file                                                                                                                                                 |
| /var/lock   | ไฟล์สำหรับจัดการการเข้าถึงทรัพยากรในการ shared ทรัพยากรร่วมกัน                                                                                               |
| /var/spool  | spool file สำหรับการ print และที่เกี่ยวของกับ mail                                                                                                           |
| /var/tmp    | ไฟล์ที่ถูกสร้างไว้ชั่วคราว โดยจะไม่ถูกลบเมื่อระบบทำการ reboot                                                                                                |

[//]: # ()
[//]: # (| Comment |)

[//]: # (|---------|)

[//]: # (| koako   |)

# Reference 

- [cisco](https://content.netdevgroup.com/contents/linux-essentials/UbvrXt151d/)
- [javapoint](https://www.javatpoint.com/linux-files#:~:text=In%20Linux%20system%2C%20everything%20is,Files%20are%20always%20case%20sensitive)
- [redhat FHS](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/4/html/reference_guide/s1-filesystem-fhs)
- [linuxiosys](https://linuxopsys.com/topics/file-types-in-linux)
- [rackspace](https://docs.rackspace.com/docs/create-files-in-linux)
- [linuxsto.pdf](https://linux-training.be/linuxsto.pdf?fbclid=IwAR0iJbUx-0Gbl3RaEXDn4iZCTVuzBy1RC7ytevA9XXRx5sxFmDS7zOYrjM8)
- [greeksforgeeks](https://www.geeksforgeeks.org/linux-directory-structure/)
- [redhat Linux file permissions](https://www.redhat.com/sysadmin/linux-file-permissions-explained)
- [Medium Linux file permissions](https://datafarm-cybersecurity.medium.com/file-permissions-in-linux-4e671dee72ca)
- [The Complete Reference LInux Sixth Edition.pdf](https://doc.lagout.org/operating%20system%20/linux/Linux%20-%20The%20Complete%20Reference.pdf?fbclid=IwAR07KOfQrR5c1Rd2Vrcew7x8vSd_QI-79OQNH7jnA_grvO_osKb-6V_1740)
