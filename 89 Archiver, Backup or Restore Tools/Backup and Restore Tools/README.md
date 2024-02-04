# Backup and Restore Tools

## Basic Tar

การบีบอัดไฟล์ก่อนการสำรองข้อมูลอาจเป็นประโยชน์ เครื่องมือยอดนิยมสำหรับการบีบอัดไฟล์ปกติบน Linux คือ gzip/gunzip
หรือไม่ก็สามารถใช้ tar ในการบีบอัดไฟล์โดย tar ได้รับชื่อมาจาก Tape Archive
เครื่องมือนี้จะรับและส่งไฟล์ไปยังปลายทาง (โดยทั่วไปจะเป็นเทปหรือไฟล์ปกติ) ตัวเลือก `-c` ใช้เพื่อสร้างไฟล์เก็บถาวร tar (
หรือ
tarfile) ซึ่งเป็นตัวเลือก `-f` เพื่อตั้งชื่อ/สร้างไฟล์ tar ตัวอย่างด้านล่างนี้จะสำรองข้อมูลของ /etc ลงในไฟล์
/backup/etc.tar

```shell
~# tar cf /backup/etc.tar /etc
~# ls -l /backup/etc.tar
-rw-r--r--  1 root root 47800320 May 12 11:47 /backup/etc.tar
```

การบีบอัดสามารถทำได้โดยไม่ต้องใช้ pipe เนื่องจาก tar ใช้ตัวเลือก `-z` เพื่อบีบอัดด้วย gzip และตัวเลือก `-j`
เพื่อบีบอัดด้วย bzip2

```shell
~# tar czf /backup/etc.tar.gz /etc
~# tar cjf /backup/etc.tar.bz2 /etc
~# ls -l /backup/etc.ta*
-rw-r--r--  1 root root 47800320 May 12 11:47 /backup/etc.tar
-rw-r--r--  1 root root  6077340 May 12 11:48 /backup/etc.tar.bz2
-rw-r--r--  1 root root  8496607 May 12 11:47 /backup/etc.tar.gz
~#
```

ตัวเลือก `-t` ใช้เพื่อแสดงรายการเนื้อหาของไฟล์ tar สามารถเปิดโหมด Verbose ได้ด้วย `-v` (
ใช้สำหรับเมื่อต้องการดูไฟล์ที่ถูกบีบระหว่างการบีบอัด)

```shell
~# tar tvf /backup/etc.tar
drwxr-xr-x root/root  0 2007-05-12 09:38:21 etc/
-rw-r--r-- root/root  2657 2004-09-27 10:15:03 etc/warnquota.conf
-rw-r--r-- root/root  13136 2006-11-03 17:34:50 etc/mime.types
drwxr-xr-x root/root  0 2004-11-03 13:35:50 etc/sound/
```

หากต้องการแสดงรายการไฟล์เฉพาะในไฟล์เก็บถาวร tar ให้ใช้ตัวเลือก `-t` เพิ่มด้วยชื่อไฟล์ (โดยไม่ต้องนำหน้าด้วย /)

```shell
~# tar tvf /backup/etc.tar etc/resolv.conf
-rw-r--r-- root/root        77 2007-05-12 08:31:32 etc/resolv.conf
```

ใช้ตัวเลือก `-x` เพื่อกู้คืนไฟล์ tar หรือไฟล์เดียวจากไฟล์บีบอัดโดยที่ค่าเริ่มต้น tar จะกู้คืนไฟล์ในไดเร็กทอรีปัจจุบัน

```shell
~# tar xvf /backup/etc.tar etc/resolv.conf
etc/resolv.conf
~# ls -l /etc/resolv.conf
-rw-r--r--  2 root root 40 May 12 12:05 /etc/resolv.conf
~# ls -l etc/resolv.conf
-rw-r--r--  1 root root 77 May 12 08:31 etc/resolv.conf
~#
```

และยังสามารถรักษาค่า Permission ของไฟล์ได้ด้วยแฟล็ก `-p` และคุณสามารถยกเว้นไดเร็กทอรีหรือไฟล์ด้วย --exclude

```shell
~# tar cpzf /backup/etc_with_perms.tgz /etc
~# tar cpzf /backup/etc_no_sysconf.tgz /etc --exclude /etc/sysconfig
~# ls -l /backup/etc_*
-rw-r--r--  1 root root 8434293 May 12 12:48 /backup/etc_no_sysconf.tgz
-rw-r--r--  1 root root 8496591 May 12 12:48 /backup/etc_with_perms.tgz
~#
```

## Backup Types

Linux ใช้ระบบในปการสำรองข้อมูลส่วนเพิ่มหลายระดับ (Multilevel Incremental) โดยใช้ระดับที่แตกต่างกัน
การสำรองข้อมูลแบบเต็มคือการสำรองข้อมูลที่ระดับ 0
การสำรองข้อมูล x ระดับที่สูงกว่าจะรวมการเปลี่ยนแปลงทั้งหมดตั้งแต่การสำรองข้อมูล x-1 ระดับล่าสุด

สมมติว่าคุณฉงนสำรองข้อมูลทั้งหมดในวันจันทร์ (ระดับ 0)

- คุณฉงนต้องการสำรองข้อมูลระดับ 1 ในวันอังคาร จากนั้นการสำรองข้อมูลในวันอังคารจะมีการเปลี่ยนแปลงทั้งหมดตั้งแต่วันจันทร์
- ในส่วนของการสำรองข้อมูลระดับที่ 2 ในวันพุธจะมีการเปลี่ยนแปลงทั้งหมดตั้งแต่วันอังคาร (ระดับ 2-1 สุดท้าย)
- การสำรองข้อมูลระดับ 3 ในวันพฤหัสบดีจะมีการเปลี่ยนแปลงทั้งหมดตั้งแต่วันพุธ (ระดับ 3-1 ล่าสุด)
- อีกระดับ 3 ในวันศุกร์จะมีการเปลี่ยนแปลงทั้งหมดตั้งแต่วันพุธด้วย
- การสำรองข้อมูลระดับ 2 ในวันเสาร์จะนำการเปลี่ยนแปลงทั้งหมดตั้งแต่ระดับ 1 สุดท้ายเมื่อวันอังคาร