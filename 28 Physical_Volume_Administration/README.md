# Physical Volumes

&emsp;หน่วยที่เก็บข้อมูลกายภาพของ LVM Logical Volume นั้นเป็น Block Device โดยอาจเป็น Partition หรือ Disk ทั้งหมด,
เพื่อให้
Device ใช้ LVM Logical Volume ได้นั้น ตัว Device ต้องเริ่มระบุเป็น **Physical Volume (PV)**, การเริ่มกำหนด Block Device
หรือก็คือ **Physical Volume** นั้นเป็นการเริ่มการทำงานตอน Device

&emsp;โดยการทำงานปกติ LVM Label จะอยู่ใน 512-byte Sector ตัวที่สอง โดยสามารถที่จะเปลี่ยนการทำงานของมันได้ โดยการวาง
Label ลงบน
Sector ไหนก็ได้ของ 4 Sectors แรก เมื่อต้องการจะสร้าง Physical Volume โดยสามารถให้ผู้ใช้คนอื่นใน Sectors สามารถใช้ LVM
volumes ร่วมกันได้ หากต้องการได้

&emsp;ตัว LVM Label ช่วยให้ระบุตัวตนที่ถูกและเรียงอุปกรณ์ให้ Physical Device เพราะ Devices สามารถอยู่ลำดับอะไรก็ได้
เมื่อตอนที่ระบบถูกบูท ตัว LVM Label ยังคงเดิมแม้จะโดนรีบูท หรือผ่าน Cluster

&emsp;LAM Label ช่วยระบุ Device ของ LVM Physical Volume ได้, มันมี Random Unique Identifier (UUID) ของ Physical Volime
ในการระบุตัวตน, มันยังเก็บขนาดของ Block Device แบบ Bytes, และยังเก็บที่อยู่ของ LVM Metadat ที่จะถูกเก็บใน Device ด้วย

&emsp;LVM Metadata เก็บรายละเอียดการตั้งค่าของ LVM Volume Groups บนระบบให้, โดยปกติจะมีสำเนาของ Metadata อยู่ในทุก ๆ
Metadata
Area ในทุก ๆ Physical Volume ข้างใน Volume Group, LVM Metadata เล็กและเก็บรูปแบบ ASCII

## LVM Physical Volume Layout

> [!Note]
> **ใน Linux kernel, จะให้แต่ละ Sectors นั้นมีขนาด 512 bytes**

- LVM Label อยู่ใน Sector ที่สอง
- Metadata
- ที่เหลือเป็น พื้นที่ว่าง

  <img alt="" src="https://access.redhat.com/webassets/avalon/d/Red_Hat_Enterprise_Linux-7-Logical_Volume_Manager_Administration-en-US/images/58b3a6c097c618cfcb03163c5cad5d16/physvol.png">

## Multiple Partitions on a Disk

&emsp;LVM สามารถทำให้สร้าง Physical Volume จาก Disk Partitions, Red Hat แนะนำให้สร้างแค่ Partition ตัวเดียวเพื่อให้คลุม
Disk
ได้ทั้งหมดไปเป็น Label ไปเป็น LVM Physical Volume ตามเหตุผลดังนี้

- **Administrative convenience**<br>
  มันง่ายกว่าในการติดตาม Hardware ในระบบ ถ้าแต่ละ Disk ปรากฏแค่ครั้งเดียว มันจะชัดเจนเมื่อ Disk นั้นผิดพลาด,
  เพิ่มเติมการมีหลาย Physical Volume อยู่ใน Disk เดียวกัน อาจทำให้ Kernel เตือนเกี่ยวกับประเภท Partition
  ที่ไม่รู้จักตอนที่บูท
- **Striping performance**<br>
  LVM ไม่สามารถบอกได้ว่าสอง Physical Volume อยู่บน Physical Disk เดียวกันหรือป่าว ถ้ามีสอง Physical Volume อยู่ใน
  Physical Disk เดียวกัน การสร้าง Logical Volume ในการแบ่งพวกมันออกจากกัน มันอาจแบ่งให้อยู่คนละ Partition ที่อยู่ Disk
  เดียวกันได้ และอาจส่งผลให้เป็นการลดประสิทธิภาพมากกว่าเพิ่มประสิทธิภาพ

