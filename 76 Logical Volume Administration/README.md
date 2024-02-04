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



### เตรียม Disk Partition เพื่อใช้ทำเป็น LVM

เริ่มต้นเราต้องเลือก parition ที่จะใช้ทำเป็น LVM โดยใช้คำสั่ง fdisk ในการสร้าง ตัวอย่างเช่นสมมติว่าเรามีฮาร์ดดิสก์อยู่สองตัว ซึ่งเป็นตัวใหม่ที่ยังไม่เคยถูกใช้งานเลยต่ออยู่เป็น /dev/sdb และ /dev/sdc เราต้องสร้าง partition ขึ้นมาใหม่แล้วเปลี่ยนชนิดของ partition ให้เป็นแบบ “Linux LVM” ได้ตามตัวอย่างต่อไปนี้

### สร้าง Physical Volume บน Disk Partition

ขั้นตอนนี้เราจะทำการสร้าง Physical Volume ขึ้นมาบน Disk Partition ที่เราเพิ่งสร้างไปคือ /dev/sdb1 และ /dev/sdc1 โดยเราสามารถทำได้โดยใช้คำสั่ง pvcreate ในการสร้างและใช้คำสัง pvdisplay ในการตรวจสอบได้ ตามตัวอย่างต่อไปนี้

คำสั่ง

```
[root@server]# pvcreate /dev/sdb1 /dev/sdc1
  Physical volume "/dev/sdb1" successfully created
  Physical volume "/dev/sdc1" successfully created
```

```
[root@server ~]# pvdisplay
  --- NEW Physical volume ---
  PV Name               /dev/sdb1
  VG Name
  PV Size               80.00 GB
  Allocatable           NO
  PE Size (KByte)       0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               DE2WGx-ENyz-oxlv-jZ20-Le5l-pnvV-GzMeJ2
```

```
--- NEW Physical volume ---
  PV Name               /dev/sdc1
  VG Name
  PV Size               80.00 GB
  Allocatable           NO
  PE Size (KByte)       0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               s0Wuj0-AYVd-jIZa-gmQp-n05o-MinM-xDQKVh
```
### รวม Physical Volume ทำเป็น Volume Group

ขั้นตอนนี้เราจะนำ Physical Volume ที่เราสร้างมารวมกันเป็นก้อนๆ เดียวเป็น Volume Group โดยเราสามารถตั้งชื่อได้เพื่อสะดวกในการดูแลระบบต่อไป ในขั้นตอนนี้เราจะใช้คำสั่ง vgcreate ในการสร้างและ vgdisplay ในการตรวจสอบ ตามตัวอย่างต่อไปนี้

คำสั่ง

```
root@server ~]# vgcreate VG_HOME /dev/sdb1 /dev/sdc1
  Volume group "VG_HOME" successfully created
```

```
[root@server ~]# vgdisplay
  --- Volume group ---
  VG Name               VG_HOME
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               159.99 GB
  PE Size               4.00 MB
  Total PE              40958
  Alloc PE / Size       0 / 0
  Free  PE / Size       40958 / 159.99 GB
  VG UUID               ivjNHc-GdhD-58jD-0b9H-LK8J-pY59-9Lj140
  ```

ตัวอย่างด้านบนนี้จะเป็นการรวม Physical Volume /dev/sdb1 และ /dev/sdc1 รวมเป็น Volume Group เดียวที่ชื่อ VG_HOME

### แบ่ง Volume Group ออกเป็น Logical Volume

เมื่อเราได้ **Volume Group** (จากขั้นตอนที่ 2) แล้ว เราจะนำมาแบ่งออกเป็นส่วนต่างๆ คือ Logical Volume เพื่อนำไปใช้งานอีกที สมมติว่าเราต้องการสร้าง Logical Volume สำหรับทำเป็น /home ขนาด 50GB สามารถทำได้โดยคำสั่งต่อไปนี้

```
ตัวอย่างการสร้าง Logical Volume ขนาด 50GB
[root@fc8-a ~]# lvcreate -L 50G -n LV_HOME VG_HOME
  Logical volume "LV_HOME" created
```




## References

- https://www.techtarget.com/searchdatacenter/definition/logical-volume-management-LVM

- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/index

- https://medium.com/@songyotemungmai/%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B9%83%E0%B8%8A%E0%B9%89%E0%B8%87%E0%B8%B2%E0%B8%99-lvm-%E0%B9%80%E0%B8%9E%E0%B8%B7%E0%B9%88%E0%B8%AD%E0%B8%88%E0%B8%B1%E0%B8%94%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B8%9E%E0%B8%B7%E0%B9%89%E0%B8%99%E0%B8%97%E0%B8%B5%E0%B9%88%E0%B8%AE%E0%B8%B2%E0%B8%A3%E0%B9%8C%E0%B8%94%E0%B8%94%E0%B8%B4%E0%B8%AA%E0%B8%81%E0%B9%8C-6c127b24ef87
