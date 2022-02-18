<script setup>
import {ref} from 'vue'
  document.title = 'lucky jing 🍀'
  //music
  const bgMusic = new Audio('.luckyJing\src\assets\sound\bg.mp3')
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
</script>
 
<template>
  <div>
    <img class="mascot" src="./assets/mascot.png" >
    <a href="https://github.com/ChisanuchaK/luckyJing" target="_blank">
      <img class="logo-git" src="./assets/logo-gti.png">
    </a>
    <img class="logo" src="./assets/logo.png">

          <div class="container">
       <img class="wood" src="./assets/wood.png">
      <h1 class="title">
          มาสุ่มกันเถอะ
      </h1>
      <textarea v-model="inputName" id="input" class="input" cols="30" rows="10" placeholder="ใส่ค่าลงในนี้">

      </textarea>
      <div class="addReset-button">
      <button @click="addname" class="addValue" :disabled="inputName.length == 0 ? true : false">เพิ่มค่า</button>
       <button @click="reset" class="reset" :disabled="name.length == 0 ? true : false">รีเซต</button>
      </div>
        <!-- modal -->
    <div class="button">
    <button @click="random" class="lucky-mode" :disabled="name.length == 0 ? true : false" >สุ่มผู้โชคดี</button>
     <button class="group-mode" :disabled="name.length == 0 ? true : false">สุ่มกลุ่ม</button>
    </div>
      <div class="modal-bg" v-show="modalLuckyShow == true">
        <div class="modal-content"> 
          <div class="title-modal-lucky">
            <h1>ขอแสดงความยินดี</h1>
          </div>
            <h1 class="lucky-N">{{randomName}}</h1>
            <button @click="back" class="modal-lucky-ok">กลับสู่หน้าหลัก</button>
        </div>
      </div>
  </div>
      </div>

  

</template>
 
<style>
 @import url(./style.css);
</style>