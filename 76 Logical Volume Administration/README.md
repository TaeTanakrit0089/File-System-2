# Logical Volume Administration

การใช้ **Physical Storage** ตรงๆ นั้นอาจจะไม่ยืดหยุ่น และจัดการได้ยาก เช่น มี SSD 128GB 3 อัน ต้องการให้ User1 กับ User2
ใช้คนละครึ่งกัน ทำยังไง?<br><br> ด้วยปัญหานี้ เราจึงต้องสร้าง **Logical Storage** ขึ้นมา เพื่อให้เราสามารจัดการกับ *
*Physical Storage** ได้ง่ายขึ้น <br><br>

## What is Logical Storage ?

**Logical Storage** คือ Storage ที่ถูกสร้างขึ้นด้วย Software จากการนำเอา **Physical Storage** มารวมกัน เป็น **Volume
Group** จากนั้นนำมาแบ่งสร้างเป็น **Logical Storage** (ขอยกตัวอย่างโดยภาพจาก Window 10)

<img alt="" src="https://www.easeus.com/images/en/screenshot/partition-manager/windows-10-c-drive-full.jpg">

แหล่งรูปภาพ: https://www.easeus.com/images/en/screenshot/partition-manager/windows-10-c-drive-full.jpg

ภาพตัวอย่างด้านบนนั้นเป็น **Logical Storage** ที่ถูกสร้างขึ้นจาก **Physical Storage** 2 อัน (SSD 250GB 2 อัน)
จะเห็นได้ว่า **Logical Storage** ที่สร้างขึ้น สามารถปรับแต่งได้ตามที่เราต้องการ เช่น จำกัดการเข้าถึง User บางกลุ่มกับ *
*Logical Storage** บางอัน ปรับขนาดเพิ่ม/ลด ได้อิสระ

## Logical Volume Management in Linux

**Linux** มีเครื่องมือที่เรียกว่า **Logical Volume Management (LVM)** ช่วยจัดการเกียวกับ **Logical Storage**

<br>
<img  src="https://www.cyberciti.biz/media/new/faq/2018/08/Shows-information-about-available-LVM-logical-volumes.png" alt="">

