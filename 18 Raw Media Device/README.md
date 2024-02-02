Raw Media Device
===
---
# Raw Media Device ใน linux
### เป็น Character Device ที่สามารถใช้เข้าถึง Block Device(disk,partition) ช่วยเพิ่มประสิทธิภาพและลดเวลาการเข้าถึงลงสำหรับแอปพลิเคชั่นที่ต้องการที่ต้องการเข้าถึง Storage โดยตรง

# การทำงานของ Raw Media Device 
### Raw Media Device จะทำให้สามารถเข้าถึง Storage Device เช่น disk,partition ได้โดยตรงโดยไม่ผ่านระบบไฟล์ หรือ cache และ buffer ของ Operation System(OS) ทำให้ทำงานได้รวดเร็ว โดย Raw Media Device อาจสื่อถึง Disk หรือ Partition ที่ไม่มี format ของไฟล์ใดๆเลย

![Raw_Media_Device_1.jpg](..%2Fassets%2Fimg%2Fmembers%2FRaw_Media_Device_1.jpg)


# Command ที่เกี่ยวข้องกับ Raw Media Device
## /dev/sda
### /dev คือที่เก็บข้อมูลใน root folder ที่เก็บไฟล์ทั้งหมดของอุปกรณ์ ซึ่งระบบจะทำการสร้างไฟล์ทันทีที่ติดตั้ง /dev/sda คือ disk ของคอมพิวเตอรที่เรากำลังใช้อยู่ ตามปกติ /dev/sda จะเป็น disk ลำดับแรก
## Fdisk
### คำสั่ง fdisk เป็นคำสั่งที่ใช้ในการจัดการ partition ซึ่งสามารถสร้างหรือลบ partition รวมถึงแสดง partiton เพื่อเรียกดูรายละเอียดที่เกี่ยวข้องต่างๆได้ การจะใช้คำสั่ง fdisk ต้องใช้มีสิทธิ์เป็น root
### การสร้าง partition ใหม่
    sudo fdisk /dev/sda 
    n #สร้างpartitionใหม่
### การลบ partition
    sudo fdisk /dev/sda 
    d #ลบpartition
### การแสดง partition
    sudo fdisk /dev/sda 
    p #แสดงข้อมูลpartitionปัจจุบัน
![newfdiskcommand.png](..%2Fassets%2Fimg%2Fmembers%2Fnewfdiskcommand.png)
## Mkfs
### mkfs เป็นคำสั่งในการช่วยสร้างระบบไฟล์บน linux การพิมพ์ mkfs และกด tab สองครั้งจะแสดงชนิด ของระบบไฟล์ที่สามารถสร้างได้ การสร้างระบบไฟล์จะเป็นการทำลายข้อมูลใน partition นั้นๆ
![mkfstype.png](..%2Fassets%2Fimg%2Fmembers%2Fmkfstype.png)
## Parted
### parted เป็นคำสั่งที่ใช้ในการจัดการ partition เหมือนกับ fdisk โดย parted จะสามารถใช้งานได้ง่ายกว่าแต่ช่วงหลังๆ parted เริ่มหายไปต้องทำการติดตั้งถึงจะใช้งานได้  
### การแสดง partition ปัจจุบัน
    sudo parted /dev/sda
    print
![partedshow.png](..%2Fassets%2Fimg%2Fmembers%2Fpartedshow.png)
### การสร้าง partition
    mkpart primary ext4 1GB 5GB 
    #สร้างpartition primary ext4 เริ่มต้น1gb-5gb
### การลบ partition
    rm 1
    #1 คือเลขของpartition ใส่เลขของpartiotionตามที่ต้องการจะลบ
### การปรับขนาดของ partition
    resizepart 1 10gb
    #1 คือเลขของpartition 10gb คือขนาดที่ต้องการ
    
### References:
    https://en.wikipedia.org/wiki/Raw_device
    https://saixiii.com/fdisk-linux-command/
    https://www.interserver.net/tips/kb/commands-check-hard-disk-partitions-disk-space-linux/
    https://oarkm.oas.psu.ac.th/blog/24
    https://ciksiti.com/th/chapters/12268-working-with-linux-mkfs-command
    https://www.howtogeek.com/443342/how-to-use-the-mkfs-command-on-linux/
    https://gcore.com/learning/manage-disk-partitions-linux-parted-command/