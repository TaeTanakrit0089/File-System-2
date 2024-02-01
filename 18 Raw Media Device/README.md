Raw Media Device
===
---
# Raw Media Device ใน linux
### เป็น Character Device ที่สามารถใช้เข้าถึง Block Device(disk,partition) ช่วยเพิ่มประสิทธิภาพและลดเวลาการเข้าถึงลงสำหรับแอปพลิเคชั่นที่ต้องการที่ต้องการเข้าถึง Storage โดยตรง

# การทำงานของ Raw Media Device 
### Raw Media Device จะทำให้สามารถเข้าถึง Storage Device เช่น disk,partition ได้โดยตรงโดยไม่ผ่านระบบไฟล์ หรือ cache และ buffer ของ Operation System(OS) ทำให้ทำงานได้รวดเร็ว โดย Raw Media Device อาจสื่อถึง Disk หรือ Partition ที่ไม่มี format ของไฟล์ใดๆเลย

![Raw_Media_Device_1.jpg](..%2Fassets%2Fimg%2Fmembers%2FRaw_Media_Device_1.jpg)

# Command ที่เกี่ยวข้องกับ Raw Media Device
## Fdisk
### คำสั่ง fdisk เป็นคำสั่งที่ใช้ในการจัดการ partition ซึ่งสามารถสร้างหรือลบ partition รวมถึงแสดง partiton เพื่อเรียกดูรายละเอียดที่เกี่ยวข้องต่างๆได้ การจะใช้คำสั่ง fdisk ต้องใช้มีสิทธิ์เป็น root
### การสร้าง partition ใหม่
    sudo fdisk /dev/sda #เข้าใช้การจัดการpartitionในdiskแรกของอุปกรณ์
    n #สร้างpartitionใหม่
### การลบ partition
    sudo fdisk /dev/sda 
    d #ลบpartition
### การแสดง partition
    sudo fdisk /dev/sda 
    p #แสดงข้อมูลpartitionปัจจุบัน
### References:
    https://en.wikipedia.org/wiki/Raw_device
    https://saixiii.com/fdisk-linux-command/
    https://www.interserver.net/tips/kb/commands-check-hard-disk-partitions-disk-space-linux/