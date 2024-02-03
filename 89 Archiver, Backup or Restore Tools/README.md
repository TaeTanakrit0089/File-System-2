# Archiver, Backup/Restore Tools

Archiver, Backup/Restore Tools คือเครื่องมือที่ช่วยในการสำรองข้อมูลและการกู้คืนข้อมูลในระบบ Linux
มีหลากหลายเครื่องมือที่มีคุณสมบัติและฟังก์ชันที่แตกต่างกัน โดยในหัวข้อนี้จะแบ่งออกเป็น 2 ส่วนหลักๆ ได้แก่

1. **Archiver:** เป็นโปรแกรมหรือเครื่องมือที่ใช้สำหรับบีบอัด (compress) และแยกไฟล์ (extract)
   ที่มีอยู่ในรูปแบบของเครื่องมือสำหรับบีบอัดและแยกไฟล์ เช่น WinRAR, 7-Zip, และ WinZip
   โดยการบีบอัดไฟล์ช่วยลดขนาดของไฟล์เพื่อประหยัดพื้นที่จัดเก็บและลดเวลาในการถ่ายโอนข้อมูล
2. **Backup and Restore Tools:** ป็นเครื่องมือหรือโปรแกรมที่ใช้สำหรับสำรองข้อมูล (backup) และคืนค่าข้อมูล (restore)
   โดยมักให้ความสามารถในการสำรองข้อมูลจากอุปกรณ์หรือระบบเก็บข้อมูลต่างๆ เช่น ฮาร์ดไดร์ฟ และการคืนค่าข้อมูลเมื่อจำเป็น
   เพื่อป้องกันข้อมูลจากการสูญหายหรือเสียหาย

## Archiver and Compressing Files

Archives
ถูกใช้ในการสำรองไฟล์หรือรวมไฟล์ให้เป็นแพ็คเกจซึ่งช่วยให้สามารถถ่ายโอนเป็นไฟล์เดียวทางอินเทอร์เน็ตหรือโพสต์บนไซต์ FTP
เพื่อให้ผู้ใช้สามารถดาวน์โหลดไฟล์ได้ง่ายขึ้น

เครื่องมือที่ใช้ในการ archives ไฟล์เรียกว่า **Archiver** โดยตัวสำคํญที่ถูกใช้ใน Linux และ Unix คือ tar ซึ่งมีส่วนหน้า
GUI อยู่หลายส่วน
และยังมีโปรแกรม Archiver ตัวอื่นๆ ที่ยังสามารถใช้งานได้เช่น GNU zip (gzip), Zip, bzip และ compress

### I. Archive Files and Devices: tar

Tar ใช้สำหรับสร้างไฟล์ archive โดยที่สามารถเลือกบีบไฟล์ได้เป็นบางไฟล์, อัปเดตไฟล์,
บีบไฟล์ใหม่เข้าไปอีกทั้งยังสามารถบีบอัดไฟล์ได้ด้วยไฟล์ทั้งหมดใน Directory นั้นๆ หรือภายใน Sub-Directories ได้ด้วย

Tar ได้รับการออกแบบมาเพื่อสร้างไฟล์บับอัดบนเทป คำว่า “tar” ย่อมาจากคำว่า "Tape Archive"
ซึ่งสามารถสร้างไฟล์บีบอัดบนอุปกรณ์ใดก็ได้ เช่น Floppy Disk หรือสามารถบีบอัดไฟล์ที่ถูกบีบอัดแล้วอีกทีได้ด้วย
อีกทั้งยังเหมาะกับการที่เอาไว้บีบอัดไฟล์หลายๆ ไฟล์ให้เป็นไฟล์เดียวเพื่อใช้ในการส่งข้ามเครือข่ายอินเตอร์เน็ตอีกด้วย

