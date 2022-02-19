<script setup>
import {ref , onMounted} from 'vue'
import soundlogo from './assets/sound.png'
import mutelogo from './assets/muteS.png'

  document.title = 'lucky jing 🍀'
//opasity style in image Group P1----------------------------------------------------

let deleteT = ' opacity: 0.5;'
let deleteF = ' opacity: 1;'

//---------------------------------------------------------------------------------------

// Array save every values
  let name = ref([]);

  // function add inputname Values --------------------------------------------------------------

  let inputName = ref('');
  function addname(){
    if(name.value.length >=0){
      inputName.value.split("\n").forEach((e)=>{
        if(e.length >0){
         name.value.push(e)
        }
      })
    
     }
     else{
       alert("input value again")
     }
    }

    // -----------------------------------------------------------------------------------

  // fucntion Reset ------------------------------------------------------------------------

  let reset = ()=> {
    name.value.length =0
    randomName.value = ''
    inputName.value = ''
    totalGroups.value.length = 0
  }

  //---------------------------------------------------------------------------------------


  // function lucky Random -----------------------------------------------------------------

    let randomName = ref('')

    let random  = (r)=>{
        if(name.value.length >0){
            for(let i in name.value){
          r = name.value[Math.floor(Math.random() * name.value.length)]
          modalLuckyShow.value = true

     
      }
        }
        else{
          alert("Please input Value in template")
        }
        
      return  randomName.value = r
    }

  //------------------------------------------------------------------------------------------

// back to menu ------------------------------------------------------------------------------

          //lucky
  let modalLuckyShow = ref(false)
  let back = ()=>{
    numberGL = ''
      modalLuckyShow.value = false
      
  }
          //GroupP1
  let modalGroupShow = ref(false)
  let backGroup = ()=>{
    numberGroup.value = 0
    numberGL.value =0
    modalGroupShow.value = false
  }
          //GroupP2
  let modalGroupShowfinal =ref(false)
  let backGroupfinal =()=>{
    modalGroupShowfinal.value = false
  }

  //----------------------------------------------------------------------------------------

  //function group mode --------------------------------------------------------------------

  let numberGroup = ref(0)
  let numberGL = ref(0)
  let totalGroups = ref([])

  let ChooseRandomGroup = ()=>{
    modalGroupShow.value = true
  }
function shuffle(array) {
    let currentIndex = array.length, randomIndex;
    while (currentIndex != 0) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;
        [array[currentIndex], array[randomIndex]] = [
            array[randomIndex], array[currentIndex]];
    }
    return array;
}

  let groupF = ()=>{ 
   if(numberGL.value > 0){
     modalGroupShowfinal.value = true
      let n  = Math.floor( name.value.length / numberGL.value)
    console.log(n)
    let member = shuffle(name.value);
    let count = numberGL.value
    for (let i = 0; i < n ; i++) {
          totalGroups.value.push(member.splice(0, count));
    }
   }
   else if (numberGroup.value > 0){
     modalGroupShowfinal.value = true
    let member2 = shuffle(name.value);
    let count2 = Math.ceil(member2.length / numberGroup.value);
    for (let i = 0; i < numberGroup.value; i++) {
        totalGroups.value.push(member2.splice(0, count2));
   }
}
else{
  alert ("Please input number in boxs")
}

  }

  //-----------------------------------------------------------------------------------------

  //sound------------------------------------------------------------------------------------

let muted = ref(false)
  onMounted(() => {
  document.querySelector('body').addEventListener('click', () => {
    document.getElementById('audio').play()
    muted.value = !muted.value
    

  })
})

 let soundMute = ()=>{
   document.getElementById("audio").muted=muted.value
   document.getElementById("thisMusic").setAttribute('src' , muted.value ? mutelogo: soundlogo)
 } 

//-------------------------------------------------------------------------------------------

// reset box in group P1 -------------------------------------------------------------------

  let resetGl = ()=>  numberGL.value = 0
  let resetNumberG = ()=> numberGroup.value = 0

//-----------------------------------------------------------------------------------------

</script>
 
