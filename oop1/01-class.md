# OOP: Class, static, constructor, getter & setter and visiblity

ปกติจะเขียนลงเฟส แต่รอบนี้ท่าจะยาว เลยขอเขียนลง GitHub แล้วกัน

## Class?

วิธีหนึ่งในการเขียนโปรแกรมที่ทำให้สะดวกขึ้นคือการมองทุกอย่างในโลกเป็นวัตถุ (Object-oriented programming) ซึ่งไม่ใช่วิธีเดียว ยังมีอีกหลายรูปแบบแต่ Java นั้นบังคับให้เราใช้แบบนี้

คลาสคือวัตถุ ปกติเค้าจะเปรียบเทียบกันกับรถ เช่น

- public class **Car**
  - Class variables 
    - private int **speed**
    - private String **model**
  - Methods
    - public void **drive(int speed)**
    - public void getCurrentSpeed()

## Visibility
ทีนี้จะมากล่าวถึง visibility กัน อันนี้เคยมีใน slide ไปแล้ว ยกมาอีกทีแล้วกัน

- **public** คือ ทุก method สามารถเข้าใช้ได้หมด
- **private** คือ เฉพาะ method ในคลาสเดียวกันสามารถเข้าใช้ได้หมด
- **protected** คือ เฉพาะ method ในคลาสกลุ่มเท่านั้นที่ใช้ได้ (ต่างกับ `private` ตรงที่คลาสย่อยก็เข้ามายุ่งไม่ได้ -- อันนี้ไว้ถึงเรื่อง Inheritance น่าจะเข้าใจมากขึ้น ตอนนี้เอาเป็นว่ามันเหมือน `private`)

ลองมาดูตัวอย่างกัน

~~~~~~java
public class Car{
    public int speed;
}
public class CarPark{
    public static void main(String[] args){
        Car prius = new Car();
        prius.speed = 50;
    }
}
~~~~~~

<p style='color: #ccc;'>(เวลารันจริง คลาสแต่ละตัวที่มีคำว่า `public` นำต้องอยู่ในไฟล์ชื่อ `ClassName.java` เอามากองๆ แบบนี้ไม่ได้)</p>

เราจะพบว่าโค้ดนี้รันได้ ถ้าลอง `System.out.println(prius.speed);` จะได้ 50 จริง ทีนี้ถ้าลองเปลี่ยน `speed` เป็น `private`/`protected` จะ error เพราะอยู่คนละคลาสกัน

### FAQ

#### ใช้ทำไม
เหตุผลที่ต้องมี visiblity เพราะโค้ดไม่ได้เขียนคนเดียว เวลาทำโปรแกรมหลายคน คนอื่นอาจจะมาใช้คลาสของเรา ตรงไหนที่เราไม่อยากให้เขาใช้ก็ตั้ง `private` ไว้ ส่วน `public` นั้นไม่ควรจะเปลี่ยน method signature

#### `public` หมดเลยได้มั้ย
ไม่มีข้อห้าม แต่ข้อแนะนำคือ*ตัวแปร*ควรจะเป็น `private` ไว้ (รออ่านในหัวข้อ setter ว่าทำไม)

#### ยาวไปไม่อ่าน
ตอนนี้เอาเป็นว่า

- method ใช้ `public`
- ตัวแปรใช้ `private` แล้วเขียนเมธอดมาคืนค่า

## Static variable

ทีนี้เราจะพบว่าเราเคยใช้ `static` มันคืออะไร

ตัวแปรชนิด `static` คือสิ่งที่**แชร์**ร่วมกันระหว่างคลาสชนิดเดียวกันทั้งหมด ถึงชื่อจะแปลว่าคงที่แต่ก็สามารถเปลี่ยนค่าได้ ตัวอย่างเช่น Car ที่อาจจะแชร์รวมกันได้คือ `String[] laws` (รถทุกคันมีกฎจราจรร่วมกัน เวลาออกกฎใหม่รถทุกคันก็ยังต้องปฏิบัติตามกฎเดียวกัน) แต่จะบอกว่า `String wheel` แชร์รวมกันคงไม่ใช่ เพราะรถทุกคันมีล้อจริง แต่ไม่ได้แปลว่ารถเรากับรถเพื่อนใช้ยางเส้นเดียวกัน

เราพบว่าหลายคนเขียน local variable เป็น static class variable หมด อันนี้ไม่ควรทำ ลองนึกดูว่า ถ้ามีคลาสของเราทำงานพร้อมกันสัก 3 อัน ตัวแปรมันคงทับกันไปกันมา

เวลาคิดว่าจะใช้  static หรือไม่ หรือจะ local คือ

- สมมุติว่ามี class นี้อยู่หลายๆ อัน มันควรจะแชร์ค่านี้แน่ๆ มั้ย ถ้าใช่แน่ๆ เป็น static
- ตัวแปรนี้ใช้ในหลายเมธอดมั้ย ถ้าใช่เป็น class variable
- ถ้าไม่ใช่เลย เป็น local

เช่น ข้อ Student 

- `int score`, `Student stud` เป็น local ใน `main`
- `int totalScore`, `int countScore`, `String name` เป็น class variable
- `Scanner scan` เป็น static

(อาจจะไม่เก็บตัวแปรตามนี้ ก็ไม่ได้แปลว่าผิด เป็นแนวทางเฉยๆ)

## this

ทำไมบางทีใส่ `this` บางทีไม่ต้อง?

ลองดูตัวอย่างนี้