> [!Tip]
> **&emsp;แม้จะไม่แนะนำให้ทำ แต่อาจมีสถานการณ์บางอย่างที่ทำให้จำเป็นต้องแบ่ง Disk ออกจาก LVM Physical Volume
ตัวอย่างเช่น
> ในระบบที่มี Disk ไม่มากอาจจำเป็นต้องส่งข้อมูลไปมาระหว่าง Partitions เมื่อต้องการที่จะโยกย้ายระบบเดิมไปยัง LVM Volumes
> เพิ่มเติมก็คือถ้ามี Disk ขนาดใหญ่มาก และต้องการจะมีมากกว่า 1 Volume Group ในการบริหารงั้นมันก็จำเป็นในการแบ่ง Disk (
Partition)
> ถ้าต้องการ Disk ที่มีมากกว่า 1 Partition และ Partitions ทั้งคู่ก็อยู่ใน Volume Group เดียวกัน ควรดูแลเฉพาะ
> Partition ที่อยู่ใน Logical Volume เมื่อสร้าง Striped Volumes**

<hr />

# The Physical Volume Label

&emsp;โดยการตั้งค่าปกติ คำสั่ง `pvcreate` จะวาง Physical Volume Label ใน sector ที่สอง 512-byte โดยที่ Label
สามารถแก้โดยไปยัง sector อื่น ๆ ได้ใน 4 sectors LVM tools สามารถสแกนหา Physical Volume Label ได้โดยจะเช็คที่ 4 sectors
แรก เพราะฉะนั้น Physical Volume Lavel จะขึ้นด้วย string `LABELONE`

&emsp;ข้อมูลภายใน Physical Volume Label

- Physical Volume UUID
- ขนาดของ Block Device รูปแบบ Bytes
- NULL - บอกตำแหน่งพื้นที่ของ List of Data
- NULL - บอกตำแหน่งพื้นที่ของ Lists of Metadata

&emsp;Metadata locations เก็บแบบ offset และเป็นขนาด (รูปแบบ Bytes) จะมีพื้นที่ใน Label ประมาณ 15 Locations แต่ LVM Tools
นั้นใช้แค่ 3 โดยมี
1 พื้นที่ของ Data บวกกับ 2 พื้นที่ของ Metadata

<hr />

# Physical Volume Administration

&emsp;มีหน้าที่ในการจัดการ **Physical Volumes** ใน Storage Environment หรือก็คือการจัดการ Disk ภายใน Server
ใช้ในการตรวจสอบ เพิ่ม ลบ ดูแล

## 1. Creating Physical Volumes

- ถ้าต้องการใช้ Disk ทั้งหมดในการใช้เป็น Physical Volume ตัว Disk จำเป็นต้องไม่มี Partition Table ในการใช้ DOS Disk
  Partitions ตัว Partition ID ต้องเซ็ตเป็น `0x8e` โดยใช้คำสั่ง `fdisk` หรือ `cfdisk` หรืออะไรก็ได้ที่ใช้ได้เหมือนกัน

```
fdisk -l
```

ด้วยคำสั่งนี้จะได้ List Disk ออกมา ใช้เพื่อดูขนาดของแต่ละ Disk และดูว่ามี Disk อะไรบ้างได้

```
fdisk `Select Devices in /dev`
```

- และเลือกคำสั่ง `n` เพื่อทำการสร้าง Partition เมื่อสร้างเสร็จแล้วให้กำหนด Type Partition โดยคำสั่ง `t` เป็น `0x8e` โดย
  Partition นั้นจะถูกเปลี่ยนเป็น `Linux LVM`
- ในการใช้ Disk Devices ทั้งหมด ต้องมีแค่ Partition Table ที่ถูกลบข้อมูล โดยจะส่งผลให้เป็นการทำลายข้อมูลทั้งหมดใน Disk
  นั้น แต่ก็จะสามารถใช้ลบ Partition Table ที่มีอยู่แล้วได้ โดยการใส่ "ศูนย์" ไปยัง Sector แรกด้วยคำสั่ง

