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
### คำสั่งอื่นๆ
    m แสดงเมนูคำสั่ง
    t เปลื่ยนประเภทpartition
    l แสดงรายการpartition
    u เปลื่ยนunitที่แสดง
    w เขียนตารางลงบนไดรฟ์แล้วออก
## Mkfs
### mkfs เป็นคำสั่งในการช่วยสร้างระบบไฟล์บน linux การพิมพ์ mkfs และกด tab สองครั้งจะแสดงชนิด ของระบบไฟล์ที่สามารถสร้างได้ การสร้างระบบไฟล์จะเป็นการทำลายข้อมูลใน partition นั้นๆ
![mkfstype.png](..%2Fassets%2Fimg%2Fmembers%2Fmkfstype.png)
### การสร้างระบบไฟล์ xfs ใน partition
    sudo mkfs.xfs /dev/sda1
![mkfscreate.png](..%2Fassets%2Fimg%2Fmembers%2Fmkfscreate.png)
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
## Sfdisk
### เป็นคำสั่งในการจัดการ partition เหมือนกับ fdisk แต่มีฟังกฺชั่นเสริมที่เพิ่มเข้ามาโดยเฉพาะที่ไม่มีใน fdisk เช่น การทำงานแบบ non-interactively
### คำสั่งแสดงขนาดของ partition
    sfdisk -s /dev/sda1 **ถ้าไม่มีการใส่partitionจะแสดงขนาดของทุกๆdiskและtotalsize**
![sfdisksize.png](..%2Fassets%2Fimg%2Fmembers%2Fsfdisksize.png)
### การแสดง partition
    sfdisk -l /dev/sda
![sfdiskshow.png](..%2Fassets%2Fimg%2Fmembers%2Fsfdiskshow.png)
### การสร้าง partition
    sfdisk /dev/sda
### คำสั่งอื่นๆ
    -v versionของsfdisk
    -h help
    -t แสดงtype
    -N การเปลื่ยนแค่partitionเดียวโดยใช้ลำดับในการเลือก
    -A activate
### ข้อมูลน่ารู้อื่นๆ
    - การใช้คำสั่งจัดการpartitionต่างๆ ต้องใช้อย่างระมัดระวังเพราะการสร้าวหรือลบอะไรถ้าผิดพลาดอาจทำให้ข้อมูลสูญหายได้
    - การใช้คำสั่ง --force สามารถผ่านขั้นตอนการเช็คต่างๆก่อนที่จะเขียนทับหรือลบpartitionได้
    - สามารถใช้คำสั่งจัดการpartitionต่างกันในการจัดการได้เช่นการสร้างpartitionด้วยfdiskแต่แสดงผลผ่านทางsfdiskได้
      เพราะเป็นแค่คำสั่งจัดการซึ่งข้อมูลหรือรายละเอียดต่างๆในpartitionถูกเปลื่ยนไปแล้ว
    - fdisk มักจะมีในระบบส่วนใหญ่แต่ parted เริ่มหายไปทำให้การจะเรียกใช้จำเป็นต้องทำการติดตั้งจากข้างนอก
    - start และ end ในการแสดงpartitionกำหนดตำแหน่งเริ่มและตำแหน่งสิ้นสุดของpartitionซึ่งจะมีคววามกว้างเท่ากับsize
    - character device คือ อุปกรณ์ที่ติดต่อข้อมูลในรูปแบบตัวอักษรโดยไม่มีรูปแบบแบบบล็อค
    - ไฟล์ที่เป็น character device ถูกเก็บในdirectory หรือ /dev
### References:
    https://en.wikipedia.org/wiki/Raw_device
    https://saixiii.com/fdisk-linux-command/
    https://www.interserver.net/tips/kb/commands-check-hard-disk-partitions-disk-space-linux/
    https://oarkm.oas.psu.ac.th/blog/24
    https://ciksiti.com/th/chapters/12268-working-with-linux-mkfs-command
    https://www.howtogeek.com/443342/how-to-use-the-mkfs-command-on-linux/
    https://gcore.com/learning/manage-disk-partitions-linux-parted-command/
    https://th.linux-console.net/?p=1279
    https://www.computerhope.com/unix/sfdisk.htm