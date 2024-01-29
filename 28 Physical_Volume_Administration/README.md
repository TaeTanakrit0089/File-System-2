# Physical Volume Administration
เป็นการจัดการ Physical Volume ทั้งหลายใน Hardisk เป็นการ สร้าง, เพิ่ม/ลดขนาด, ลบ, ป้องกันการเขียนข้อมูล ของ Physical Volume ได้

## Physical Component

โดย Physical Volumes ก็เป็นอีกหนึ่ง Component ที่สำคัญในการจัดการ Disk

ก่อนที่จะเข้าไปจัดการ Physical Volumn ได้นั้น เราต้องลง Logical Volumn Manager (LVM) ใน Linux ก่อน เพื่อให้ใช้งานคำสั่งได้
แต่ถ้าใช้ Ubuntu Server จะสามารถใช้งาน LVM ได้เลย โดยที่เราไม่ต้องลง Package ของ LVM
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

