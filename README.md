# FileSystem-2

Computer Organization and Operating System Assignment (Chapter: File System, Sec: 2)

## Overview

## Key Areas of Focus

1. **Files, Directories and FHS:** มาตรฐานระบบไฟล์ (FHS)
   กำหนดโครงสร้างไดเรกทอรีและเนื้อหาในไดเรกทอรีในระบบปฏิบัติการที่คล้ายกับ Unix มันถูกดูแลโดย Linux Foundation
   และกำหนดชื่อ, ตำแหน่ง, และสิทธิ์สำหรับหลายประเภทของไฟล์และไดเรกทอรี
2. **Raw Media Devices:** ในระบบคอมพิวเตอร์เบื้องต้น, Raw Media Devices
   เป็นประเภทของอุปกรณ์ทางตรรกกะคอมพิวเตอร์ที่อนุญาตให้อุปกรณ์จัดเก็บข้อมูลเช่นฮาร์ดดิสก์ไดรฟ์เข้าถึงโดยตรง,
   ข้ามแคชและบัฟเฟอร์ของระบบปฏิบัติการ
3. **Physical Volume Administration:** หน่วยจัดเก็บข้อมูลทางกายภาพพื้นฐานของ LVM (Logical Volume Manager)
   ซึ่งสามารถเป็นอุปกรณ์บล็อกเช่นพาร์ทิชันหรือฮาร์ดดิสก์ทั้งหมด
4. **Volume Group Administration:**  กลุ่มของพื้นที่เสมือนในฮาร์ดดิสก์ถูกสร้างขึ้นมาโดยการแบ่งดิสก์ทางกายภาย (Physical
   Volume) ออกเป็นส่วนๆ สร้างพื้นที่ในส่วนของฮาร์ดดิสก์ที่สามารถจัดการส่วนของ Logical Volume ได้
5. **Logical Volume Administration:** เป็นการจัดการจัดการการแบ่งฮาร์ดดิสก์ทางตรรกะ (Logical Volumes)
   เปรียบเสมือนเป็นการสร้างกล่องจัดเก็บข้อมูลจำลองที่ระบบไฟล์ (File System), ระบบฐานข้อมูล (DMBS) หรือแอปพลิเคชั่นต่างๆ
   สามารถเข้าถึงได้ซึ่งการจัดการเหล่านี้จัดการโดย Volume Group
6. **File System Type:**
   ระบบไฟล์คือวิธีและโครงสร้างของข้อมูลที่ระบบปฏิบัติการใช้ในการควบคุมวิธีการจัดเก็บและเรียกคืนข้อมูล
   โดยจะมีระบบไฟล์หลายประเภทซึ่งแต่ละประเภทมีโครงสร้างและการทำงานเป็นของตัวเองเช่น NTFS, FAT32, exFAT, และ EXT2/2/4
7. **Archiver, Backup/Restore Tools:** เป็นเครื่องมือซอฟต์แวร์ทีสร้างและจัดการการสำรองข้อมูลของคุณและกู้คืนของข้อมูล

## Contributors

| ID       | Name                            | Sub Topics                     | Img                                                                              |
|----------|---------------------------------|--------------------------------|----------------------------------------------------------------------------------|
| 65070001 | นางสาวกชกร ครุธเวโช             | Files, Directories and FHS     | <img alt="PraeMai" height="150" src="/assets/img/members/001.webp" width="150"/> |
| 65070018 | นายกิตติ์ชินทักษ์ หรรษานนท์โชติ | Raw Media Devices              | <img alt="Win" height="150" src="/assets/img/members/018.webp" width="150"/>     |
| 65070028 | นายคณิศร สมศรีอักษรแสง          | Physical Volume Administration | <img alt="Best" height="150" src="/assets/img/members/028.webp" width="150"/>    |
| 65070035 | นายจิรโชติ อินทรวงษ์โชติ        | Volume Group Administration    | <img alt="Jai" height="150" src="/assets/img/members/035.webp" width="150"/>     |
| 65070076 | นายณัฐนนท์ วงศ์หนองเเวง         | Logical Volume Administration  | <img alt="Nont" height="150" src="/assets/img/members/076.webp" width="150"/>    |
| 65070078 | นายณัฐพงศ์ มาสำราญ              | File System Type               | <img alt="James" height="150" src="/assets/img/members/078.webp" width="150"/>   |
| 65070089 | นายธนกฤต ทรัพย์ประสิทธิ์        | Archiver, Backup/Restore Tools | <img alt="Tae" height="150" src="/assets/img/members/089.webp" width="150"/>     |

[//]: # (![GroupMembers]&#40;/assets/img/members/group-members.jpeg&#41;)

## นำเสนอ

ผู้ช่วยศาสตราจารย์ ดร. สุเมธ ประภาวัต
อาจารย์ประจำคณะเทคโนโลยีสารสนเทศ สถาบันเทคโนโลยีพระจอมเกล้าเจ้าคุณทหารลาดกระบัง
<br>
<img style="margin: 0 auto" alt="Sumet" src="/assets/img/members/Sumet-200x200.png"/>
