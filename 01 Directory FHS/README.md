# File

ใน Linux System ทุกอย่างคือไฟล์ ไม่ว่าจะเป็น Partition, Hardware, อุปกรณ์ต่างๆ, driver และ directories

การดำเนินการต่างๆเกี่ยวกับไฟล์มีหลายเรื่องที่ต้องคำนึงมากมาย
กรณีที่ตั้งชื่อไฟล์เหมือนกันแต่ต่างกันที่ตัวพิมพ์เล็กหรือตัวพิมพ์ใหญ่ ก็คือว่าทั้งสองไฟล์นั้นเป็นคนล่ะไฟล์กัน

## ประเภทของไฟล์

#### 1. Regular files

เป็นไฟล์ทั่วไปพวก media file, program และ executable file ไฟล์ชนิดนี้สามารถอยู่ในรูปของ ASCII หรือ Binary

#### 2. Directories

สำหรับ linux ถือเป็นไฟล์ที่ใช้ในการจัดเก็บไฟล์อื่นๆและ subdirectories

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /home/ubuntu/ | grep ^d

#### 3. Special files หรือ Device files

เป็นพวกไฟล์อุปกรณ์ที่เชื่อมโยงกับอุปกรณ์ฮาร์ดแวร์ในระบบ สามารถแบ่งเป็น 5 ประเภท ดังนี้

- Block file (b) : เป็นไฟล์ที่ทำหน้าที่เป็น direct interface สำหรับ block devices เช่น Hard drive
  โดยเป็นไฟล์ที่แทนอุปกรณ์ที่ถ่ายโอนข้อมูลเป็น block โดยไฟล์เหล่านี้จะถูกเก็บอยู่ใน /dev

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^b

- Character device file (c) :
  เป็น hardware file ที่อ่าน/เขียนข้อมูลทีล่ะ 1 ตัวอักษร โดยใช้รุปแบบในการ input/output แบบ serial stream
  และให้การเข้าถึงโดยตรงกับ hardware ตัวอย่าง hardware เช่น terminal, Serial port

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^c

- Named pipe file หรือ just a pipe file (p) : ชื่อของ named pipe เป็นชื่อไฟล์ในระบบไฟล์ โดยทำหน้าที่ส่งข้อมูลจาก process
  หนึ่งไปยังอีก process

_คำสั่งที่ใช้ในการดูไฟล_์

    ls -l /dev | grep ^p

- Symbolic link file (l) : ทำหน้าที่ชี้ไปยังไฟล์หรือ folder อื่นๆ ทำให้มีความยืดหยุ่นในการใช้ชื่อไฟล์ต่างกันหรืออยู่ใน
  location ที่ต่างกัน
  ซึ่ง link มีอยู่ 2 ประเภท ดังนี้
    - Hard link สำหรับคัดลอกไฟล์ต้นฉบับ โดยจะไม่สามารถสร้าง directory หรือ file ในระบบไฟล์อื่นได้
    - Soft link สำหรับชี้ไปยังไฟล์ฉบับ ซึ่งสามารถสร้าง directory หรือ file ในระบบไฟล์อื่นได้

_คำสั่งที่ใช้ในการดูไฟล์_

    ls -l /dev | grep ^l

- Socket file (s) :  อนุญาตให้มีการแลกเปลี่ยนข้อมูลโดยไม่ต้องใช้กระบวนการที่ซับซ้อนของเครือข่ายและ sockets
  โดยใช่ชื่อไฟล์เป็นที่อยู่ แทนการใช้ IP Address และ Port Number

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

# File System Hierarchy Standard (FHS)

**Computer** Organization

| Comment |
|---------|
| koako   |

    Refference 
    - File
    https://www.geeksforgeeks.org/linux-directory-structure/
    https://linuxopsys.com/topics/file-types-in-linux
    https://www.javatpoint.com/linux-files#:~:text=In%20Linux%20system%2C%20everything%20is,Files%20are%20always%20case%20sensitive.
    