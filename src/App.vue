<script setup>
import { ref, onMounted } from 'vue'
import soundlogo from './assets/img/sound.png'
import mutelogo from './assets/img/muteS.png'
document.title = 'lucky jing 🍀'
//opasity style in image Group P1----------------------------------------------------

const deleteT = 'opacity: 0.5;'
const deleteF = 'opacity: 1;'

// Array save every values
const name = ref([])

// function add inputname Values --------------------------------------------------------------

const inputName = ref('')

const addname = () => {
  if (name.value.length >= 0) {
    inputName.value.split('\n').forEach((e) => {
      if (e.length > 0) {
        name.value.push(e)
      }
    })
  } else {
    alert('input value again')
  }
  name.value = [...new Set(name.value)]
}

// fucntion Reset ------------------------------------------------------------------------

const reset = () => {
  name.value.length = 0
  randomName.value = ''
  inputName.value = ''
  totalGroups.value.length = 0
}

// function lucky Random -----------------------------------------------------------------

const randomName = ref('')

const random = (r) => {
  if (name.value.length > 0) {
    for (let i in name.value) {
      r = name.value[Math.floor(Math.random() * name.value.length)]
      modalLuckyShow.value = true
    }
  } else {
    alert('Please input Value in template')
  }

  return (randomName.value = r)
}


// function show List
const modalShowList = ref(false)
const list = ref('')
const showList = () => {
  if (name.value.length > 0) {
      modalShowList.value = true
  } else {
    alert('Please input Value in template')
  }
}
const listBack = () => {
  modalShowList.value = false
}

// back to menu ------------------------------------------------------------------------------

//lucky
const modalLuckyShow = ref(false)
const back = () => {
  modalLuckyShow.value = false
}

//GroupP1
const modalGroupShow = ref(false)
const backGroup = () => {
  numberGroup.value = null
  numberGL.value = null
  modalGroupShow.value = false
}

//GroupP2
const modalGroupShowfinal = ref(false)
const backGroupfinal = () => {
  numberGroup.value = null
  numberGL.value = null
  modalGroupShow.value = false
  modalGroupShowfinal.value = false
}

//function group mode --------------------------------------------------------------------

const numberGroup = ref()
const numberGL = ref()
const totalGroups = ref([])

const chooseRandomGroup = () => {
  modalGroupShow.value = true
}

const shuffle = array => {
  let currentIndex = array.length,
    randomIndex;
  while (currentIndex != 0) {
    randomIndex = Math.floor(Math.random() * currentIndex)
    currentIndex--
    [array[currentIndex], array[randomIndex]] = [
      array[randomIndex],
      array[currentIndex],
    ]
  }
  return array;
}

let groupF = () => {
  if (numberGL.value > 0) {
    modalGroupShowfinal.value = true;
    let n = Math.floor(name.value.length / numberGL.value);
    console.log(n);
    let member = shuffle(name.value);
    let count = numberGL.value;
    for (let i = 0; i < n; i++) {
      totalGroups.value.push(member.splice(0, count));
    }
    while (name.value.length > 0) {
      totalGroups.value.push(name.value.splice(0, name.value.length));
      alert(`กลุ่มที่ไม่ตรงตามเงื่อนไขคือกลุ่ม : ${totalGroups.value.length}`);
    }
  } else if (numberGroup.value > 0) {
    modalGroupShowfinal.value = true;
    let member2 = shuffle(name.value);
    let count2 = Math.ceil(member2.length / numberGroup.value);
    for (let i = 0; i < numberGroup.value; i++) {
      totalGroups.value.push(member2.splice(0, count2));
    }
  } else {
    alert("Please input number in boxs");
  }
};
//sound------------------------------------------------------------------------------------

const muted = ref(false)
onMounted(() => {
  document.querySelector('body').addEventListener('click', () => {
    document.getElementById("audio").play()
    muted.value = !muted.value
  })
})

const soundMute = () => {
  document.getElementById('audio').muted = muted.value
  document
    .getElementById('thisMusic')
    .setAttribute("src", muted.value ? mutelogo : soundlogo)
}

// reset box in group P1 -------------------------------------------------------------------

const resetGl = () => numberGL.value = null
const resetNumberG = () => numberGroup.value = null

//history-------------------------------------------------------------------------------------

const history = () => {
  modalGroupShowfinal.value = true
}
</script>

