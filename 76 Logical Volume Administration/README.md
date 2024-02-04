# Logical Volume Administration

การใช้ **Physical Storage** ตรงๆ นั้นอาจจะไม่ยืดหยุ่น และจัดการได้ยาก เช่น มี SSD 128GB 3 อัน ต้องการให้ User1 กับ User2
ใช้คนละครึ่งกัน ทำยังไง?<br><br> ด้วยปัญหานี้ เราจึงต้องสร้าง **Logical Volume** ขึ้นมา เพื่อให้เราสามารจัดการกับ
**Physical Storage** ได้ง่ายขึ้น <br><br>

# Index

- [Logical Volume คืออะไร ?](#logical-volume-คืออะไร)
- [Logical Volume Management (LVM) in Linux](#logical-volume-management-in-linux)
- [ส่วนประกอบของ LVM](#ส่วนประกอบของ-lvm)
- [การสร้าง Logical Volume](#การสร้าง-logical-volume)
- [Striped Logical Volume](#striped-logical-volume)
- [Mirror Logical Volume](#mirror-logical-volume)
- [การทำ Snapshot](#การทำ-snapshot)
- [RAID คืออะไร](#redundant-array-of-independent-disks-raid)
- [References](#references)

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


> [!WARNING]
> การสร้าง Logical Volume นั้นต้องมี Volume Group ก่อน <br>
สามารถย้อนอ่านได้ที่หน้า [การสร้าง Volume Group](https://github.com/TaeTanakrit0089/FileSystem-2/tree/main/35%20Volume%20Group%20Administraton#%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B8%AA%E0%B8%A3%E0%B9%89%E0%B8%B2%E0%B8%87-volume-group)

สร้าง **Logical Volume** ขนาด 10 GB ใน **Volume Group** vg1

```
# lvcreate -L 10G vg1
```

หน่วยวัดพื้นที่ ที่ใช้สร้าง **Logical Volume** โดยพื้นฐานแล้วคือ **megabytes**
คำส่งต่อไปนี้หมายถึง สร้าง **LV** ขนาด 1500 mb ใน **VG** testvg

```
# lvcreate -L 1500 testvg
```

สร้าง **Logical Volume** ขนาด 50 GB ใน **Volume Group** VG_HOME โดยตั้งชื่อ **Logical Volume** ว่า LV_HOME

```
# lvcreate -L 50G -n LV_HOME VG_HOME
    Logical volume "LV_HOME" created
```

เราสามารถกำหนดพื้นที่โดยวัดจาก % ของพื้นที่ที่เหลือได้ ตัวอย่างนี้คือ100% ของพื้นที่ที่เหลือ
```
lvcreate -l 100%FREE -n data vgdata
```
นอกจากนี้สามารถกำหนดเป็น % ของ **Volume Group** (VG) หรือ **Physical Volumes** (PV) ทั้งหมดใน **Volume Group** ได้

```
lvcreate -l 50%VG -n data vgdata
```

```
lvcreate -l 50%PV -n data vgdata
```


คำสั่งต่อไปนี้ใช้แสดง **Logical Volume** ในเครื่อง
```
[root@fc8-a ~]# lvdisplay
  --- Logical volume ---
  LV Name                /dev/VG_HOME/LV_HOME
  VG Name                VG_HOME
  LV UUID                VldIjI-LyUr-raDW-Of5Z-Hwes-Sl1P-Hu2juQ
  LV Write Access        read/write
  LV Status              available
  # open                 0
  LV Size                50.00 GB
  Current LE             12800
  Segments               1
  Allocation             inherit
  Read ahead sectors     0
  Block device           253:0
```

## Striped Logical Volume

**Striped Logical Volumes** หรือ [**RAID 0**](#เพิ่มเติม-redundant-array-of-independent-disks-raid) เป็นเทคนิคการกระจายข้อมูลใน Physical Volume หลายตัวเพื่อใช้ประโยชน์จากความสามารถในการเข้าถึงข้อมูลแบบขนานกัน ช่วยเพิ่มประสิทธิภาพการ I/O ของ **Logical Volume**

![](https://www.prepressure.com/images/raid-level-0-striping.svg)


![](https://www.tecmint.com/wp-content/uploads/2014/09/LVM-Striping.jpeg)
https://www.tecmint.com/wp-content/uploads/2014/09/LVM-Striping.jpeg


สมมุติว่าเรามี **Physical Volume** 4 อัน หากเขียนข้อมูลแบบธรรมดา ก็จะเขียนแค่ **Physical Volume** อันแรกเท่านั้น

ถ้าหากเราใช้แบบวิธีการแบบ **Striped** จะเป็นการเขียนข้อมูลพร้อมกันทั้ง 4 Physical Volume
ด้วยเทคนิค จะทำให้ Performance ของ I/O เพิ่มขึ้นอย่างมาก

คำสั่งการสร้าง **Striped Logical Volume**

```
  # lvcreate -L 900M -n lv_tecmint_strp1 -i4 vg_strip
```
1. -L = logical volume size
2. -n = logical volume name
3. -i = stripes

สร้าง LV ขนาด 900MB ชื่อว่า lv_tecmint_strp1 กำหนดให้เป็น Striped โดยใช้ Physical Volume 4 อัน สร้าง LV ใน vg_strip

### ข้อเสียของการใช้ Striped Logical Volume
- **ความเสี่ยงของข้อมูลสูญหาย:** หาก **Physical Volume** ตัวใดตัวหนึ่งล้มเหลว ข้อมูลใน **Logical Volume** นั้นจะอ่านเขียนไม่ได้
  <br><br>
- **ความซับซ้อนในการจัดการ:** **Striped Logical Volume** มีความซับซ้อนในการจัดการมากกว่า **Logical Volume** แบบธรรมดา (เพราะหากมีการเพิ่ม ลด ขนาดของ **Logical Volume** อาจทำให้ข้อมูลเกิดความผิดพลาดได้ )

## Mirror Logical Volume

**Mirror Logical Volume** หรือ [**RAID 1**](#เพิ่มเติม-redundant-array-of-independent-disks-raid)  เป็นเทคนิคที่สร้างสำเนาข้อมูลสำรองบน Physical Volume หลายตัว ซึ่งช่วยป้องกันข้อมูลสูญหายในกรณีที่ Physical Volume ตัวใดตัวหนึ่งล้มเหลว
<br><br>
![](https://www.salvagedata.com/wp-content/uploads/2022/01/raid-1-configuration_-SALVAGEDATA-recovery-infographic.svg)

เช่น หากเขียนข้อมูลลงบน Mirror Logical Volume ข้อมูลนั้นจะถูกสร้างสำเนาและเขียนลงบน Physical Volume อีกหลายตัว



คำสั่งการสร้าง **Mirror Logical Volume**

```
# lvcreate --type mirror -L 50G -m 1 -n mirrorlv vg0
```
สร้าง Logical Volume<br>
`--type mirror`
กำหนด type เป็น mirror<br>
`-L 50G`
มีพื้นที่ 50GB<br>
`-m 1`
สร้างสำเนา 1 ชุด (จะมีข้อมูลชุดนี้อยู่ 2 ที่ ต้นฉบับและสำเนา) <br>
`-n mirrrorlv`
ชื่อ Logical Volume นี้ คือ mirrorlv <br>
`vg0` สร้างใน **Volume Group** vg0


### ข้อเสียของการใช้ Mirror Logical Volume
- ใช้พื่้นที่จัดเก็บข้อมูลเพิ่มขึ้นหลายเท่าตัว ขึ้นอยู่กับว่าทำสำเนากี่ฉบับ

- ประสิทธิภาพช้าลง เพราะต้องเขียนข้อมูลอันเดียวกันหลายที่

- ไม่เหมาะกับข้อมูลที่เปลี่ยนแปลงบ่อย เพราะใช้ทรัพยากรในการเขียนข้อมูลมาก
## การทำ Snapshot

**Snapshot** คือ การสร้างสำเนาของ **Logical Volume (LV)** ณ ช่วงเวลาใดเวลาหนึ่ง เปรียบเสมือนการถ่ายภาพของ **Logical Volume** ในขณะนั้น โดยเราสามารถย้อนกลับไปยัง **Logical Volume** ณ ช่วงเวลานั้นได้ มักใช้กันในการทำ Backups หรือ Rollback

![](https://devconnected.com/wp-content/uploads/2020/02/featured-2.png)

วิธีการทำ **Snapshot**


คำสั่ง

```
lvcreate -s <snapshot_name> <logical_volume>
```

```
lvcreate -s mysnapshot lv1
```
สร้าง **Snapshot** โดยใช้คำสั่ง **lvcreate** กับตัวเลือก -s ตั้งชื่อ mysnapshot (ไม่มี -n เพราะการสร้าง snaphshot จะตั้งชื่อตามที่พิมพ์เข้าไปเลย) และ ทำ **snapshot** ใน lv1 (Logical Volume)

**รายละเอียดเพิ่มเติม**
- **Snapshot** จะใช้พื้นที่เก็บข้อมูลเท่ากับ **Logical Volume** ที่ถูก **Snapshot**
- สามารถสร้าง **Snapshot** หลาย **Snapshot** ของ **Logical Volume** เดียวกันได้
- **Snapshot** จะถูกเก็บไว้จนกว่าจะลบออก

**ตัวอย่างการทำ Snapshot เพิ่มเติม**
```
lvcreate -s mysnapshot -L 10G lv1
```
การสร้าง **snapshot** ของ **Logical Volume** ที่ชื่อว่า "lv1" และตั้งชื่อ **snapshot** ว่า "mysnapshot" โดยจำกัดขนาดไว้ที่ 10GB (กรณีที่ข้อมูลเกินพื้นที่ๆ กำหนดไว้ ข้อมูลบางส่วนอาจสูญหาย)


### การใช้งาน Snapshot

- **การกู้คืน snapshot:**
```
lvrestore mysnapshot
```
คำสั่งนี้จะกู้คืน snapshot "mysnapshot" ไปยัง Logical Volume "lv1"

- **การลบ snapshot:**
```
lvremove mysnapshot
```
คำสั่งนี้จะลบ Snapshot "mysnapshot"

## Redundant Array of Independent Disks (RAID)
**RAID** คือ เทคโนโลยีที่นำ **Hard Disk** ตั้งแต่ 2 ลูกขึ้นไปมาต่อเข้าด้วยกันเพื่อให้มองเห็นเป็นก้อนเดียวกัน โดยประโยชน์หลักๆ ของการทำ **RAID** ก็คือการป้องกันการเสียหายของข้อมูล และเป็นการเพิ่มประสิทธิภาพในการอ่าน/เขียนของข้อมูล

การทำ **RAID** นั้นมีมากมายหลายแบบ แต่จะยกตัวอย่างที่นิยมกันคือ RAID 0 , RAID 1 , RAID 5 , RAID 6

### RAID 0 – Striping

<img height="300px" src="https://addin.co.th/blog/wp-content/uploads/2020/05/article-raid-three.png"> <br>
คือการนำ Hard disk ตั้งแต่ 2 ลูกขึ้นไปมารวมกัน โดยจะเป็นการแบ่งเขียนข้อมูลไว้ในแต่ละลูก โดยไม่ได้มีการเขียนข้อมูลซ้ำตามรูปด้านล่าง ดังนั้นข้อดีของการทำ RAID 0 ก็ คือเราจะมองเห็น Harddisk เป็นก้อนเดียวกันและทำให้ได้ Performance ในการอ่านและเขียน แต่ข้อเสีย ถ้าหาก Disk ใดใน Group RAID นี้เสีย จะทำให้ข้อมูลเสียทั้งหมด

ตัวอย่างการคำนวณถ้าเรามี HDD 1TB จำนวน 2 ลูกทำ RAID 0 กันจะได้พื้นที่ทั้งหมด
(1TB + 1TB) = 2 TB



### RAID 1 – Mirroring

<img height="300px" src="https://addin.co.th/blog/wp-content/uploads/2020/05/article-raid-four.png"> <br>

คือการนำ Hard disk ตั้งแต่ 2 ลูกขึ้นไปมาทำ RAID ด้วยกัน คือ จะเป็นการเขียนข้อมูลลงบน Hard disk ทั้งสองชุดดังตัวอย่างในรูปข้อมูลชุด A1 ถูกเขียนลงบนทั้ง Disk 0 และ Disk 1 ข้อดีก็คือในแง่ความปลอดภัย Hard disk สามารถเสียได้ ครึ่งหนึ่งโดยที่ข้อมูลที่เราเขียนไว้บน Disk ไม่หายไป ข้อเสียก็คือจะเสียพื้นที่ Hard Disk ครึ่งหนึ่งเนื่องจากเป็นการเขียนข้อมูลลงบนทั้งสองฝั่ง

ตัวอย่างการคำนวณถ้าเรามี HDD 1TB จำนวน 2 ลูกทำ RAID 1 กันจะได้พื้นที่ทั้งหมด
(1TB + 1TB)/2 = 1 TB


### RAID 5 – Striping with Parity
<img height="300px" src="https://addin.co.th/blog/wp-content/uploads/2020/05/article-raid-five.png"> <br>
คือการนำ Hard Disk ตั้งแต่ 3 ลูกขึ้นไปมารวมกัน โดยจะเป็นการเขียนข้อมูลกระจายไปทั่วทุก Hard Disk และมี Parity ในชุดนี้ 1 ลูกเพื่อกู้ข้อมูลกรณี Hard Disk ในชุดนี้เสียดังรูป ข้อมูลชุด A ถูกเขียนกระจายไปบน Disk 0,1,2 และมี Parity อยู่บน Disk 3 ข้อดีคือในชุด RAID นี้ Hard Disk สามารถเสียได้สูงสุด 1 ลูกโดยที่ข้อมูลไม่เสียหาย ข้อเสียก็คือจะเสียพื้นที่ Hard Disk ไป 1 ลูก

ตัวอย่างการคำนวณถ้าเรามี HDD 1TB จำนวน 3 ลูกทำ RAID 5 กันจะได้พื้นที่ทั้งหมด
1TB x (3-1) = 2 TB

### RAID 6 – Striping with Double Parity
<img height="300px" src="https://addin.co.th/blog/wp-content/uploads/2020/05/article-raid-six.png"> <br>
คือการนำ Hard Disk ตั้งแต่ 4 ลูกขึ้นไปมารวมกัน โดยจะเป็นการเขียนข้อมูลกระจายไปทั่วทุก Hard Disk และมี Parity ในชุดนี้ 2 ลูกเพื่อกู้ข้อมูลกรณี Hard Disk ในชุดนี้เสียดังรูป ข้อมูลชุด A ถูกเขียนกระจายไปบน Disk 0,1,2 และมี Parity อยู่บน Disk 3,4 ข้อดีคือในชุด RAID นี้ คือ Hard Disk สามารถเสียได้สูงสุด 2 ลูกโดยที่ข้อมูลไม่เสียหาย ข้อเสียก็คือจะเสียพื้นที่ Hard Disk ไป 2 ลูก

ตัวอย่างการคำนวณถ้าเรามี HDD 1TB จำนวน 4 ลูกทำ RAID 6 กันจะได้พื้นที่ทั้งหมด
1TB x (4-2) = 2 TB

## References

- https://www.techtarget.com/searchdatacenter/definition/logical-volume-management-LVM

- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/index

- https://medium.com/@songyotemungmai/%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B9%83%E0%B8%8A%E0%B9%89%E0%B8%87%E0%B8%B2%E0%B8%99-lvm-%E0%B9%80%E0%B8%9E%E0%B8%B7%E0%B9%88%E0%B8%AD%E0%B8%88%E0%B8%B1%E0%B8%94%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B8%9E%E0%B8%B7%E0%B9%89%E0%B8%99%E0%B8%97%E0%B8%B5%E0%B9%88%E0%B8%AE%E0%B8%B2%E0%B8%A3%E0%B9%8C%E0%B8%94%E0%B8%94%E0%B8%B4%E0%B8%AA%E0%B8%81%E0%B9%8C-6c127b24ef87

- https://www.tecmint.com/manage-multiple-lvm-disks-using-striping-io/

- https://devconnected.com/lvm-snapshots-backup-and-restore-on-linux/

- https://addin.co.th/blog/what-is-raid/