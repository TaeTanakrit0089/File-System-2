# File and Directory

ใน Linux System ทุกอย่างคือไฟล์ ไม่ว่าจะเป็น Partition, Hardware, อุปกรณ์ต่างๆ, driver และ directories 

การดำเนินการต่างๆเกี่ยวกับไฟล์มีหลายเรื่องที่ต้องคำนึงมากมาย กรณีที่ตั้งชื่อไฟล์เหมือนกันแต่ต่างกันที่ตัวพิมพ์เล็กหรือตัวพิมพ์ใหญ่  ก็คือว่าทั้งสองไฟล์นั้นเป็นคนละไฟล์กัน

## ประเภทของไฟล์
#### 1. Regular files
   เป็นไฟล์ทั่วไปพวก Media file, Program และ Executable file ไฟล์ชนิดนี้สามารถอยู่ในรูปของ ASCII หรือ Binary

#### 2. Directories
   สำหรับ linux ถือเป็นไฟล์ที่ใช้ในการจัดเก็บไฟล์อื่นๆและ Subdirectories

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /home/ubuntu/ | grep ^d

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-02.png)

#### 3. Special files หรือ Device files 
   เป็นพวกไฟล์อุปกรณ์ที่เชื่อมโยงกับอุปกรณ์ฮาร์ดแวร์ในระบบ สามารถแบ่งเป็น 5 ประเภท ดังนี้

- Block file (b) : เป็นไฟล์ที่ทำหน้าที่เป็น Direct interface สำหรับ Block devices เช่น Hard drive โดยเป็นไฟล์ที่แทนอุปกรณ์ที่ถ่ายโอนข้อมูลเป็น Block โดยไฟล์เหล่านี้จะถูกเก็บอยู่ใน /dev

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^b

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-03.png)

- Character device file (c) :
  เป็น hardware file ที่อ่าน/เขียนข้อมูลทีล่ะ 1 ตัวอักษร โดยใช้รุปแบบในการ Input/Output แบบ Serial stream และให้การเข้าถึงโดยตรงกับ Hardware ตัวอย่าง Hardware เช่น terminal, Serial port

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^c

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-04.png)

- Named pipe file หรือ just a pipe file (p) : ชื่อของ Named pipe เป็นชื่อไฟล์ในระบบไฟล์ โดยทำหน้าที่ส่งข้อมูลจาก Process หนึ่งไปยังอีก Process

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^p

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-05.png)

- Symbolic link file (l) : ทำหน้าที่ชี้ไปยังไฟล์หรือ Folder อื่นๆ ทำให้มีความยืดหยุ่นในการใช้ชื่อไฟล์ต่างกันหรืออยู่ใน Location ที่ต่างกัน 
ซึ่ง link มีอยู่ 2 ประเภท ดังนี้
   - Hard link สำหรับคัดลอกไฟล์ต้นฉบับ โดยจะไม่สามารถสร้าง Directory หรือ File ในระบบไฟล์อื่นได้
   - Soft link สำหรับชี้ไปยังไฟล์ฉบับ ซึ่งสามารถสร้าง Directory หรือ File ในระบบไฟล์อื่นได้

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^l

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-06.png)

- Socket file (s) :  อนุญาตให้มีการแลกเปลี่ยนข้อมูลโดยไม่ต้องใช้กระบวนการที่ซับซ้อนของเครือข่ายและ Sockets โดยใช่ชื่อไฟล์เป็นที่อยู่ แทนการใช้ IP Address และ Port Number

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^s

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-07.png)


### วิธีดูประเภทของไฟล์
- File command
คำสั่งนี้จะแสดงเฉพาะชนิดของไฟล์แต่ไม่แสดงชนิดของเนื้อหาข้างใน