<template>
  <div>
    <img @click="soundMute" id="thisMusic" class="audio" src="./assets/img/sound.png" />
    <audio id="audio" class="audio" autoplay loop>
      <source src="./assets/sound/bg2.mp3" />
    </audio>
    <img class="mascot" src="./assets/img/mascot.png" />
    <a href="https://github.com/ChisanuchaK/luckyJing" target="_blank">
      <img class="logo-git" src="./assets/img/logo-gti.png" />
    </a>
    <img class="logo" src="./assets/img/logo.png" />

    <div class="container">
      <div class="lucky-box">
        <h1 class="title">มาสุ่มกันเถอะ</h1>
        <img
          @click="history"
          class="history"
          src="./assets/img/history.png"
          v-show="modalGroupShowfinal == false"
        />
        <textarea
          v-model="inputName"
          id="input"
          class="input"
          rows="10"
          placeholder="ใส่ค่าลงในนี้ค่าต่อไป ให้กด Enter&#10;ข้อมูลซ้ำจะถูกตัดออกโดยอัตโนมัติ&#10;ตัวอย่างการกรอกข้อมูล :&#10;มิ้น&#10;นาย&#10;คิม&#10;ปอย&#10;บิลลี่"
        ></textarea>
       <div class="devNumberTitle"><label class="numberTitle" @click="showList">จำนวนค่าทั้งหมด : {{ name.length }}</label></div>
        <div class="input-group">
          <button @click="addname" class="addValue mainb" :disabled="inputName.length == 0">เพิ่มค่า</button>
          <button
            @click="reset"
            class="reset mainb"
            :disabled="
              (name.length == 0 && totalGroups.length == 0)
            "
          >รีเซต</button>
        </div>
        <!-- modal -->
        <div class="input-group">
          <button @click="random" class="lucky-mode mainb" :disabled="name.length == 0">สุ่มผู้โชคดี</button>
          <button
            @click="chooseRandomGroup"
            class="group-mode mainb"
            :disabled="name.length == 0"
          >สุ่มกลุ่ม</button>
        </div>
      </div>
      <!-- modal mode -->
      <!-- modal-lucku-mode -->
      <div class="modal-bg" v-show="modalLuckyShow == true">
        <div class="modal-content">
          <div class="title-modal-lucky">
            <h1 class="con">ขอแสดงความยินดี</h1>
          </div>
          <div style="display: table; height: 100%">
            <h1 class="lucky-N">{{ randomName }}</h1>
          </div>
          <div class="input-group">
            <button @click="random" class="modal-btn modal-lucky-again">สุ่มอีกครั้ง</button>
            <button @click="back" class="modal-btn modal-lucky-ok">กลับสู่หน้าหลัก</button>
          </div>
        </div>
      </div>
      <!--modal group mode  choose -->
      <div class="modal-bg-group" v-show="modalGroupShow == true">
        <div class="chooseNumbeGroup">
          <div class="title-group">
            <h1 class="title-g2">กรุณาใส่จำนวนกลุ่มหรือสมาชิกต่อกลุ่ม</h1>
            <label class="numberTitleGroup">จำนวนสมาชิกทั้งหมด : {{ name.length }}</label>
            <div class="content-all">
              <form>
                <div class="box-numberG">
                  <label class="contentG" for="numberGroup">จำนวนกลุ่ม</label>
                  <div class="sum-input-bin1">
                    <input
                      :disabled="numberGL > 0"
                      v-model="numberGroup"
                      class="numberInputC"
                      id="numberGroup"
                      type="number"
                      min="1"
                      placeholder="1"
                    />
                    <img
                      :style="numberGL > 0 ? deleteT : deleteF"
                      @click="resetNumberG"
                      class="delete"
                      src="./assets/img/deletenumberG.png"
                      alt="number of group"
                    />
                  </div>
                </div>

                <div class="box-numberGl">
                  <label class="contentG" for="no-less">จำนวนสมาชิกในแต่ละกลุ่ม</label>
                  <div class="sum-input-bin2">
                    <input
                      :disabled="numberGroup > 0"
                      v-model="numberGL"
                      class="numberInputC"
                      id="no-less"
                      type="number"
                      min="1"
                      placeholder="1"
                    />
                    <img
                      :style="numberGroup > 0 ? deleteT : deleteF"
                      @click="resetGl"
                      class="delete"
                      src="./assets/img/deletenumberG.png"
                      alt="number of person"
                    />
                  </div>
                </div>
              </form>
            </div>
          </div>

          <div class="button-group">
            <button @click="backGroup" class="modal-btnG modal-lucky-ok">กลับสู่หน้าหลัก</button>
            <button
              @click="groupF"
              class="modal-btnG modal-lucky-again"
              :disabled="totalGroups.length > 0"
            >สุ่มกลุ่ม</button>
          </div>
        </div>
      </div>

      <!--  -->

      <!-- Group mode summit -->
      <div class="modal-bg-group" v-show="modalGroupShowfinal == true">
        <div class="modal-content-group-submit">
          <div class="title-modal-submit">
            <h1 class="h1G">ประกาศการจับกลุ่ม</h1>
          </div>
          <div class="grid-container">
            <div class="grid-item" v-for="(totalGroup, index) in totalGroups" :key="index">
              <div class="list-Group-final">
                <div class="head-modalG">
                  <h4 class="indexGroup">กลุ่มที่{{ index + 1 }}</h4>
                </div>
                <p class="listG" v-html="totalGroup.join('<br>')"></p>
              </div>
            </div>
          </div>
          <div class="back-final-maodal">
            <button @click="backGroupfinal" class="modal-group-final">กลับสู่หน้าหลัก</button>
          </div>
        </div>
      </div>

      <!--  -->

      <!-- ShowList -->
      <div class="modal-bg" v-show="modalShowList == true">
        <div class="modal-content-group-submit">
          <div class="title-modal-submit">
            <h1 class="h1G">จำนวนค่าทั้งหมด</h1>
          </div>
          <div class="grid-container-list mainList">
            <p class="listG" v-html="name.join('<br>')"></p>
          </div>
          <div class="input-group">
            <button @click="listBack" class="modal-btn modal-lucky-ok">กลับสู่หน้าหลัก</button>
          </div>
        </div>
      </div>
      <!--  -->
    </div>
  </div>
</template>

<style>
@import url(./groupMode.css);
@import url(./bg.css);
@import url(./menuMain.css);
@import url(./luckyMode.css);
</style>
