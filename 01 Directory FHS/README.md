# File

ใน Linux System ทุกอย่างคือไฟล์ ไม่ว่าจะเป็น Partition, Hardware, อุปกรณ์ต่างๆ, driver และ directories 

การดำเนินการต่างๆเกี่ยวกับไฟล์มีหลายเรื่องที่ต้องคำนึงมากมาย กรณีที่ตั้งชื่อไฟล์เหมือนกันแต่ต่างกันที่ตัวพิมพ์เล็กหรือตัวพิมพ์ใหญ่  ก็คือว่าทั้งสองไฟล์นั้นเป็นคนล่ะไฟล์กัน

## ประเภทของไฟล์
#### 1. Regular files
   เป็นไฟล์ทั่วไปพวก media file, program และ executable file ไฟล์ชนิดนี้สามารถอยู่ในรูปของ ASCII หรือ Binary

#### 2. Directories
   สำหรับ linux ถือเป็นไฟล์ที่ใช้ในการจัดเก็บไฟล์อื่นๆและ subdirectories

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /home/ubuntu/ | grep ^d

#### 3. Special files หรือ Device files 
   เป็นพวกไฟล์อุปกรณ์ที่เชื่อมโยงกับอุปกรณ์ฮาร์ดแวร์ในระบบ สามารถแบ่งเป็น 5 ประเภท ดังนี้

- Block file (b) : เป็นไฟล์ที่ทำหน้าที่เป็น direct interface สำหรับ block devices เช่น Hard drive โดยเป็นไฟล์ที่แทนอุปกรณ์ที่ถ่ายโอนข้อมูลเป็น block โดยไฟล์เหล่านี้จะถูกเก็บอยู่ใน /dev

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^b

- Character device file (c) :
  เป็น hardware file ที่อ่าน/เขียนข้อมูลทีล่ะ 1 ตัวอักษร โดยใช้รุปแบบในการ input/output แบบ serial stream และให้การเข้าถึงโดยตรงกับ hardware ตัวอย่าง hardware เช่น terminal, Serial port

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^c

- Named pipe file หรือ just a pipe file (p) : ชื่อของ named pipe เป็นชื่อไฟล์ในระบบไฟล์ โดยทำหน้าที่ส่งข้อมูลจาก process หนึ่งไปยังอีก process

_คำสั่งที่ใช้ในการดูไฟล_์

    ls -l /dev | grep ^p

- Symbolic link file (l) : ทำหน้าที่ชี้ไปยังไฟล์หรือ folder อื่นๆ ทำให้มีความยืดหยุ่นในการใช้ชื่อไฟล์ต่างกันหรืออยู่ใน location ที่ต่างกัน 
ซึ่ง link มีอยู่ 2 ประเภท ดังนี้
   - Hard link สำหรับคัดลอกไฟล์ต้นฉบับ โดยจะไม่สามารถสร้าง directory หรือ file ในระบบไฟล์อื่นได้
   - Soft link สำหรับชี้ไปยังไฟล์ฉบับ ซึ่งสามารถสร้าง directory หรือ file ในระบบไฟล์อื่นได้

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^l

- Socket file (s) :  อนุญาตให้มีการแลกเปลี่ยนข้อมูลโดยไม่ต้องใช้กระบวนการที่ซับซ้อนของเครือข่ายและ sockets โดยใช่ชื่อไฟล์เป็นที่อยู่ แทนการใช้ IP Address และ Port Number

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^s



### วิธีดูประเภทของไฟล์
- File command
คำสั่งนี้จะแสดงเฉพาะชนิดของไฟล์แต่ไม่แสดงชนิดของเนื้อหาข้างใน

_ตัวอย่างคำสั่ง_

    file fail.txt
    file /dev/*

- ls -l command
คำสั่งนี้จะแสดงเนื้อหาภายใน directory ปัจจุบันด้วย โดยตัวอักษรแรกใน list แต่ล่ะอันบอกถึงชนิดของ file

_ตัวอย่างคำสั่ง_
    
    ls -l /dev

- stat command
คำสั่งนี้แสดงข้อมูลของ file system, ขนาดของไฟล์, สิทธิ์การเข้าถึง, user และ group IDs

_ตัวอย่างคำสั่ง_

    stat records.csv

## สรุปเรื่องประเภทของไฟล์


| ชนิด       | คำอธิบาย                                            | คำสั่งที่่ใช้ในการสร้าง  | ที่อยู่ของไฟล์  | ls -l |
|------------|-----------------------------------------------------|---|---|-------|
| Regular    | ประกอบไปด้วย ข้อมูลพวก text, image, video, script   | touch  | directory ไหนก้ได้  | -     |
| Directory  | ประกอบไปด้วยชื่อและที่อยู่ของไฟล์อื่นๆ              | mkdir  | Directory  | d     |
| Character  | เกี่ยวข้องกับการเข้าถึงอุปกรณ์ด้วยตัวอักษร          | mknod  | /dev  | c     |
| pipe       | อนุญาตให้มีการรับส่งข้อมูลระหว่างกัน                | mkfifo  |/dev   | p     |
| Symbol     | เป็นตัวชี้หรือคัดลอกไฟล์อื่นๆ                       | ln  |/dev   | l     |
| Socket     | ให้การแลกเปลี่ยนสื่อสารระหว่างกัน                   | socket() system call  |/dev   | s     |

    
# File System Hierarchy Standard (FHS)
FHS (File System Hierachy Standard) เป็นโครงสร้าง file system แบบ tree โดยเริ่มจาก root directory ไล่ลงมาเป็น system directory และ user directory โดยใช้เพื่อการนิยามชื่อ ที่อยู่ และสิทธิ์การเข้าถึงของไฟล์ชนิดต่างๆและdirectory เป็นแบบลำดับขั้น (hierarchy) หากต้องการเข้าถึงทุก directory ต้องเข้าถึงในฐานะ root

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



| Comment |
|---------|
| koako   |

# Reference 

- [The Complete Reference LInux Sixth Edition](https://doc.lagout.org/operating%20system%20/linux/Linux%20-%20The%20Complete%20Reference.pdf?fbclid=IwAR07KOfQrR5c1Rd2Vrcew7x8vSd_QI-79OQNH7jnA_grvO_osKb-6V_1740)
- [greeksforgeeks](https://www.geeksforgeeks.org/linux-directory-structure/)
- [linuxiosys](https://linuxopsys.com/topics/file-types-in-linux)
- [javapoint](https://www.javatpoint.com/linux-files#:~:text=In%20Linux%20system%2C%20everything%20is,Files%20are%20always%20case%20sensitive)
- [redhat](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/4/html/reference_guide/s1-filesystem-fhs)**
