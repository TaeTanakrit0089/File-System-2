# Physical Volume Administration

เป็นการจัดการ **Physical Volume** ทั้งหลายใน Hardisk สามารถที่จะ สร้าง, เพิ่ม/ลดขนาด, ลบ, ป้องกันการเขียนข้อมูล ของ **Physical Volume** ได้ และสามารถใช้ในการนำไปทำ **Volume Group Administrator** หรือ **Logical Volume Administrator** ต่อได้ แต่ก่อนอื่นเราต้องสร้าง และกำหนด **Physical Volume** ขึ้นมาก่อน

## Physical Component
Physical Storage Unit เป็นโครงสร้างพื้นฐานของ LVM Logical Volume ที่เป็น Block Device โดยอาจเป็น Partition หรือ Disk ทั้งหมด, เพื่อให้ Device ใช้ LVM Logical Volume ได้นั้น ตัว Device ต้องเริ่มระบุเป็น **Physical Volume (PV)**, การเริ่มการทำ Block Device โดยการทำ **Physical Volume** นั้นเป็น Label ใกล้กับตอนการเริ่มการทำงานของ Device

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

#References

<a href="https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/index" target="_blank">
<picture>
  <img alt="" src="https://access.redhat.com/webassets/avalon/d/Red_Hat_Enterprise_Linux-7-Logical_Volume_Manager_Administration-en-US/images/fb83bf56728805639af6b760fac589d0/title_logo.png">
</picture>
</a>

