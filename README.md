# <img src="./src/assets/img/logo.png" width="50">  luckyJing 🍀´ˎ˗
Project INT203 Client-side Web Programming II 

## ˗ˏˋ 🍀 About's luckyJing ´ˎ˗
<img src="./src/assets/img/mascot.png" width="200">

luckyJing เป็น Web Application เล็ก ๆ ที่มีการนำเอาความรู้ในรายวิชา INT203 Client-side Web Programming II มาใช้ในการจัดทำ
โดยความสามารถของ luckyJing จะมีอยู่ 2 function
1. สุ่มผู้โชคดี
    * จะสุ่มผู้โชคดีจากรายชื่อที่ผู้ใช้ได้ทำการกรองเข้าไปมา 1 คน 
2. จัดกลุ่ม
    * สุ่มกลุ่มแบบเลือกจำนวนกลุ่ม
    * สุ่มกลุ่มแบบเลือกจำนวนสมาชิกภายในกลุ่ม
    
 ## 🍀 Prerequisites ᵎᵎ 
 * Download and install `node` per [this](https://nodejs.org/en/download/)
    * This will install npm as well
 * As an alternative to `npm` you can use `yarn`. If you choose to do so, you can find installation instructions [here](https://classic.yarnpkg.com/en/)
 * Vue CLI - You will find instructions [here](https://cli.vuejs.org/)
 ### Optional (but highly recommended)
* Install [Git](https://git-scm.com/downloads)
* Install [Visual Studio Code](https://code.visualstudio.com/)
* Install [Google Chrome](https://www.google.com/chrome/index.html)
    * Install the [Vue.js devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=en)
   
## 🍀 Getting started ⤾·˚
First `git-clone` or [download](https://github.com/ChisanuchaK/luckyJing/tree/main) this repository
```
git clone https://github.com/ChisanuchaK/luckyJing.git
```
Then open a terminal, `cd` to the directory where you cloned this repository.

Make sure `node`/`npm` (or `yarn` if you have it installed) are in your path!
```
npm install # or `yarn install`
```
## 🍀 Running the app ♡   ·͜·♡
```
npm run dev
```
Open http://localhost:8080/ to see the site locally. Changes to assets will rebuild the site. Refresh to see your changes.

## 🍀 User Manual ✧ﾟ
* <img src="./src/assets/img/sound.png" width="30"> 
   สำหรับเปิด / ปิด Background Music 
* ช่องสำหรับกรอกรายชื่อของผู้ที่เข้าร่วมการสุ่ม
* กด `เพิ่มค่า` เพื่อยืนยันการกรอกชื่อ
  * จะมีการแสดงจำนวนทั้งหมวดที่ได้เพิ่มรายชื่อเข้าไป โดยจะไม่เพิ่มจำนวนหากมีรายชื่อที่ซ้ำ (หมายเหตุถ้าผู้เข้าร่วมชื่อซ้ำกันควรจะมีวิธีการเขียนที่แตกต่างกัน เช่น มิ้น มิ้นท์ มิ้นต์ เป็นต้น)
  * กด `จำนวนค่าทั้งหมด` เพื่อตรวจเช็ครายชื่อทั้งหมดที่ได้กรอกเข้าไปว่าถูกต้องหรือไม่
  * การแสดง `รายชื่อทั้งหมด`จะสามารถลบรายชื่อได้ตามที่ต้องการ เพียงแค่คลิกไปยังรายชื่อที่ต้องการนำออก
* กด `สุ่มผู้โชคดี` เพื่อสุ่มผู้โชคดี 1 คนจากจำนวนทั้งหมดที่ได้ทำการกรอกรายชื่อไป
  * สามารถกด `สุ่มอีกครั้ง` เพื่อเลือกผู้โชคดีคนใหม่
  * `กลับสู่หน้าหลัก` จะไปยังหน้าแรกเพื่อใช้งาน function อื่น
* กด `สุ่มกลุ่ม` เพื่อทำการสุ่มกลุ่ม
  * เลือก `จำนวนกลุ่ม` หมายถึง จำนวนของกลุ่มที่ต้องการจากสมาชิกที่กรอกมาทั้งหมด
  * เลือก `จำนวนสมาชิกในกลุ่ม` หมายถึง จำนวนสมาชิกที่ต้องการในแต่ละกลุ่ม
  * หากกลุ่มที่สุ่มได้ไม่ตรงตามเงื่อนไข ***กรณีที่เหลือเศษ*** จะมี alert แจ้งให้ผู้ใช้ทราบ
  * เลือก  <img src="./src/assets/img/deletenumberG.png" width="15">  เพื่อรีเซตค่า
  *  `กลับสู่หน้าหลัก` จะไปยังหน้าแรกเพื่อใช้งาน function อื่น และ function สุ่มกลุ่มจะปิดใช้งานจนกว่ากดปุ่ม `รีเซต`
  *  กด <img src="./src/assets/img/history.png" width="20"> เพื่อดูประวัติการสุ่มกลุ่มที่สุ่มไปและประวัติการสุ่มผู้โชคดี
  *  กด <img src="./src/assets/img/logo-gti.png" width="20"> เพื่อไปยัง GitHub this repository.
* กด `รีเซต` เพื่อเริ่มใหม่

## ‧₊˚ About Us 🖐🏻 ˊˎ -
งานนี้เป็นส่วนของวิชา INT203 Client-side Web Programming II ภาคเรียนที่ 2 ปีการศึกษา 2564 คณะเทคโนโลยีสารสนเทศ มหาวิทยาลัยเทคโนโลยีพระจอมเกล้าธนบุรี

🍀Team : luckyJing 𓇼
| StudentID     | Name     | 
| ------------- |:-------------:| 
|63130500022    |CHINNAWAT KAEWNONGSANG
|63130500023    |CHISANUCHA SOMBOONWANNA
|63130500024    |CHOKJAROEN TANCHAROENRAT
|63130500031    |NATTHAKAN THAWEEWATTHANAPRAYUN
|63130500054    |THARADON SAENMART