_ตัวอย่างคำสั่ง_

    file fail.txt
    file /dev/*

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-08.png)

- ls -l command
คำสั่งนี้จะแสดงเนื้อหาภายใน Directory ปัจจุบันด้วย โดยตัวอักษรแรกใน List แต่ล่ะอันบอกถึงชนิดของ File

_ตัวอย่างคำสั่ง_
    
    ls -l /dev

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-09.png)

- stat command
คำสั่งนี้แสดงข้อมูลของ File system, ขนาดของไฟล์, สิทธิ์การเข้าถึง, User และ Group IDs

_ตัวอย่างคำสั่ง_

    stat records.csv

![](https://linuxopsys.com/wp-content/uploads/2023/06/Linux-File-Types-142023-10.png)

## สรุปเรื่องประเภทของไฟล์


| ชนิด       | คำอธิบาย                                            | คำสั่งที่่ใช้ในการสร้าง  | ที่อยู่ของไฟล์  | ls -l |
|------------|-----------------------------------------------------|---|---|-------|
| Regular    | ประกอบไปด้วย ข้อมูลพวก text, image, video, script   | touch  | directory ไหนก้ได้  | -     |
| Directory  | ประกอบไปด้วยชื่อและที่อยู่ของไฟล์อื่นๆ              | mkdir  | Directory  | d     |
| Character  | เกี่ยวข้องกับการเข้าถึงอุปกรณ์ด้วยตัวอักษร          | mknod  | /dev  | c     |
| pipe       | อนุญาตให้มีการรับส่งข้อมูลระหว่างกัน                | mkfifo  |/dev   | p     |
| Symbol     | เป็นตัวชี้หรือคัดลอกไฟล์อื่นๆ                       | ln  |/dev   | l     |
| Socket     | ให้การแลกเปลี่ยนสื่อสารระหว่างกัน                   | socket() system call  |/dev   | s     |


## ตัวอย่างอื่นๆเกี่ยวกับ Regular file
- เริ่มจาการสร้างไฟล์ด้วยคำสั่ง : touch [file name]
      
      touch testfile

- สามารถเช็คว่าไฟล์ถูกสร้างขึ้นเรียบร้อยแล้วหรือไม่ได้ : ls [file name]

      ls testfile
      testfile

- เพิ่มเนื้อหาเข้าไปในไฟล์ด้วยคำสั่ง : echo

      echo "all cat are so cute" > testfile

- แสดงเนื้อหาในไฟล์ด้วยคำสั่ง : cat

      cat testfile
      all cat are so cute

## สิทธิ์การเข้าถึงไฟล์

### 1. ประเภทของ Permission

| ตัวอักษร | ความหมาย                                               |
|:--------:|:-------------------------------------------------------|
|    r     | Read : ไฟล์นี้สามารถเปิดอ่านข้อมูลในไฟล์ได้            |
|    w     | Write : ไฟล์นี้สามารถแก้ไขข้อมูลในไฟล์ได้              |
|    x     | Execute : สามารถเรียกใช้งานหรือ run การทำงานของไฟล์ได้ |

คำสั่งที่ใช้แสดง Permission

    ls -l
    total 4
    -rwxrwxrwx 1 kali pentest 21 Dec 18 20:37 note
แต่ล่ะ permission สามารถแสดงในรูปแบบ Numberic Mode ได้ ดังนี้

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



### Permission พิเศษ

| ตัวอักษร | ความหมาย                                                                                                                                                                                                    | ตัวอย่างการใช้งาน                                                                                                                                                          |
|:--------:|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    s    | 1) `SUID` ไม่ว่าคนที่เรียกใช้งานไฟล์นี้จะเป็นสิทธิ์อะไรก็ตาม แต่ไฟล์จะทำงานด้วยสิทธิ์ของเจ้าของไฟล์<br/>2) `SGID` ไม่ว่าคนที่เรียกใช้งานไฟล์นี้จะเป็นสิทธิ์อะไรก็ตาม แต่ไฟล์จะทำงานด้วยสิทธิ์ของ Group owner | `SUID` : การแก้ไข password ใน /usr/bin/psswd<br/>`SGID` สำหรับการ Share directory ให้ทุกคนใน group ที่เข้ามาสร้างไฟล์ใน Dierctory นี้ มี Group owner เป็น Parent directory |
|    t     | `Sticky Bit` Directory พิเศษที่ทุกคนสามารถอ่านและเขียนได้ แต่ไม่สามารถถูกลบได้ และจะสามารถถูกลบได้โดย เจ้าของไฟล์ (User/Owner) หรือ root เท่านั้นมักใช้กับ Share Directory                                  | /tmp                                                                                                                                                                       |



### วิธีการแก้ไข File permissions

ใช้คำสั่ง chmod โดยมีรูปแบบดังนี้


chmod u 

    chmod ug+rwx example.txt

จากตัวอย่างเป็นการเพิ่ม Permission read, write, execute ให้กับ user และ group


### 2. user owner and group owner

สามารถดูว่าใครเป็น user owner หรือ group owner ได้ โดยใช้คำสั่งต่อไปนี้

    paul@rhel65:~/owners$ ls -lh
    total 636K
    -rw-r--r--. 1 paul snooker 1.1K Apr 8 18:47 data.odt
    -rw-r--r--. 1 paul paul 626K Apr 8 18:46 file1
    -rw-r--r--. 1 root tennis 185 Apr 8 18:46 file2
    -rw-rw-r--. 1 root root 0 Apr 8 18:47 stuff.txt
    paul@rhel65:~/owners$

หรือ
     
    paul@rhel65:~/owners$ ls -l

รายละเอียดใน data.odt

 -rw-r--r-- แบ่งได้เป็น 4 ส่วน `-|rw-|r--|r--` โดยอธิบายตามลำดับดังนี้
- ชนิดของไฟล์ : -
- สิทธิ์ของ User/Owner หรือเจ้าของไฟล์ : `rw-` read ได้, write ได้, execute ไฟล์ไม่ได้
- สิทธิ์ของ Group หรือกลุ่ม : `r--` read ได้, write ไฟล์ไม่ได้, execute ไฟล์ไม่ได้
- สิทธิ์ของ Other หรือคนอื่นๆที่ไม่ใช่เจ้าของ : `r--` read ได้, write ไฟล์ไม่ได้, execute ไฟล์ไม่ได้

จะเห็นว่าใน data.odt มี user paul เป็น user owners และมี group owner คือ group snooker

### 3. แสดงผู้ใช้ทั้งหมด

    paul@debian7~$ cut -d: -f1 /etc/passwd | column

### 4. เปลี่ยน group owner และ user owner  

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


[See anohter file command options here!](https://phoenixnap.com/kb/linux-file-command)


    
# File System Hierarchy Standard (FHS)
FHS (File System Hierachy Standard) เป็นโครงสร้าง file system แบบ tree โดยเริ่มจาก root directory ไล่ลงมาเป็น system directory และ user directory โดยใช้เพื่อการนิยามชื่อ ที่อยู่ และสิทธิ์การเข้าถึงของไฟล์ชนิดต่างๆและ Directory เป็นแบบลำดับขั้น (hierarchy) หากต้องการเข้าถึงทุก directory ต้องเข้าถึงในฐานะ root user หรือผู้ใช้ที่มีสิทธิ์สูงสุด

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

  โดยทั่วไปใช้เป็นจุดติดตั้งสำหรับระบบไฟล์ที่ติดตั้งเพิ่มเติม
  *การ mount ใน linux คือการทำให้ระบบของ linux มีความพร้อมในการอ่าน/เขียนไฟล์ที่ถูกติดตั้งเข้ามาใหม่ได้


- **The /opt/Diectory (opt : optional)**

  เก็บพวก package ที่ติดตั้งมาจากภายนอก


- **The /proc/Directory (proc : process)**

  เป็นไฟล์พิเศษที่ถูกสร้างขึ้นใน system memory โดยจะไม่มีการปรากฏอยู่บน disk โดยในส่วนนี้จะประกอบด้วยข้อมูลสำคัญเกี่ยวกับสถานะของระบบ  ซึ่งมีไว้ควบคุมและจัดการทรัพยากรต่างใน kernal โดย /proc ต้องมีการ mounted ก่อน


- **The /sbin/Directory (sbin : system binary)**

  เก็บโปรแกรมที่สามารถให้ผู้ใช้ระดับสิทธิ์สูงสุด (root user) เรียกใช้ได้ โปรแกรมใน /sbin/ นี้จะถูกใช้งานเฉพาะในขณะที่ระบบกำลังทำการบูตและทำงานในกระบวนการซ่อมแซมหรือกู้คืนระบบ


- **The /srv/Directory (sys : system)**

  เก็บข้อมูลที่เฉพาะเจาะจงสำหรับบริการที่ระบบของผู้ใช้กำลังให้บริการ ตัวอย่างเช่น FTP, WWW, หรือ CVS. ไดเรกทอรีนี้จะแสดงตำแหน่งของไฟล์ข้อมูลสำหรับบริการที่ระบบบริการนั้น ๆ โดยเฉพาะ


- **The /sys/Directory (sys : system)**

  ใช้ระบบไฟล์เสมือน (virtual file system) แบบใหม่ที่เรียกว่า sysfs มีเฉพาะกับ karnel เวอร์ชัน 2.6 กับการ support จากอุปกรณ์  hot plug ในเคอร์เนลเวอร์ชัน 2.6, ไดเรกทอรี /sys/ ประกอบด้วยข้อมูลที่คล้ายกับที่อยู่ใน /proc/ แต่แสดงผลในรูปแบบลำดับชั้นข้อมูลเฉพาะเกี่ยวกับอุปกรณ์  hot plug .


- **The /usr/Directory**

  ประกอบด้วยไดเรกทอรีย่อยมากมายที่สำคัญเพื่อรองรับผู้ใช้ โดยให้แอปพลิเคชัน, ไลบรารี, และเอกสาร.


- **The /var/Directory (var: variable)**

  ใช้เก็บ subdirectory สำหรับงานที่ไฟล์มีการเปลี่ยนแปลงบ่อย


[//]: # ()
[//]: # (| Comment |)

[//]: # (|---------|)

[//]: # (| koako   |)

# Reference 

- [The Complete Reference LInux Sixth Edition](https://doc.lagout.org/operating%20system%20/linux/Linux%20-%20The%20Complete%20Reference.pdf?fbclid=IwAR07KOfQrR5c1Rd2Vrcew7x8vSd_QI-79OQNH7jnA_grvO_osKb-6V_1740)
- [greeksforgeeks](https://www.geeksforgeeks.org/linux-directory-structure/)
- [linuxiosys](https://linuxopsys.com/topics/file-types-in-linux)
- [javapoint](https://www.javatpoint.com/linux-files#:~:text=In%20Linux%20system%2C%20everything%20is,Files%20are%20always%20case%20sensitive)
- [redhat](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/4/html/reference_guide/s1-filesystem-fhs)