มีตัวเลือกอื่นที่สามารถมาใช้แทน Tar ได้คือ **_Pax_** โดยที่ pax
เป็นเครื่องมือที่ใช้สำหรับจัดการไฟล์เอาไว้ในรูปแบบของสิ่งที่เรียกว่า archive บนระบบ Unix โดย pax
ถูกออกแบบมาเพื่อใช้งานกับรูปแบบอาร์กีฟต่างๆ เช่น cpio, bcpio, และ tar และยังสามารถสร้าง แยกแยะ และรายการ archive ได้ด้วย
pax
โดยในการใช้งาน pax จะมีประโยชน์โดยเฉพาะเมื่อต้องจัดการกับไฟล์ Archive ที่สร้างขึ้นมาบนระบบ Unix โดยมีรูปแบบที่แตกต่างกัน
![emailspin.gif](..%2Fassets%2Femailspin.gif)

#### Creating Archives

บน Linux, คำสั่ง tar มักถูกใช้สร้างไฟล์ Archive บนอุปกรณ์ โดยสามารถนำ tar
ไปสร้างไฟล์ Archive ลงในอุปกรณ์หรือไฟล์ที่ต้องการโดยใช้ตัวเลือก f พร้อมกับชื่อของอุปกรณ์หรือไฟล์ โครงสร้างของคำสั่ง tar
ที่ใช้ตัวเลือก f จะแสดงในตัวอย่างต่อไปนี้ อุปกรณ์หรือชื่อไฟล์มักจะถูกอ้างถึงในไฟล์ Archive เมื่อสร้างไฟล์สำหรับไฟล์
Archive tar
ชื่อไฟล์มักจะได้รับส่วนขยาย .tar

```shell
tar optionsf archive-name.tar directory-and-file-names
```

เพื่อสร้างอาร์กีฟใช้ตัวเลือก c ร่วมกับตัวเลือก f โดย c จะสร้างอาร์กีฟบนไฟล์หรืออุปกรณ์ เมื่อใช้ร่วมกับตัวเลือก f
คำสั่งจะมีรูปแบบดังนี้ โปรดทราบว่าไม่มีเครื่องหมายขีดก่อนตัวเลือกของ tar ตาราง 6-6 รายการตัวเลือกที่คุณสามารถใช้ได้กับ
tar ในตัวอย่างถัดไป ไดเรกทอรี mydir และไดเรกทอรีย่อยทั้งหมดของมันถูกบันทึกไว้ในไฟล์ myarch.tar ในตัวอย่างนี้ ไดเรกทอรี
mydir มีไฟล์ mymeeting และ party รวมทั้งไดเรกทอรีที่เรียกว่า reports ที่มีไฟล์ 3 ไฟล์คือ weather, monday, และ friday

```shell
$ tar cvf myarch.tar mydir

mydir/
mydir/reports/
mydir/reports/weather
mydir/reports/monday
mydir/reports/friday
mydir/mymeeting
mydir/party
```

| Option   | Description                                                                                           |
|----------|-------------------------------------------------------------------------------------------------------|
| c        | Creates a new archive.                                                                                |
| t        | Lists the names of files in an archive.                                                               |
| r        | Appends files to an archive.                                                                          |
| U        | Updates an archive with new and changed files; adds only those files modified or not already present. |
| --delete | Removes a file from the archive.                                                                      |
| w        | Waits for confirmation from the user before archiving each file; enables selective updating.          |
| x        | Extracts files from an archive.                                                                       |
| m        | When extracting a file, no new timestamp is assigned.                                                 |
| M        | Creates a multi-volume archive to be stored on several floppy disks.                                  |
| f        | Specifies the file to save the archive to, instead of the default tape device.                        |
| v        | Displays each filename as it is archived.                                                             |
| z        | Compresses or decompresses files using gzip.                                                          |
| j        | Compresses or decompresses files using bzip2.                                                         |

#### Extracting Archives

ผู้ใช้สามารถดึงไฟล์และไดเรกทอรีจากไฟล์ Archive ได้โดยใช้ตัวเลือก x และยังมีตัวเลือกเสริมดังนี้:

- **xf:** จะดึงไฟล์จากไฟล์อาร์กีฟหรืออุปกรณ์ การดึงข้อมูลด้วย tar จะสร้างไดเรกทอรีทั้งหมด

