# 🔧 Volume Group Administration

&emsp; **Volume Group**
คือ กลุ่มของ Physical Volume ที่รวมกันเป็นก้อนๆหนึ่ง โดยภายใน Volume Group พื้นที่ว่างที่สามารจัดสรรได้
จะถูกเเบ่งเป็น unit ที่มีขนาดคงที่ เรียกว่า extents. </br>
&emsp; **extent** คือ unit ที่เล็กที่สุดของพื้นที่ที่สามารถจัดสรรได้ภายใน Physical Volume โดย extents จะถูกเรียกเป็น Physical extents. </br>
</br>

[//]: # ([![IMAGE ALT TEXT HERE]&#40;https://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg&#41;]&#40;https://youtu.be/dQw4w9WgXcQ?si=vB-JQ1_cXYx51HBb&#41;)


<hr>

## การสร้าง Volume Group

&emsp; &emsp; &emsp; ในการสร้าง Volume Group จาก Physical Volume เราจะทำได้โดยการเรียกใช้คำสั่ง

    # vgcreate [ชื่อ vg] [Physical Volume ที่ต้องการ] [Physical Volume ที่ต้องการ]....

Example:

- _vgcreate vg1 /dev/sdd1_
- _vgcreate vg1 /dev/sdd1 /dev/sde1_

เมื่อ Physical Volume ถูกนำไปใช้ พื้นที่ว่างของมันจะถูกเเบ่งเป็น extent 4MB โดยค่า default ซึ่งเป็นขนาดขั้นต่ำ
โดยสามารถเพิ่มหรือลดค่าได้
> [!TIP]
> - ขนาดที่ใหญ่ของ extent ไม่มีผลต่อประสิทธิภาพ I/O ของ Logical Volume.
  >- สามารถกำหนดขนาดของ extent ได้ด้วยการใส่ option `-s`
> - จำกัดจำนวน Logical Volume ใน Volume Group ได้ด้วยการ ใช้ option `-l`
> - จำกัดจำนวน Physical Volume ใน Volume Group ได้ด้วยการ ใช้ option `-p`
> - Volume group ที่ภูกสร้างจะถูกเพิ่มเข้าไปใน `/dev` เช่น สร้าง vg ชื่อ test1 เมื่อทำการดูใน /dev จะพบ test1


<hr>

## การจัดสรร LVM

&emsp; &emsp; &emsp; เมื่อ **Logical Volume Manager(LVM)** จำเป็นที่จะต้องจัดสรร Physical extents สำหรับหนึ่ง logical
volume หรือมากกว่า จะมีการจัดสรรดังต่อไปนี้ </br>
- ชุดของ Physical extent ที่ไม่ได้ถูกจัดสรรถูกสร้างขึ้นเพื่อการพิจารณา ถ้าหากระบุ ranges ของ Physical Volume ไว้ที่ท้าย 
commmand line จะทำให้มีเพียงเเต่ Physical extents ที่ไม่ได้จัดสรร ที่อยู่ใน ranges นใน Physical volumes ที่ระบุเท่านั้นที่จะถูกนำมาพิจารณา </br>
</br>
- เเต่ละ policy ของการจัดสรร จะพยายามจัดลำดับ โดยเริ่มด้วย policy ที่เข้มงวดที่สุด (_contiguous_) และ จบด้วย policy 
การจัดสรรที่ ใช้ option `--alloc` หรือ ตั้งเป็นค่า default สำหรับ logical volume หรือ volume group เฉพาะสำหรับเเต่ละ policy, ทำงานจาก 
logical extent ที่หมายเลขต่ำสุด ของพื้นที่ logical volume ที่ว่าง ที่ต้องการบรรจุ โดยจัดสรรพื้นที่ให้ได้มากที่สุด, ตามข้อจำกัดของ policy การจัดสรร เเละ
ถ้าหากต้องการพื้นที่เพิ่ม LVM จะไปยัง policy ถัดไป.
<br>


- **ข้อจำกัดของ policy การจัดสรร**
  - Policy ของการจัดสรรเเบบ `contigous` กำหนดให้ตำเเหน่ง physical ของ logical extent ใดๆที่ไม่ใช่ logical extent ตัวเเรกของ logical volume ที่อยุ่ติด
ตำเเหน่ง physical ของ logical extent ก่อนหน้านั้นทันที, เมื่อ logical volume ถูก striped หรือ mirrored ข้อจำกัดการจัดสรร `contigous` จะถูกนำไปใช้กับเเต่ละ stripe หรือ mirror image ที่ต้องการพื้นที่.
  - Policy ของการจัดสรรเเบบ `cling` กำหนดไว้ว่า Physical volume ที่ถูกใช้โดย logical extent จะถูกเพิ่มเข้าไปยัง logical volume ที่ถูกใช้เเล้ว
  อย่างน้อยหนึ่ง logical extent ก่อนหน้านั้นใน logical volume นั้น. หากมีการกำหนด parameter `allocation/cling_tag_list` Physical volume
  สองตัวจะถือว่า match กัน ถ้าหากมี tag ใดๆ บนทั้งสอง Physical volume. ซึ่งช่วยให้ groups ของ Physical Volume ที่มีคุณสมบัติคล้ายกัน (เช่น ตำเเหน่ง physical)
  สามารถ tag เเละถือว่าเทียบเท่ากัน ในการจัดสรร





[//]: # (    อาจเพิ้มให้ไปอ่านต่อในส่วนของนนท์ ****************************) 
    
    

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