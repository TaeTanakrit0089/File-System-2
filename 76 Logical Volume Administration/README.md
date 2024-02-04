# Logical Volume Administration

การใช้ **Physical Storage** ตรงๆ นั้นอาจจะไม่ยืดหยุ่น และจัดการได้ยาก เช่น มี SSD 128GB 3 อัน ต้องการให้ User1 กับ User2
ใช้คนละครึ่งกัน ทำยังไง?<br><br> ด้วยปัญหานี้ เราจึงต้องสร้าง **Logical Volume** ขึ้นมา เพื่อให้เราสามารจัดการกับ
**Physical Storage** ได้ง่ายขึ้น <br><br>

# Index

- [Logical Volume คืออะไร ?](#logical-volume-คืออะไร)
- [Logical Volume Management (LVM) in Linux](#logical-volume-management-in-linux)
- [ส่วนประกอบของ LVM](#ส่วนประกอบของ-lvm)
- [การสร้าง Logical Volume](#การสร้าง-logical-volume)
- What is Snapshot

## Logical Volume คืออะไร ?

**Logical Volume** คือ Storage ที่ถูกสร้างขึ้นด้วย Software จากการนำเอา **Physical Storage** มารวมกัน เป็น **Volume
Group** จากนั้นนำมาแบ่งสร้างเป็น **Logical Volume** ดังภาพ

<img src="https://miro.medium.com/v2/format:webp/0*AfmQ4EWxlDsXJbFQ.png">

https://miro.medium.com/v2/format:webp/0*AfmQ4EWxlDsXJbFQ.png

### ข้อดีของ Logical Volume

- ยืดหยุ่นมากกว่าการใช้ **Physical Volume** ตรงๆ ปรับขนาด เพิ่มลดได้ตามอิสระ

- สามารถทำ **Backups** ได้เช่น การทำ Snapshot, Mirroring และ striping

## Logical Volume Management in Linux

**Linux** มีสิ่งที่เรียกว่า **Logical Volume Management (LVM)** มีความสามารถในการทำและจัดการ **Logical Volume**
<br>
<br>
<img  src="https://www.cyberciti.biz/media/new/faq/2018/08/Shows-information-about-available-LVM-logical-volumes.png">
<br>
https://www.cyberciti.biz/media/new/faq/2018/08/Shows-information-about-available-LVM-logical-volumes.png

## ส่วนประกอบของ LVM

**ส่วนประกอบหลักๆ ของ LVM จะมีอยู่ 3 ส่วนคือ**

**Physical Volume (PV)** ในส่วนนี้ก็คือส่วนของฮาร์ดดิสก์จริงๆ ที่เราจะใช้ในการเก็บข้อมูล
เราสามารถใช้ฮาร์ดดิสก์เพื่อทำเป็น Physical Volume ได้สองแบบ แบบแรกใช้ทีเดียวทั้งก้อนเลยเช่นทั้งก้อน /dev/sda
หรือจะเป็นแบบที่สองคือทำทีละ disk partitionเช่น /dev/sda1, /dev/sda2 ตามคำเอกสาร LVM HOWTO
แล้วเขาแนะนำเป็นแบบที่สองคือแบ่งเป็น partition ก่อนแล้วค่อยทำเป็น **Physical Volume**

**Volume Group (VG)** จะทำหน้าที่รวบรวม Physical Volume ต่างๆ เข้าด้วยกันเพื่อมองเป็นก้อนๆ เดียว เช่นรวม /dev/sda1,
/dev/sdb1, /dev/sdc1 ซึ่งทำถูกทำเป็น Physical Volume แล้ว นำมาเข้าด้วยกันเป็น Volume Group ที่ชื่อ VG0

**Logical Volume (LV)** เป็นก้อนย่อยๆ ที่แบ่งมาจาก Volume Group นั่นเอง เช่นเมื่อเราสร้าง Volume Group ที่ชื่อ VG0
ขึ้นมาแล้วเราก็นำมาแบ่งย่อยอีกทีนึงเช่นเป็น LV0_home สำหรับใช้เป็น /home ของระบบ LV0_var สำหรับใช้เป็น /var เป็นตัน

หลังจากแบ่งเป็น **Logical Volume** แล้ว เราก็สามารถนำมา Volume นั้นมา format เป็น filesystem ตามที่เราต้องการได้เช่น
ext2, ext3 เพื่อนำมา mount เป็น /home, /var อีกที

## การสร้าง Logical Volume

ขั้นตอนคร่าว

## References

- https://www.techtarget.com/searchdatacenter/definition/logical-volume-management-LVM

- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/index

- https://medium.com/@songyotemungmai/%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B9%83%E0%B8%8A%E0%B9%89%E0%B8%87%E0%B8%B2%E0%B8%99-lvm-%E0%B9%80%E0%B8%9E%E0%B8%B7%E0%B9%88%E0%B8%AD%E0%B8%88%E0%B8%B1%E0%B8%94%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B8%9E%E0%B8%B7%E0%B9%89%E0%B8%99%E0%B8%97%E0%B8%B5%E0%B9%88%E0%B8%AE%E0%B8%B2%E0%B8%A3%E0%B9%8C%E0%B8%94%E0%B8%94%E0%B8%B4%E0%B8%AA%E0%B8%81%E0%B9%8C-6c127b24ef87
