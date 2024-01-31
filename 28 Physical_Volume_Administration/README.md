# Physical Volumes

เป็นการจัดการ **Physical Volume** ทั้งหลายใน Hardisk สามารถที่จะ สร้าง, เพิ่ม/ลดขนาด, ลบ, ป้องกันการเขียนข้อมูล ของ **Physical Volume** ได้ และสามารถใช้ในการนำไปทำ **Volume Group Administrator** หรือ **Logical Volume Administrator**
ต่อได้ แต่ก่อนอื่นเราต้องสร้าง และกำหนด **Physical Volume** ขึ้นมาก่อน

## Physical Component

หน่วยที่เก็บข้อมูลกายภาพของ LVM Logical Volume นั้นเป็น Block Device โดยอาจเป็น Partition หรือ Disk ทั้งหมด, เพื่อให้
Device ใช้ LVM Logical Volume ได้นั้น ตัว Device ต้องเริ่มระบุเป็น **Physical Volume (PV)**, การเริ่มกำหนด Block Device
หรือก็คือ **Physical Volume** นั้นเป็นการเริ่มการทำงานตอน Device

โดยการทำงานปกติ LVM Label จะอยู่ใน 512-byte Sector ตัวที่สอง โดยสามารถที่จะเปลี่ยนการทำงานของมันได้ โดยการวาง Label ลงบน
Sector ไหนก็ได้ของ 4 Sectors แรก เมื่อต้องการจะสร้าง Physical Volume โดยสามารถให้ผู้ใช้คนอื่นใน Sectors สามารถใช้ LVM
volumes ร่วมกันได้ หากต้องการได้

ตัว LVM Label ช่วยให้ระบุตัวตนที่ถูกและเรียงอุปกรณ์ให้ Physical Device เพราะ Devices สามารถอยู่ลำดับอะไรก็ได้
เมื่อตอนที่ระบบถูกบูท ตัว LVM Label ยังคงเดิมแม้จะโดนรีบูท หรือผ่าน Cluster

LAM Label ช่วยระบุ Device ของ LVM Physical Volume ได้, มันมี Random Unique Identifier (UUID) ของ Physical Volime
ในการระบุตัวตน, มันยังเก็บขนาดของ Block Device แบบ Bytes, และยังเก็บที่อยู่ของ LVM Metadat ที่จะถูกเก็บใน Device ด้วย

LVM Metadata เก็บรายละเอียดการตั้งค่าของ LVM Volume Groups บนระบบให้, โดยปกติจะมีสำเนาของ Metadata อยู่ในทุก ๆ Metadata
Area ในทุก ๆ Physical Volume ข้างใน Volume Group, LVM Metadata เล็กและเก็บรูปแบบ ASCII

## LVM Physical Volume Layout

> [!Note]
> ใน Linux kernel, จะให้แต่ละ Sectors นั้นมีขนาด 512 bytes

- LVM Label อยู่ใน Sector ที่สอง
- Metadata
- ที่เหลือเป็น พื้นที่ว่าง

  <img alt="" src="https://access.redhat.com/webassets/avalon/d/Red_Hat_Enterprise_Linux-7-Logical_Volume_Manager_Administration-en-US/images/58b3a6c097c618cfcb03163c5cad5d16/physvol.png">

## Multiple Partitions on a Disk

LVM สามารถทำให้สร้าง Physical Volume จาก Disk Partitions, Red Hat แนะนำให้สร้างแค่ Partition ตัวเดียวเพื่อให้คลุม Disk
ได้ทั้งหมดไปเป็น Label ไปเป็น LVM Physical Volume ตามเหตุผลดังนี้

- Administrative convenience<br>
  มันง่ายกว่าในการติดตาม Hardware ในระบบ ถ้าแต่ละ Disk ปรากฏแค่ครั้งเดียว มันจะชัดเจนเมื่อ Disk นั้นผิดพลาด,
  เพิ่มเติมการมีหลาย Physical Volume อยู่ใน Disk เดียวกัน อาจทำให้ Kernel เตือนเกี่ยวกับประเภท Partition
  ที่ไม่รู้จักตอนที่บูท
- Striping performance<br>
  LVM ไม่สามารถบอกได้ว่าสอง Physical Volume อยู่บน Physical Disk เดียวกันหรือป่าว ถ้ามีสอง Physical Volume อยู่ใน
  Physical Disk เดียวกัน การสร้าง Logical Volume ในการแบ่งพวกมันออกจากกัน มันอาจแบ่งให้อยู่คนละ Partition ที่อยู่ Disk
  เดียวกันได้ และอาจส่งผลให้เป็นการลดประสิทธิภาพมากกว่าเพิ่มประสิทธิภาพ

