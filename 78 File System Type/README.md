File System Type
===
---

## บทบาทหน้าที่หลักคือการจัดระเบียบข้อมูลและจัดการข้อมูลบนอุปกรณ์จัดเก็บข้อมูล เช่น Hard drives, solid-state drives (SSDs)

### ประเภทของระบบไฟล์ที่ใช้บนระบบ Linux ได้แก่

- Ext4 : รองรับไฟล์ที่มีขนาดใหญ่สูงสุดถึง 16TB และยังรองรับ subdirectories ได้ไม่จำกัดและยังสามารถใช้กับรุ่นก่อนหน้า
  Ext2 และ Ext3 ได้
    - ประสิทธิภาพสูงเหมาะสำหรับงานปริมาณใหญ่ๆ
    - ลดความเสี่ยงของความเสียหายเมื่อระบบล่ม
- JFS : ระบบไฟล์ชี้สามารถกู้คืนไฟล์ได้อย่างรวดเร็วหลังจากเกิดการขัดข้องจากการบันทึกข้อมูล
- XFS : เป็นระบบไฟล์ที่เป็นค่าเริ่มต้นใน ระบบ Red Hat Enterprise Linux
    - สามารถจัดการไฟล์ขนาดใหญ่ได้อย่างมีประสิทธิภาพ
    - ประสิทธิภาพดีมาก สำหรับ parallel I/O operations
- ZFS : เป็นระบบไฟล์ที่มีคุณสมบัติการซ่อมแซมข้อมูลที่ยอดเยี่ยมและความจุในการจัดเก็บข้อมูลสูง
  และเหมาะสำหรับการจัดเก็บข้อมูลขนาดใหญ่
- ReiserFS : เป็นระบบไฟล์ทางเลือกของ ext3 แต่จะมีประสิทธิภาพที่ดีกว่าและมีฟีเจอร์ที่เยอะกว่า
- Btrfs : เป็นระบบไฟล์ที่ให้ความสำคัญกับความถูกต้องของข้อมูล จัดการได้ง่ายและรองรับฟีเจอร์ขั้นสูงเช่น snapshots,
  built-in RAID, and copy-on-write.
    - มีการทำ checksums
    - มี snapshots ไว้สำหรับ สำรอง และ กู้คืนข้อมูล
- Vfat : เป็นระบบไฟล์ที่เก็บ bootloader ไว้ โดยทั่วไปจะเรียกกันว่า "boot partition"
- Tmpfs : เป็นระบบที่เก็บไฟล์ไว้ใน Virtual memory.

---
หลักการทำงาน
===
---
![The Picture](/assets/img/78%20File%20System%20Type/linux-file-system.png)

    รูปจาก : https://www.javatpoint.com/linux-file-system 

**File System ต้องเรียกใช้ API ในการเข้าถึงฟังก์ชั่น เพื่อโต้ตอบกับส่วนต่างๆ เช่น File และ Directory**

- **API จะช่วยอำนวยความสะดวกให้กับงานต่างๆ เช่น การสร้าง การลบ และการคัดลอกไฟล์
  อำนวยความสะดวกให้กับอัลกอริธึมที่กำหนดการจัดเรียงไฟล์**

- **Virtual file system ให้บริการชุดคำสั่งสำหรับ kernel เพื่อที่ให้ผู้ใช้งานสามารถเข้าถึง ประเภทของ File system
  ทั้งหมดได้
  Virtual file system
  จะเรียกเฉพาะไดรฟ์เวอร์ที่จำเป็นในการเชื่อมต่อกับ ระบบไฟล์(File System)ประเภทนั้นๆเพื่อทำการเชื่อมต่อกับระบบไฟล์ประเภท(
  Type)ต่างๆได้**

---

# Commands for File System

## mkfs (make file system)

**เป็นคำสั่งที่ย่อมาจาก 'make file system' คือการ format partition นั้นๆที่เราเลือก ให้มี ระบบไฟล์ตามที่เราต้องการ เช่น
**

```sudo mkfs.xfs /dev/sda1 -f```

**เป็นการทำให้ partition /dev/sda1 มีระบบไฟล์แบบ xfs การเติม -f เป็นการบังคับหลีกเลี่ยง error**

![The Picture 2](/assets/img/xfs.png)

    รูปจาก : https://www.geeksforgeeks.org/linux-file-system/

## mount < device > < path >

**เป็นคำสั่งสำหรับการเชื่อม ต่อ อุปกรณ์เก็บข้อมูล กับ directory**

```sudo mount /dev/sda1 /mnt/jayesh_xfs_partition```

## df (disk free)

> เป็นคำสั่งในระบบปฏิบัติการ Linux ที่ใช้เพื่อแสดงข้อมูลเกี่ยวกับพื้นที่ว่างและใช้งานใน (partition)
> และอุปกรณ์เก็บข้อมูลต่าง ๆ บนระบบไฟล์



<details>

<summary>  1.Check Mount  </summary>

สามารถใช้ `df -h` เพื่อตรวจสอบการ mount ได้

![The Picture 3](/assets/img/xfs2.png)

    รูปจาก : https://www.geeksforgeeks.org/linux-file-system/

</details>

<details>

<summary>2.Display Details For File System</summary>

สามารถใช้ `df -Th` เพื่อแสดงผลข้อมูลต่างๆได้ดังนี้