<template>
<img @click="soundMute" id="thisMusic" class="audio" src="./assets/sound.png">
    <audio   id="audio" class="audio"   autoplay loop >
      <source src="./assets/sound/bg2.mp3"> 
    </audio>

  <div>

  

    <img class="mascot" src="./assets/mascot.png" >
    <a href="https://github.com/ChisanuchaK/luckyJing" target="_blank">
      <img class="logo-git" src="./assets/logo-gti.png" >
    </a>
    <img class="logo" src="./assets/logo.png">

          <div class="container">
       <img class="wood" src="./assets/wood.png">
      <h1 class="title">
          มาสุ่มกันเถอะ
      </h1>
      <textarea v-model="inputName" id="input" class="input" cols="30" rows="10" placeholder="ใส่ค่าลงในนี้ค่าต่อไป ให้กด Enter">

      </textarea>
      <div class="addReset-button">
      <button @click="addname" class="addValue" :disabled="inputName.length == 0 ? true : false">เพิ่มค่า</button>
       <button @click="reset" class="reset" :disabled="name.length == 0 && totalGroups.length == 0 ? true : false">รีเซต</button>
      </div>
        <!-- modal -->
    <div class="button">
    <button @click="random" class="lucky-mode" :disabled="name.length == 0 ? true : false" >สุ่มผู้โชคดี</button>
     <button @click=" ChooseRandomGroup" class="group-mode" :disabled="name.length == 0 ? true : false">สุ่มกลุ่ม</button>
    </div>
         <!-- modal mode -->
     <!-- modal-lucku-mode -->
      <div class="modal-bg" v-show="modalLuckyShow == true">
        <div class="modal-content"> 
          <div class="title-modal-lucky">
            <h1>ขอแสดงความยินดี</h1>
          </div>
            <h1 class="lucky-N">{{randomName}}</h1>
            <button @click="back" class="modal-lucky-ok">กลับสู่หน้าหลัก</button>
              <button @click="random" class="modal-lucky-again">สุ่มอีกครั้ง</button>
        </div>
      </div>
      <!--modal group mode  choose -->
     <div class="modal-bg-group" v-show="modalGroupShow == true" >
       <div class="chooseNumbeGroup">
         <h1>กรุณาใส่จำนวนกลุ่มหรือสมาชิกต่อกลุ่ม</h1>
            <form>
              <label class="numberGroup" for="numberGroup">จำนวนกลุ่ม</label>
              <input :disabled ="numberGL > 0 ? true : false"  v-model="numberGroup" class="inputNumberG" id="numberGroup" type="number" min="1" placeholder="0"> <br>
              <label  class="no-less" for="no-less">จำนวนสมาชิกในแต่ละกลุ่ม</label>
              <input  :disabled ="numberGroup > 0 ? true : false"  v-model="numberGL" class="Group-no-less" id="no-less" type="number" min="1" placeholder="0" > 
            </form>
            <img  :style="numberGL > 0 ? deleteT : deleteF"  @click="resetNumberG" class="delete one" src="./assets/deletenumberG.png" alt="number of group" >
              <img :style="numberGroup > 0 ? deleteT : deleteF"  @click="resetGl" class="delete two" src="./assets/deletenumberG.png" alt="number of person" >
            <button @click="backGroup"  class="modal-lucky-again">กลับหน้าหลัก</button>
            <button @click="groupF" class="modal-lucky-ok" >สุ่มกลุ่ม</button>
       </div>
      </div>

      <!--  -->

      <!-- Group mode summit -->
      <div class="modal-bg-group" v-show="modalGroupShowfinal == true">
          <div class="modal-content-group-submit">
              <ul class="GroupName" v-for="(totalGroup , index) in totalGroups" :key="index">
                  {{index+1}} : {{totalGroup}}
              </ul>
          </div>
         <button @click="backGroupfinal"   class="modal-lucky-again final">กลับหน้าหลัก</button>
      </div>

      <!--  -->
  </div>
      </div>
<!-- :style="boxs.includes(number) ? usedNumber : '' -->
  

</template>
 
<style>
 @import url(./style.css);
</style>