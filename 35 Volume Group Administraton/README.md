# 🔧 Volume Group Administration

&emsp; **Volume Group**
คือ กลุ่มของ Physical Volume ที่รวมกันเป็นก้อนๆหนึ่ง โดยภายใน Volume Group พื้นที่ว่างที่สามารจัดสรรได้
จะถูกเเบ่งเป็นหน่วยที่มีขนาดคงที่ เรียกว่า extents. </br>
&emsp; **extent** คือ หน่วยที่เล็กที่สุดของพื้นที่ที่สามารถจัดสรรได้ภายใน Physical Volume. </br>
โดยจะมีคำสังในกาจัดการต่อไปนี้. </br>

[//]: # ([![IMAGE ALT TEXT HERE]&#40;https://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg&#41;]&#40;https://youtu.be/dQw4w9WgXcQ?si=vB-JQ1_cXYx51HBb&#41;)


<hr>

- ### การสร้าง Volume Group

&emsp; &emsp; &emsp; ในการสร้าง Volume Group จาก Physical Volume เราจะทำได้โดยการเรียกใช้คำสั่ง

    # vgcreate [ชื่อ vg] [Physical Volume ที่ต้องการ] [Physical Volume ที่ต้องการ]....

Example:

- _vgcreate vg1 /dev/sdd1_
- _vgcreate vg1 /dev/sdd1 /dev/sde1_

เมื่อ Physical Volume ถูกนำไปใช้ พื้นที่ว่างของมันจะถูกเเบ่งเป็น extent 4MB โดยค่า default ซึ่งเป็นขนาดขั้นต่ำ
โดยสามารถเพิ่มหรือลดค่าได้
> [!TIP]
> - ขนาดที่ใหญ่ของ extent ไม่มีผลต่อประสิทธิภาพ I/O ของ Logical Volume.
    >
- สามารถกำหนดขนาดของ extent ได้ด้วยการใส่ option -s
>   - จำกัดจำนวน Logical Volume ใน Volume Group ได้ด้วยการ ใช้ option -l
>   - จำกัดจำนวน Physical Volume ใน Volume Group ได้ด้วยการ ใช้ option -p
> - Volume group ที่ภูกสร้างจะถูกเพิ่มเข้าไปใน /dev เช่น สร้าง vg ชื่อ test1 เมื่อทำการดูใน /dev จะพบ test1

- ### การจัดสรร LVM

&emsp; &emsp; &emsp; เมื่อ **Logical Volume Manager(LVM)** จำเป็นที่จะต้องจัดสรร Physical extents สำหรับหนึ่ง logical
volume หรือมากกว่า จะมีการจัดสรรดังต่อไปนี้ </br>
&emsp; &emsp; &emsp; - ชุดของ Physical extent ที่ไม่ได้ถูกจัดสรรจะถูกสร้างขึ้นเพื่อการพิจารณา


[//]: # (- การเรียกใช้งานและผลลัพธ์ที่ได้)

[//]: # (- แต่ละ Command ที่เกี่ยวข้องนั้น มี Options หรือ Arguments อะไรที่ควรทราบ และได้อะไรออกมา)

[//]: # (- ตัวอย่าง Code การเรียกใช้งานที่ต้องการผลลัพธ์แบบต่างๆ)

[//]: # (- อภิปรายในเรื่องที่น่าสนใจ เช่น ข้อควรระวังในการใช้งาน, Bug, หรือช่องโหว่ ฯลฯ)

[//]: # (- อื่นๆที่น่าสนใจ)

[//]: # (<p style='color:red'>This is some red text.</p>)


[//]: # ()

[//]: # (```python)

[//]: # (def example_function&#40;&#41;:)

[//]: # (    print&#40;"Hello, world!"&#41;)

[//]: # (```)

[//]: # ()

[//]: # (```scala)

[//]: # (object Hello {)

[//]: # (  def main&#40;args: Array[String]&#41; = {)

[//]: # (    println&#40;"Hello, world"&#41;)

[//]: # (  })

[//]: # (})

[//]: # (```)

[//]: # ()

[//]: # (```Unix)

[//]: # (sudo rm -rf  jjtyjtjttjtjttjth/*)

[//]: # (```)