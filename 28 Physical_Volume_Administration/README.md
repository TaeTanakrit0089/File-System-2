# Physical Volume Administration

เป็นการจัดการ **Physical Volume** ทั้งหลายใน Hardisk สามารถที่จะ สร้าง, เพิ่ม/ลดขนาด, ลบ, ป้องกันการเขียนข้อมูล ของ **Physical Volume** ได้ และสามารถใช้ในการนำไปทำ **Volume Group Administrator** หรือ **Logical Volume Administrator** ต่อได้ แต่ก่อนอื่นเราต้องสร้าง และกำหนด **Physical Volume** ขึ้นมาก่อน

## Physical Component
หน่วยที่เก็บข้อมูลกายภาพของ LVM Logical Volume นั้นเป็น Block Device โดยอาจเป็น Partition หรือ Disk ทั้งหมด, เพื่อให้ Device ใช้ LVM Logical Volume ได้นั้น ตัว Device ต้องเริ่มระบุเป็น **Physical Volume (PV)**, การเริ่มกำหนด Block Device หรือก็คือ **Physical Volume** นั้นเป็นการเริ่มการทำงานตอน Device

โดยการทำงานปกติ LVM Label จะอยู่ใน 512-byte Sector ตัวที่สอง โดยสามารถที่จะเปลี่ยนการทำงานของมันได้ โดยการวาง Label ลงบน Sector ไหนก็ได้ของ 4 Sectors แรก เมื่อต้องการจะสร้าง Physical Volume โดยสามารถให้ผู้ใช้คนอื่นใน Sectors สามารถใช้ LVM volumes ร่วมกันได้ หากต้องการได้

ตัว LVM Label ช่วยให้ระบุตัวตนที่ถูกและเรียงอุปกรณ์ให้ Physical Device เพราะ Devices สามารถอยู่ลำดับอะไรก็ได้ เมื่อตอนที่ระบบถูกบูท ตัว LVM Label ยังคงเดิมแม้จะโดนรีบูท หรือผ่าน Cluster

LAM Label ช่วยระบุ Device ของ LVM Physical Volume ได้, มันมี Random Unique Identifier (UUID) ของ Physical Volime ในการระบุตัวตน, มันยังเก็บขนาดของ Block Device แบบ Bytes, และยังเก็บที่อยู่ของ LVM Metadat ที่จะถูกเก็บใน Device ด้วย

LVM Metadata เก็บรายละเอียดการตั้งค่าของ LVM Volume Groups บนระบบให้, โดยปกติจะมีสำเนาของ Metadata อยู่ในทุก ๆ Metadata Area ในทุก ๆ Physical Volume ข้างใน Volume Group, LVM Metadata เล็กและเก็บรูปแบบ ASCII

## LVM Physical Volume Layout
> [!Note]
> ใน Linux kernel, จะให้แต่ละ Sectors นั้นมีขนาด 512 bytes

- LVM Label อยู่ใน Sector ที่สอง
- Metadata
- ที่เหลือเป็น พื้นที่ว่าง
<img alt="" src="https://access.redhat.com/webassets/avalon/d/Red_Hat_Enterprise_Linux-7-Logical_Volume_Manager_Administration-en-US/images/58b3a6c097c618cfcb03163c5cad5d16/physvol.png">

## Multiple Partitions on a Disk
LVM สามารถทำให้สร้าง Physical Volume จาก Disk Partitions, Red Hat แนะนำให้สร้างแค่ Partition ตัวเดียวเพื่อให้คลุม Disk ได้ทั้งหมดไปเป็น Label ไปเป็น LVM Physical Volume ตามเหตุผลดังนี้
- Administrative convenience<br>
มันง่ายกว่าในการติดตาม Hardware ในระบบ ถ้าแต่ละ Disk ปรากฏแค่ครั้งเดียว มันจะชัดเจนเมื่อ Disk นั้นผิดพลาด, เพิ่มเติมการมีหลาย Physical Volume อยู่ใน Disk เดียวกัน อาจทำให้ Kernel เตือนเกี่ยวกับประเภท Partition ที่ไม่รู้จักตอนที่บูท
- Striping performance<br>

โดย **Physical Volumes** ก็เป็นอีกหนึ่ง Component ที่สำคัญในการจัดการ Disk
ก่อนที่จะเข้าไปจัดการ **Physical Volumn** ได้นั้น เราต้องลง **Logical Volumn Manager (LVM)** ใน Linux ก่อน เพื่อให้ใช้งานคำสั่งได้
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