แม้จะไม่แนะนำให้ทำ แต่อาจมีสถานการณ์บางอย่างที่ทำให้จำเป็นต้องแบ่ง Disk ออกจาก LVM Physical Volume ตัวอย่างเช่น
ในระบบที่มี Disk ไม่มากอาจจำเป็นต้องส่งข้อมูลไปมาระหว่าง Partitions เมื่อต้องการที่จะโยกย้ายระบบเดิมไปยัง LVM Volumes
เพิ่มเติมก็คือถ้ามี Disk ขนาดใหญ่มาก และต้องการจะมีมากกว่า 1 Volume Group ในการบริหารงั้นมันก็จำเป็นในการแบ่ง Disk (
Partition) ถ้าต้องการ Disk ที่มีมากกว่า 1 Partition และ Partitions ทั้งคู่ก็อยู่ใน Volume Group เดียวกัน ควรดูแลเฉพาะ
Partition ที่อยู่ใน Logical Volume เมื่อสร้าง Striped Volumes


# Physical Volume Administration
มีหน้าที่ในการจัดการ Physical Volumes ใน Storage Environment หรือก็คือการจัดการ Disk ภายใน Server ใช้ในการตรวจสอบ เพิ่ม ลบ ดูแล

## 1. Creating Physical Volumes
โดยก่อนอื่นต้องทำการตั้งค่า Partition Type ก่อน เพื่อให้สามารถระบุว่า Partition ว่าเป็น Physical Volumes หรือจริง ๆ ก็คือให้ LVM สามารถระบุได้ว่าเป็น Physical Volumes โดยวิธีทำก็คือ
ใช้คำสั่ง `fdisk` หรือ `cfdisk` หรืออื่น ๆ โดยต้องตั้งค่า Partition id เป็น `0x8e` ถ้าต้องการจะให้ทั้ง Disk เป็น Physical Volume ตัว Disk ต้องไม่มี Partition Table สำหรับ DOS Disk Partition, สำหรับทั้ง Disk ต้องมีแค่ Partition Table ที่ต้องถูกล้างข้อมูล โดยจะส่งผลให้เป็นการทำลายข้อมูลทั้งหมดใน Disk นั้น แต่ก็จะสามารถใช้ลบ Partition Table ที่มีอยู่แล้ว โดยการใส่ "ศูนย์" ไปยัง Sector แรกด้วยคำสั่ง

```CIL
# dd if=/dev/zero of=PhysicalVolume bs=512 count=1
```

### 1.1 Initializing Physical Volumes
ใช้คำสั่ง `pvcreate` ในการเริ่มสร้าง Block Device ที่ใช้ในการเป็น Physical Volume การเริ่มต้นจะคล้ายคลึงกับการจัดรูปแบบระบบไฟล์

โดยคำสั่งที่ตามคำสั่งที่ใช้ในการสร้างคือ `/dev/sdd`, `/dev/sde`, และ `/dev/sdf` เป็น LVM Physical Volumes ที่ใช้ในภายหลังเป็นส่วนของ LVM Logical Volumes

```CIL
# pvcreate /dev/sdd /dev/sde /dev/sdf
```

ในการที่ต้องการสร้างเป็น Partitions มากกว่าเป็นทั้ง Disk เริ่ม: รันคำสั่ง `pvcreate` ของ Partition ตามดังตัวอย่างในการเริ่ม Partition `/dev/hdb1` โดยเป็น LVM Physical Volume สำหรับใช้กับส่วนของ LVM Logical Volume

```CIL
# pvcreate /dev/hdb1
```

## 2. Displaying Physical Volumes

โดย **Physical Volumes** ก็เป็นอีกหนึ่ง Component ที่สำคัญในการจัดการ Disk และ

ก่อนจะเข้าไปจัดการ **Physical Volumn** ได้นั้น เราต้องลง **Logical Volumn Manager (LVM)** ใน Linux ก่อน
เพื่อให้ใช้งานคำสั่งได้
แต่ถ้าใช้ Ubuntu Server จะสามารถใช้งาน **LVM** ได้เลย โดยที่เราไม่ต้องลง Package ของ **LVM**

```
apt install lvm2
```

OR

```
sudo apt install lvm2
```

# References

<a href="https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/index" target="_blank">
<picture>
  <img alt="" src="https://access.redhat.com/webassets/avalon/d/Red_Hat_Enterprise_Linux-7-Logical_Volume_Manager_Administration-en-US/images/fb83bf56728805639af6b760fac589d0/title_logo.png">
</picture>
</a>