~~~~~java
public class Car{
    private int speed = 0;
    public Car(int speed){
        this.speed = speed;
    }
    public int getSpeed(){
        return speed;
    }
    public static void main(String[] args){
        Car car = new Car(200);
        System.out.println(car.getSpeed());
    }
}
~~~~~

ลองรันดู แล้วลองเอา `this` ออกดู

ผลของ `this` คือบังคับว่านี่จะกล่าวถึง **class variable** เท่านั้น ไม่อย่างนั้นถ้ามี local variable ชื่อซ้ำกันก็จะกล่าวถึงตัวแปร local นั้นแทน

ใน Java ไม่บังคับใส่ บางภาษา เช่น JavaScript, PHP, Python บังคับต้องใส่ `this` (หรือ `self` ใน Python) เสมอ ฉะนั้นแล้วเราขอแนะนำว่าควรใช้ `this` ให้เป็นนิสัย

## Constructor

เวลาสร้างคลาส จะมีเมธอดตัวหนึ่งที่กำหนดค่าเริ่มต้น หรือเตรียมงานของคลาส เรียกว่า Constructor

Constructor จะใช้รูป `public ClassName()` เสมอ  (เหมือนเมธอด แต่ไม่มี type) และสามารถมี arguments ได้ตามต้องการ เช่น

~~~~~java
public Car(){
}
public Car(int speed){
    this.speed = speed;
}
~~~~~
ในตัวอย่างนี้เรามี constructor ของคลาส `Car` 2 ตัว ใช้เหมือน method overloading

- `Car prius = new Car();` เรียกตัวบน
- `Car vios = new Car(30);` เรียกตัวล่าง เพราะเราส่ง int เข้าไปในวงเล็บ

เวลาเรียก Constructor เรียกเหมือนเมธอดแต่ต้องมีคำว่า `new`นำหน้าเสมอ

ถ้า Constructor เปล่าๆ (เหมือน `Car()` ในตัวอย่าง) ไม่ต้องใส่ก็ได้ แต่ยังต้อง `new` เรียกเหมือนเดิม ตัวอย่างเช่น ถ้าเราลบ `public Car()` ในโค้ดข้างบนออกไปเลย เรายังต้องใช้ `Car soluna = new Car();` อยู่ในการสร้าง object ใหม่

## Getter & Setter

Getter คือตัวเรียกข้อมูล Setter คือตัวกำหนดค่าข้อมูล

ด้วยข้อจำกัดของ Java ที่ไม่มีระบบ Getter & Setter ในตัว เราเลยไม่ควรเรียกค่าตัวแปรจากตัวแปรตรงๆ (กลับไปดูโค้ดตัวอย่างในหัวข้อ Visibility แบบนั้นไม่ควรทำ) แต่ควรเขียนเมธอดที่คืนค่าตัวแปร (และตัวแปรจริงเป็น `private`) โดย

- **Getter** ตัวเรียกข้อมูล ใช้ชื่อ `getVariableName()` (เปลี่ยนไปตามชื่อตัวแปรนั้นๆ)
- **Setter** ตัวกำหนดข้อมูล ใช้ชื่อ `setVariableName(type val)`

เหตุผลที่ควรเขียนคือ สมมุติโปรแกรมเรามีขนาดใหญ่มาก วันดีคืนดีเราจำเป็นจะต้องย้ายตัวแปรนี้ไปเก็บไว้ในฐานข้อมูล เราจึงไม่สามารถเรียกตัวแปรตรงๆ ได้แล้วแต่ต้องเรียกเมธอด ถ้าเราเขียน method ไว้แต่แรกแล้วเพียงแค่แก้ `getVariableName()` ให้ดึงจากฐานข้อมูล ที่เหลือในโปรแกรมก็ไม่ต้องแก้ ส่วนถ้าแก้จาก variable ธรรมดาๆ ทำแบบนี้ไม่ได้

หรืออาจจะเป็นการกระทำอื่นๆ ก็ได้ ในโปรแกรมเล็กๆ อาจจะได้ใช้ เช่น

- เรียงข้อมูลตอนเซฟ ตอนเรียกข้อมูลจะได้ไวๆ ไม่ต้องเรียงทุกครั้ง (โดยทั่วไปเรามักจะเรียกข้อมูลบ่อยกว่าเซฟ)
- มีการกระทำเพิ่มเติม เช่น เก็บว่าเปลี่ยนตัวแปรไหนไปบ้าง เก็บว่าแก้ไปแล้วกี่ครั้ง
- บังคับการใช้ เช่น ทีมปิงปองอาจจะมี `addScore` แทนที่จะมี `setScore` เพื่อบังคับว่าคะแนนเพิ่มแล้วห้ามลด

ในภาษาอื่นๆ เช่น C#, Python มีรูปแบบที่ทำให้เราเขียน method แบบนี้ได้ แต่ตอนเรียกใช้แค่กำหนดค่าไปในตัวแปรตรงๆ ได้เลย

## สรุป

- ตัวแปรใช้ `private` นำ
- เมธอดใช้ `public` นำ
- เขียนเมธอด `getVariableName()` ให้ตัวแปรทุกตัว (หรือตัวที่จะใช้)
- เขียนเมธอด `setVariableName(type val)` ให้ตัวแปรที่จะมีการกำหนดค่า
- `this` ใช้เรียก class variable ถ้าไม่ใส่อาจหมายถึง local variable ถ้ามีชื่อซ้ำกัน
- `static` ใช้แชร์ตัวแปรระหว่างคลาสเดียวกันหลายๆ ตัว