```
dd if=/dev/zero of=PhysicalVolume bs=512 count=1
```

<details>

<summary>1.1 Initializing Physical Volumes</summary>

- ใช้คำสั่ง `pvcreate` ในการเริ่มสร้าง Block Device ที่ใช้ในการเป็น Physical Volume
  การเริ่มต้นจะคล้ายคลึงกับการจัดรูปแบบระบบไฟล์
- โดยตามคำสั่งที่ใช้ในการสร้างคือ `/dev/sdd`, `/dev/sde`, และ `/dev/sdf` เป็น LVM Physical Volumes ที่จะใช้ในภายหลัง
  โดยเป็นส่วนของ LVM Logical Volumes

```
pvcreate /dev/sdd /dev/sde /dev/sdf
```

- ในการที่ต้องการสร้างเป็น Partitions มากกว่าใช้ Disk ทั้งหมด โดยรันคำสั่ง
  <br>`pvcreate` ของ Partition ตามดังตัวอย่างในการเริ่ม Partition `/dev/hdb1` โดยเป็น LVM Physical Volume
  สำหรับใช้กับส่วนของ LVM Logical Volume

```
pvcreate /dev/hdb1
```

</details>

<details>

<summary>1.2 Scanning for Block Devices</summary>

&emsp;สามารถสแกนหา Block Devices ที่จะถูกใช้เป็น **Physical Volumes** โดยใช้คำสั่ง `lvmdiskscan` ตามดังนี้

```
lvmdiskscan
```

&emsp;_ผลลัพท์_

```
  /dev/ram0                    [       16.00 MB]
  /dev/sda                     [       17.15 GB]
  /dev/root                    [       13.69 GB]
  /dev/ram                     [       16.00 MB]
  /dev/sda1                    [       17.14 GB] LVM physical volume
  /dev/VolGroup00/LogVol01     [      512.00 MB]
  /dev/ram2                    [       16.00 MB]
  /dev/new_vg/lvol0            [       52.00 MB]
  /dev/ram3                    [       16.00 MB]
  /dev/pkl_new_vg/sparkie_lv   [        7.14 GB]
  /dev/ram4                    [       16.00 MB]
  /dev/ram5                    [       16.00 MB]
  /dev/ram6                    [       16.00 MB]
  /dev/ram7                    [       16.00 MB]
  /dev/ram8                    [       16.00 MB]
  /dev/ram9                    [       16.00 MB]
  /dev/ram10                   [       16.00 MB]
  /dev/ram11                   [       16.00 MB]
  /dev/ram12                   [       16.00 MB]
  /dev/ram13                   [       16.00 MB]
  /dev/ram14                   [       16.00 MB]
  /dev/ram15                   [       16.00 MB]
  /dev/sdb                     [       17.15 GB]
  /dev/sdb1                    [       17.14 GB] LVM physical volume
  /dev/sdc                     [       17.15 GB]
  /dev/sdc1                    [       17.14 GB] LVM physical volume
  /dev/sdd                     [       17.15 GB]
  /dev/sdd1                    [       17.14 GB] LVM physical volume
  7 disks
  17 partitions
  0 LVM physical volume whole disks
  4 LVM physical volumes
```

</details>

## 2. Displaying Physical Volumes

&emsp;มี 3 คำสั่งที่สามารถใช้ในการแสดง Properties ของ LVM Physical Volumes:<br>`pvs`, `pvdisplay`, และ `pvscan`

<details>

<summary>2.1 คำสั่ง `pvs`</summary>

- คำสั่ง `pvs` ให้ข้อมูลการตั้งค่าของ Physical Volume แสดงผลหนึ่งบรรทัดต่อ Physical Volume

```
pvs
```

&emsp;_ผลลัพท์_

```
  PV         VG     Fmt  Attr PSize  PFree
  /dev/sdb1  new_vg lvm2 a-   17.14G 17.14G
  /dev/sdc1  new_vg lvm2 a-   17.14G 17.09G
  /dev/sdd1  new_vg lvm2 a-   17.14G 17.14G
```