ในตัวอย่างถัดไป ตัวเลือก xf จะสั่งให้ tar ดึงไฟล์และไดเรกทอรีทั้งหมดจากไฟล์
.tar ชื่อ myarch.tar:

```shell
$ tar xvf myarch.tar

mydir/
mydir/reports/
mydir/reports/weather
mydir/reports/monday
mydir/reports/friday
mydir/mymeeting
mydir/party
```

สามารถใข้ตัวเลือก r เพื่อเพิ่มไฟล์เข้าไปในอาร์กีฟที่สร้างไว้แล้ว ตัวเลือก r จะเพิ่มไฟล์เข้าไปในอาร์กีฟ
เช่นในตัวอย่างถัดไป
ผู้ใช้เพิ่มไฟล์ในไดเรกทอรี letters เข้าไปในไฟล์ Archive ชื่อว่่า myarch.tar ที่สร้างไว้แล้ว
และนี่คือผลลัพธ์หลังจากการทำงานเสร็จโดยจะแสดงไดเรกทอรีทั้งหมดภายใน myarch.tar:

```shell
$ tar rvf myarch.tar mydocs

mydocs/
mydocs/doc1
```

#### Updating Archives

หากต้องการเปลี่ยนแปลงไฟล์ใดในไดเรกทอรีที่เคยบันทึกไว้ในไฟล์ Archive เราสามารถใช้ตัวเลือก -u เพื่อสั่งให้ tar
อัปเดตไฟล์ Archive ด้วยไฟล์ที่มีการแก้ไขล่าสุด โดยที่การทำงานของตัวเลือกนี้คือคำสั่ง tar
จะเปรียบเทียบเวลาของการอัปเดตครั้งล่าสุดสำหรับแต่ละไฟล์ที่บันทึกไว้กับไดเรกทอรีในเครื่องของผู้ใช้และคัดลอกไฟล์ที่มีการเปลี่ยนแปลงไปในไฟล์
Archive
ในตัวอย่างถัดไป ผู้ใช้อัปเดตไฟล์ myarch.tar
ด้วยไฟล์ที่มีการเปลี่ยนแปลงล่าสุด นี้หรือไฟล์ใหม่ที่สร้างขึ้นในไดเรกทอรี mydir ในกรณีนี้ ไฟล์ gifts
ถูกเพิ่มเข้าไปในไดเรกทอรี mydir

```shell
tar uvf myarch.tar mydir

mydir/
mydir/gifts
```

หากต้องการดูไฟล์ที่เก็บอยู่ในไฟล์ Archive เราสามารถใช้คำสั่ง tar พร้อมกับตัวเลือก -t โดยในตัวอย่างถัดไป
จะแสดงรายการของไฟล์ทั้งหมดที่เก็บในอาร์กีฟ myarch.tar:

```shell
tar tvf myarch.tar

drwxr-xr-x root/root 0  2000-10-24 21:38:18   mydir/
drwxr-xr-x root/root 0  2000-10-24 21:38:51   mydir/reports/
-rw-r--r-- root/root 22 2000-10-24 21:38:40   mydir/reports/weather
-rw-r--r-- root/root 22 2000-10-24 21:38:45   mydir/reports/monday
-rw-r--r-- root/root 22 2000-10-24 21:38:51   mydir/reports/friday
-rw-r--r-- root/root 22 2000-10-24 21:38:18   mydir/mymeeting
-rw-r--r-- root/root 22 2000-10-24 21:36:42   mydir/party
drwxr-xr-x root/root 0  2000-10-24 21:48:45   mydocs/
-rw-r--r-- root/root 22 2000-10-24 21:48:45   mydocs/doc1
drwxr-xr-x root/root 0  2000-10-24 21:54:03   mydir/
-rw-r--r-- root/root 22 2000-10-24 21:54:03   mydir/gifts
```

### File Compression: gzip, bzip2, and zip

## Resources

- [Archive and retrieve your data (UNIX and Linux)](https://www.ibm.com/docs/en/spectrum-protect/8.1.9?topic=clients-archive-retrieve-your-data-unix-linux)