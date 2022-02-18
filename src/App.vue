<script setup>
import {ref , onMounted} from 'vue'
import soundlogo from './assets/sound.png'
import mutelogo from './assets/muteS.png'

  document.title = 'lucky jing 🍀'
// Array name
  let name = ref([]);
  // inputname

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
  // fucntion Reset
  let reset = ()=> {
    name.value.splice(0 , name.value.length) 
    randomName.value = ''
    inputName.value = ''
  }

  // function lucky Random
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


// back to menu 
  let modalLuckyShow = ref(false)
  let back = ()=>{
      modalLuckyShow.value = false
  }

  let modalGroupShow = ref(false)
  let backGroup = ()=>{
    numberGL.value = ''
    numberGroup.value = ''
    modalGroupShow.value = false
  }
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

  //function group mode
  let numberGroup = ref()
  let numberGL = ref()
  let totalGroup = ref([])

  let ChooseRandomGroup = ()=>{
    modalGroupShow.value = true
  }
  let randomGroup = ()=>{
    if(name.value.length !=0){
       let count = Math.ceil(numberGL.value.length / numberGroup.value);
    for (let i = 0; i < name.value; i++) {
      totalGroup.value.push(name.value.splice(0, count));
    }
    }
  }


</script>
 
<template>
<img @click="soundMute" id="thisMusic" class="audio" src="./assets/sound.png">
    <audio   id="audio" class="audio"   autoplay loop >
      <source src="./assets/sound/bg.mp3"> 
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
       <button @click="reset" class="reset" :disabled="name.length == 0 ? true : false">รีเซต</button>
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
      <!--modal group mode  -->
     <div class="modal-bg-group" v-show="modalGroupShow == true" >
       <div class="chooseNumbeGroup">
         <h1>กรุณาใส่จำนวนกลุ่มและจำนวนต่อกลุ่ม</h1>
            <form>
              <label class="numberGroup" for="numberGroup">จำนวนกลุ่ม</label>
              <input v-model="numberGroup" class="inputNumberG" id="numberGroup" type="number" min="1" > <br>
              <label class="no-less" for="no-less">จำนวนสมาชิกในแต่ละกลุ่ม</label>
              <input v-model="numberGL" class="Group-no-less" id="no-less" type="number" min="1" > 
            </form>
            <button @click="backGroup"  class="modal-lucky-again">กลับหน้าหลัก</button>
            <button @click="randomGroup" class="modal-lucky-ok" >สุ่มกลุ่ม</button>
       </div>
      </div>

      <!--  -->
  </div>
      </div>

  

</template>
 
<style>
 @import url(./style.css);
</style>