<hr />

<details>

<summary>Customize Display</summary>

- คำสั่ง `pvs` ทำให้ควบคุมการฟอร์แมตได้ดีมาก ๆ, และมีประโยชน์ในการทำ Scripting อีกด้วย โดยในการแก้ไข Output
  ของการใช้คำสั่ง `pvs` สามารถทำได้
  และสามารถเปลี่ยนฟิลด์ที่อยากให้แสดงให้มากกว่าปกติ โดยการใช้ Argument `-o`

```
pvs -o pv_name,pv_size
```

&emsp;_ผลลัพท์_

```
  PV         PSize
  /dev/sdb1  17.14G
  /dev/sdc1  17.14G
  /dev/sdd1  17.14G
```

### ตารางฟิลด์ที่แสดงในคำสั่ง pvs

| Argument            | Header  | Description                                                                |
|---------------------|---------|:---------------------------------------------------------------------------|
| `dev_size`          | DevSize | The size of the underlying device on which the physical volume was created |
| `pe_start`          | 1st PE  | Offset to the start of the first physical extent in the underlying device  |
| `pv_attr`           | Attr    | Status of the physical volume: (a)llocatable or e(x)ported.                |
| `pv_fmt`            | Fmt     | The metadata format of the physical volume (lvm2 or lvm1)                  |
| `pv_free`           | PFree   | The free space remaining on the physical volume                            |
| `pv_name`           | PV      | The physical volume name                                                   |
| `pv_pe_alloc_count` | Alloc   | Number of used physical extents                                            |
| `pv_pe_count`       | PE      | Number of physical extents                                                 |
| `pvseg_size`        | SSize   | The segment size of the physical volume                                    |
| `pvseg_start`       | Start   | The starting physical extent of the physical volume segment                |
| `pv_size`           | PSize   | The size of the physical volume                                            |
| `pv_tags`           | PV Tags | LVM tags attached to the physical volume                                   |
| `pv_used`           | Used    | The amount of space currently used on the physical volume                  |
| `pv_uuid`           | PV UUID | The UUID of the physical volume                                            |

