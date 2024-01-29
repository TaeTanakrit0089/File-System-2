# Physical Volume Administration


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