```
Filesystem     Type      Size  Used Avail Use% Mounted on 
devtmpfs       devtmpfs  4.0M     0  4.0M   0% /dev 
tmpfs          tmpfs     1.9G     0  1.9G   0% /dev/shm 
tmpfs          tmpfs     756M  1.6M  754M   1% /run
/dev/sda3      btrfs      14G  4.3G  8.5G  34% / 
tmpfs          tmpfs     1.9G   16K  1.9G   1% /tmp 
/dev/sda3      btrfs      14G  4.3G  8.5G  34% /home 
/dev/sda2      ext4      974M  297M  610M  33% /boot 
/dev/sda1      vfat      599M   18M  582M   3% /boot/efi
tmpfs          tmpfs     378M  124K  378M   1% /run/user/1000 
```

</details>

## fsck (file system consistency check)

> เป็นคำสั่งในระบบปฏิบัติการ Linux ที่ใช้ในการตรวจสอบและซ่อมแซมไฟล์ระบบหรือระบบไฟล์ที่มีปัญหา

```
exit code ที่สามารถแสดงออกมาได้
0 – No errors
1 – Filesystem errors corrected
2 – System should be rebooted
4 – Filesystem errors left uncorrected
8 – Operational error
16 – Usage or syntax error
32 – Fsck canceled by user request
128 – Shared-library error
```

## fsck Command Options

> -A ใช้เพื่อตรวจสอบ File System ทั้งหมดจาก `/etc/fstab`

> -C ใช้เพื่อแสดง progress bar

> -l ใช้เพื่อ lock อุปกรณ์ เพื่อไม่ให้เกิดการใช้งานระหว่างกำลังตรวจสอบ

> -M ใช้เพื่อไม่ตรวจสอบ File System ที่ทำ mount

> -N ใช้เพื่อแสดงผลการตรวจสอบแต่ไม่ทำการซ่อมแซมใดๆ

> -R ใช้เมื่อไม่ต้องการตรวจสอบ root File System มีประโยชน์เฉพาะกับ -A

> -r ใช้เพื่อทำ สถิติ สำหรับแต่ละอุปกรณ์เมื่อทำการตรวจสอบ

> -V ใช้เพื่อแสดงเวอร์ชั่นของ fsck



<details>

<summary>1.Check File System And Disk Partition</summary>

สามารถใช้ `fsck < path >` เพื่อเช็คได้ จะแสดงผลลัพธ์ ดังนี้

```
$ fsck /dev/sda6
fsck from util-linux 2.20.1
e2fsck 1.42 (29-Nov-2011)
/dev/sda6: clean, 95/2240224 files, 3793506/4476416 blocks
```

เมื่อพบ error

```
$ fsck /dev/sda2
fsck from util-linux 2.20.1
fsck: fsck.ntfs: not found
fsck: error 2 while executing fsck.ntfs for /dev/sda2
```

</details>

<details>

<summary>2.Check All File System</summary>

สามารถใช้ `option -A` เพื่อเช็ค file system ภายใต้ `/etc/fstab` ทั้งหมดได้

```
$ fsck -AR -y
fsck from util-linux 2.20.1
e2fsck 1.42 (29-Nov-2011)
/dev/sda6: clean, 95/2240224 files, 3793506/4476416 blocks
dosfsck 3.0.12, 29 Oct 2011, FAT32, LFN
/dev/sda7: 8 files, 50/1463400 clusters
```

</details>

<details>

<summary>3.Check Only File System You Want To Check</summary>

สามารถใช้ `option -t` ในการแสดง File System Type ที่เราต้องการได้

```
$ fsck -AR -t ext2 -y
fsck from util-linux 2.20.1
e2fsck 1.42 (29-Nov-2011)
/dev/sda6: clean, 11/2240224 files, 70327/4476416 blocks
```

</details>

<details>

<summary>4.Repair</summary>

เมื่อพบเจอ error หลังจากทำการตรวจสอบสามารถแก้ไขได้ด้วยการ ใช้ `option -y`

```
$ fsck -y /dev/sda6
fsck from util-linux 2.20.1
e2fsck 1.42 (29-Nov-2011)
/dev/sda6 contains a file system with errors, check forced.
Pass 1: Checking inodes, blocks, and sizes
Inode 2060353 is a unknown file type with mode 0137642 but it looks 
like it is really a directory.
Fix? yes

Pass 2: Checking directory structure
Entry 'test' in / (2) has deleted/unused inode 49059.  Clear? yes

Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information

/dev/sda6: ***** FILE SYSTEM WAS MODIFIED *****
/dev/sda6: 96/2240224 files (7.3% non-contiguous), 3793508/4476416 blocks
```

</details>


---

### Reference:

    https://www.networkworld.com/article/972445/how-to-determine-your-linux-system-s-filesystem-types.html
    https://www.javatpoint.com/linux-file-system
    https://medium.com/@extio/a-comprehensive-guide-to-linux-file-system-types-fcb13cd7d3f3
    https://www.geeksforgeeks.org/linux-file-system/
    https://www.networkworld.com/article/972156/understanding-linux-file-system-types.html
    https://www.ibm.com/docs/en/aix/7.1?topic=d-df-command
    https://www.tecmint.com/fsck-repair-file-system-errors-in-linux/
    https://saixiii.com/fsck-linux-command/