Information: [Customized Reporting for LVM](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/custom_report#report_format_control)

<hr />
<br />

- สามารถที่จะเพิ่มฟิลด์ไปยัง Output ด้วยสัญลักษณ์บวก (+) โดยเอามาใช้ร่วมกับ Argument `-o`

```
pvs -o +pv_uuid
```

&emsp;_ผลลัพท์_

```
  PV         VG     Fmt  Attr PSize  PFree  PV UUID
  /dev/sdb1  new_vg lvm2 a-   17.14G 17.14G onFF2w-1fLC-ughJ-D9eB-M7iv-6XqA-dqGeXY
  /dev/sdc1  new_vg lvm2 a-   17.14G 17.09G Joqlch-yWSj-kuEn-IdwM-01S9-X08M-mcpsVe
  /dev/sdd1  new_vg lvm2 a-   17.14G 17.14G yvfvZK-Cf31-j75k-dECm-0RZ3-0dGW-UqkCS
```

<hr />
<br />

- การเพิ่ม Argument `-v` ไปยังคำสั่งจะเพิ่มฟิลด์เพิ่มเติม เช่นคำสั่ง `pvs -v` จะแสดงฟิลด์ `DevSize` และ `PV UUID`
  เพิ่มเติมไปยังฟิลด์ตามปกติ

```
pvs -v
```

&emsp;_ผลลัพท์_

```
    Scanning for physical volume names
  PV         VG     Fmt  Attr PSize  PFree  DevSize PV UUID
  /dev/sdb1  new_vg lvm2 a-   17.14G 17.14G  17.14G onFF2w-1fLC-ughJ-D9eB-M7iv-6XqA-dqGeXY
  /dev/sdc1  new_vg lvm2 a-   17.14G 17.09G  17.14G Joqlch-yWSj-kuEn-IdwM-01S9-XO8M-mcpsVe
  /dev/sdd1  new_vg lvm2 a-   17.14G 17.14G  17.14G yvfvZK-Cf31-j75k-dECm-0RZ3-0dGW-tUqkCS
```

<hr />
<br />

- Argument `--noheadings` จะไม่แสดง Headings line สามารถนำไปใช้ในการเขียน Scripts
  ตัวอย่างก็เช่นการใช้ Argument `--noheadings` ในการร่วมกับ Argument `pv_name` โดยมันจะแสดงลิสต์ทั้งหมดของ **Physical
  Volumes**

```
pvs --noheadings -o pv_name
```

&emsp;_ผลลัพท์_

```
  /dev/sdb1
  /dev/sdc1
  /dev/sdd1
```

<hr />
<br />

- Argument `--separator (separator)` ใช้ตัว separator ในการแยกแต่ละฟิลด์
  ตัวอย่างการแยก Output ฟิลด์ของคำสั่ง `pvs` ด้วยสัญลักษณ์เท่ากับ (=)

```
pvs --separator =
```

&emsp;_ผลลัพท์_

```
  PV=VG=Fmt=Attr=PSize=PFree
  /dev/sdb1=new_vg=lvm2=a-=17.14G=17.14G
  /dev/sdc1=new_vg=lvm2=a-=17.14G=17.09G
  /dev/sdd1=new_vg=lvm2=a-=17.14G=17.14G
```

<hr />
<br />

- เพื่อให้แต่ละฟิลด์นั้นเว้นระยะห่าง เมื่อใช้ Argument `separator` ต้องใช้ร่วมกับ Argument `--aligned`

```
pvs --separator = --aligned
```

&emsp;_ผลลัพท์_

```
  PV        =VG    =Fmt =Attr=PSize =PFree
  /dev/sdb1 =new_vg=lvm2=a-  =17.14G=17.14G
  /dev/sdc1 =new_vg=lvm2=a-  =17.14G=17.09G
  /dev/sdd1 =new_vg=lvm2=a-  =17.14G=17.14G
```

</details>

<hr />

- สามารถใช้คำสั่ง `pvs -a` เพื่อตรวจสอบ Devices ที่ไม่ได้ถูกกำหนดเป็น LVM Physical Volumes ด้วย LVM

```
pvs -a
```

&emsp;_ผลลัพท์_

```
  PV                             VG     Fmt  Attr PSize  PFree
  /dev/VolGroup00/LogVol01                   --       0      0
  /dev/new_vg/lvol0                          --       0      0
  /dev/ram                                   --       0      0
  /dev/ram0                                  --       0      0
  /dev/ram2                                  --       0      0
  /dev/ram3                                  --       0      0
  /dev/ram4                                  --       0      0
  /dev/ram5                                  --       0      0
  /dev/ram6                                  --       0      0
  /dev/root                                  --       0      0
  /dev/sda                                   --       0      0
  /dev/sdb                                   --       0      0
  /dev/sdb1                      new_vg lvm2 a-   17.14G 17.14G
  /dev/sdc                                   --       0      0
  /dev/sdc1                      new_vg lvm2 a-   17.14G 17.09G
  /dev/sdd                                   --       0      0
  /dev/sdd1                      new_vg lvm2 a-   17.14G 17.14G
```

</details>

<details>

<summary>2.2 คำสั่ง `pvdisplay`</summary>

&emsp;คำสั่ง `pvdisplay` ให้ Output ออกมารายละเอียดแต่ละ Physical Volume เป็นหลายบรรทัด มันจะแสดงคุณสมบัติของ Physical (
Size, Extents, Volume Group, และอื่น ๆ) ในรูปแบบที่ตั้งค่าไว้

```
pvdisplay
```

&emsp;_ผลลัพท์_

```
  --- Physical volume ---
  PV Name               /dev/sdc1
  VG Name               new_vg
  PV Size               17.14 GB / not usable 3.40 MB
  Allocatable           yes
  PE Size (KByte)       4096
  Total PE              4388
  Free PE               4375
  Allocated PE          13
  PV UUID               Joqlch-yWSj-kuEn-IdwM-01S9-XO8M-mcpsVe
```

</details>

<details>

<summary>2.3 คำสั่ง `pvscan`</summary>

&emsp;คำสั่ง `pvscan` จะสแกน **Physical Volumes** ทุกตัวที่ได้มาจาก LVM Block Devices ในระบบ

```
pvscan
```

&emsp;_ผลลัพท์_

```
 PV /dev/sdb2   VG vg0   lvm2 [964.00 MB / 0   free]
 PV /dev/sdc1   VG vg0   lvm2 [964.00 MB / 428.00 MB free]
 PV /dev/sdc2            lvm2 [964.84 MB]
 Total: 3 [2.83 GB] / in use: 2 [1.88 GB] / in no VG: 1 [964.84 MB]
```

&emsp;สามารถกำหนด Filter ในไฟล์ `lvm.conf` ที่จะทำให้คำสั่งนี้หลีกเลี่ยงการสแกน **Physical Volumes** ที่กำหนดได้
สำหรับข้อมูลในการใช้ Filter ในการควบคุมว่าจะให้ Devices ไหนถูกสแกน
สามารถดูได้ใน [Controlling LVM Device Scans with Filters](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/lvm_filters)

</details>

<hr />

## 3. Preventing Allocation on a Physical Volume

&emsp;สามารถระงับการจัดสรรพื้นที่ว่างเพิ่มของ **Physical Volumes** หนึ่งหรือมากกว่านั้น ด้วยคำสั่ง `pvchange`
อาจจำเป็นถ้าตัว Disk เกิดข้อผิดพลาดขึ้น หรือถ้าต้องการจะลบตัว Physical Volume

&emsp;ตามตัวอย่างเป็นการระงับไม่ให้จัดสรรตัว `/dev/sdk1`

```
pvchange -x n /dev/sdk1
```

&emsp;และก็สามารถใช้ Arguments `-xy` ในคำสั่งของ `pvchange` เพื่ออนุณาตให้จัดสรรได้อีกครั้ง หลังจากที่ทำการระงับไป

<hr />

## 4. Resizing a Physical Volume

&emsp;ถ้าต้องการเปลี่ยนขนาดของโครงสร้างของ Block Device ไม่ว่าเหตุผลใดก็ตาม ก็ใช้คำสั่ง `pvresize` เพื่ออัพเดท LVM
ด้วยขนาดที่เปลี่ยน สามารถดำเนินคำสั่งนี้ในขณะที่ LVM ใช้ Physical Volume อยู่

<hr />

## 5. Removing Physical Volumes

&emsp;ถ้า Device ไม่จำเป็นในการใช้ด้วย LVM อีกแล้ว สามารถที่จะลบ LVM Label ด้วยคำสั่ง `pvremove` การทำคำสั่ง `pvremove`
เป็นลบ LVM Metadata ของ Physical Volume ที่ว่าง

```
pvremove /dev/ram15
```

&emsp;_ผลลัพท์_

```
  Labels on physical volume "/dev/ram15" successfully wiped
```

<hr />

# Practice

&emsp;โดย **Physical Volumes** ก็เป็นอีกหนึ่ง Component ที่สำคัญในการจัดการ Disk และ

&emsp;ก่อนจะเข้าไปจัดการ **Physical Volumn** ได้นั้น เราต้องลง **Logical Volumn Manager (LVM)** ใน Linux ก่อน
เพื่อให้ใช้งานคำสั่งได้
แต่ถ้าใช้ Ubuntu Server จะสามารถใช้งาน **LVM** ได้เลย โดยที่เราไม่ต้องลง Package ของ **LVM**

```
apt install lvm2
```

OR

```
sudo apt install lvm2
```

<hr />

# References

<a href="https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/index" target="_blank">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/img/redhaticon/redhat_w.png">
  <source media="(prefers-color-scheme: light)" srcset="../assets/img/redhaticon/redhat_b.png">
  <img alt="Shows an illustrated sun in light mode and a moon with stars in dark mode." src="../assets/img/redhaticon/redhat_w.png">
</picture>
</a>

