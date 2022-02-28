<script setup>
import { ref, reactive, onMounted, computed } from 'vue'
import unmutedImg from './assets/img/sound.png'
import mutedImg from './assets/img/muteS.png'
document.title = 'lucky jing 🍀'
//opasity style in image Group P1----------------------------------------------------

const deleteT = 'opacity: 0.5;'
const deleteF = 'opacity: 1;'

// Array save every values
const name = ref([])
const history = ref([])

const RandomType = Object.freeze({ LUCKY: 0, GROUPING: 1 })
const RandomTypeContext = Object.freeze({
  [RandomType.LUCKY]: 'สุ่มรายชื่อ',
  [RandomType.GROUPING]: 'สุ่มกลุ่ม',
})

// Create dynamic modal
const ModalEnum = Object.freeze({
  RANDOM_RESULT: 0,
  INPUT_LIST: 1,
  GROUPING_OPTION: 2,
  GROUPTING_RESULT: 3,
  HISTORY: 4,
})

let modals = {}
Object.values(ModalEnum).forEach((val) => {
  modals[val] = {
    state: false,
    transitionClass: computed(() => modals[val].state && 'scale-in-center'),
    setState: (state) => (modals[val] = state),
    show: () => (modals[val].state = true),
    close: () => (modals[val].state = false),
  }
})
modals = reactive(modals)

// function add inputname Values --------------------------------------------------------------

const inputName = ref('')

const addName = () => {
  if (inputName.value.length <= 0) return
  const inputArr = inputName.value
    .split(/\n/)
    .map((e) => e.trim())
    .filter((e) => e.length > 0)

  name.value.push(...inputArr)
  const dupicate = findDuplicates(name.value)

  if (dupicate.length > 0) {
    alert(`รายชื่อที่ซ้ำคือ : ${dupicate}`)
  }

  name.value = [...new Set(name.value)]
}

// function lucky Random -----------------------------------------------------------------

const randomResult = ref('')

const random = () => {
  if (name.value <= 0) {
    alert('Please input Value in template')
    return
  }

  randomResult.value = name.value[Math.floor(Math.random() * name.value.length)]
  modals[ModalEnum.RANDOM_RESULT].show()

  history.value.unshift({
    type: RandomType.LUCKY,
    data: randomResult.value,
    timestamp: new Date().toLocaleString('th'),
  })
}

//function group mode --------------------------------------------------------------------

const numberGroup = ref()
const numberGL = ref()
const totalGroups = ref([])

const doGrouping = () => {
  if (numberGL.value <= 0 && numberGroup.value <= 0) {
    alert('Please input number in boxs')
    return
  }

  totalGroups.value.length = 0

  const inputArr = shuffle(name.value)
  const memberPerGroup =
    numberGroup.value > 0
      ? Math.floor(inputArr.length / numberGroup.value)
      : numberGL.value

  do {
    if (
      numberGroup.value > 0 &&
      totalGroups.value.length >= numberGroup.value
    ) {
      totalGroups.value
        .find((group) => group.length === memberPerGroup)
        .push(inputArr.pop())
    } else if (numberGL.value > 0 && inputArr.length <= 1) {
      totalGroups.value[0].push(inputArr.pop())
    } else {
      totalGroups.value.push(inputArr.splice(0, memberPerGroup))
    }
  } while (inputArr.length > 0)
  modals[ModalEnum.GROUPTING_RESULT].show()

  history.value.unshift({
    type: RandomType.GROUPING,
    data: totalGroups.value,
    timestamp: new Date().toLocaleString('th'),
  })
}

const viewHistory = (historyObj) => {
  if (historyObj.type === RandomType.LUCKY) {
    randomResult.value = historyObj.data
    modals[ModalEnum.RANDOM_RESULT].show()
  } else if (historyObj.type === RandomType.GROUPING) {
    totalGroups.value = historyObj.data
    modals[ModalEnum.GROUPTING_RESULT].show()
  }
}

const singleRemove = (index) => {
  name.value.splice(index, 1)
  inputName.value = name.value.reduce((prev, curr) => {
    return `${prev}\n${curr}`
  })
}

/***
 * Ref: https://stackoverflow.com/questions/2450954/how-to-randomize-shuffle-a-javascript-array
 */
const shuffle = (array) => {
  const newArr = [...array]
  let currentIndex = newArr.length,
    randomIndex
  while (currentIndex != 0) {
    randomIndex = Math.floor(Math.random() * currentIndex)
    currentIndex--
    ;[newArr[currentIndex], newArr[randomIndex]] = [
      newArr[randomIndex],
      newArr[currentIndex],
    ]
  }
  return newArr
}

/***
 * Ref: https://flexiple.com/find-duplicates-javascript-array/
 */
const findDuplicates = (arr) =>
  arr.filter((item, index) => arr.indexOf(item) !== index)

//sound------------------------------------------------------------------------------------

const audioRef = ref()
const soundImgRef = ref()

onMounted(() => {
  let firstInteract = true
  document.querySelector('body').addEventListener('click', () => {
    if (firstInteract) {
      audioRef.value.play()
      audioRef.value.muted = false
      firstInteract = false
    }
  })
})

const toggleMute = () => {
  audioRef.value.muted = !audioRef.value.muted
  soundImgRef.value.src = !audioRef.value.muted ? unmutedImg : mutedImg
}

// reset box in group P1 -------------------------------------------------------------------
const resetGl = () => (numberGL.value = null)
const resetNumberG = () => (numberGroup.value = null)

const reset = () => {
  name.value.length = 0
  randomResult.value = ''
  inputName.value = ''
  totalGroups.value.length = 0
}

const closeGroupOption = () => {
  numberGroup.value = null
  numberGL.value = null
  modals[ModalEnum.GROUPING_OPTION].close()
}
</script>

<template>
  <div class="app">
    <img
      @click="toggleMute"
      ref="soundImgRef"
      class="audio"
      :src="unmutedImg"
    />
    <audio ref="audioRef" class="audio" volume="0.2" autoplay loop>
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
          @click="modals[ModalEnum.HISTORY].show()"
          class="history"
          src="./assets/img/history.png"
          v-show="history.length > 0"
        />
        <textarea
          v-model="inputName"
          id="input"
          class="input"
          rows="10"
          placeholder="กรุณาใส่ชื่อ แล้วกด Enter&#10;ข้อมูลซ้ำจะถูกตัดออกโดยอัตโนมัติ&#10;ตัวอย่างการกรอกข้อมูล :&#10;มิ้น&#10;นาย&#10;คิม&#10;ปอย&#10;บิลลี่"
        ></textarea>
        <div class="devNumberTitle">
          <label
            class="numberTitle"
            :style="{ pointerEvents: name.length > 0 ? 'auto' : 'none' }"
            @click="modals[ModalEnum.INPUT_LIST].show()"
            >จำนวนทั้งหมด : {{ name.length }}
          </label>
        </div>
        <div class="input-group">
          <button
            @click="addName"
            class="addValue mainb"
            :disabled="inputName.length <= 0"
          >
            เพิ่มค่า
          </button>
          <button
            @click="reset"
            class="reset mainb"
            :disabled="name.length <= 0 && totalGroups.length <= 0"
          >
            รีเซต
          </button>
        </div>
        <!-- modal -->
        <div class="input-group">
          <button
            @click="random"
            class="lucky-mode mainb"
            :disabled="name.length <= 0"
          >
            สุ่มผู้โชคดี
          </button>
          <button
            @click="modals[ModalEnum.GROUPING_OPTION].show()"
            class="group-mode mainb"
            :disabled="name.length <= 0"
          >
            สุ่มกลุ่ม
          </button>
        </div>
      </div>
      <!-- modal mode -->
      <div class="modal-bg" v-show="modals[ModalEnum.HISTORY].state">
        <div
          class="modal-content"
          :class="modals[ModalEnum.HISTORY].transitionClass"
        >
          <h1 class="title-modal-lucky">ประวัติการสุ่ม</h1>
          <div class="grid-container-list history-box">
            <div
              class="history-item"
              v-for="(historyObj, index) in history"
              :key="index"
              @click="viewHistory(historyObj)"
            >
              <div
                :class="`history-type ${
                  historyObj.type === RandomType.LUCKY ? 'person' : 'group'
                }`"
              >
                {{ RandomTypeContext[historyObj.type] }}
              </div>
              <div>{{ historyObj.timestamp }}</div>
            </div>
          </div>
          <button
            @click="modals[ModalEnum.HISTORY].close()"
            class="modal-btn modal-lucky-ok"
          >
            กลับสู่หน้าหลัก
          </button>
        </div>
      </div>

      <!-- modal-lucku-mode -->
      <div class="modal-bg" v-show="modals[ModalEnum.RANDOM_RESULT].state">
        <div
          class="modal-content"
          :class="modals[ModalEnum.RANDOM_RESULT].transitionClass"
        >
          <div class="title-modal-lucky">
            <h1 class="con">ขอแสดงความยินดี</h1>
          </div>
          <div style="display: table; height: 100%">
            <h1 class="lucky-N">{{ randomResult }}</h1>
          </div>
          <div class="input-group">
            <button
              @click="random"
              class="modal-btn modal-lucky-again"
              v-show="!modals[ModalEnum.HISTORY].state"
            >
              สุ่มอีกครั้ง
            </button>
            <button
              @click="modals[ModalEnum.RANDOM_RESULT].close()"
              class="modal-btn modal-lucky-ok"
            >
              กลับสู่หน้าหลัก
            </button>
          </div>
        </div>
      </div>
      <!--modal group mode  choose -->
      <div class="modal-bg" v-show="modals[ModalEnum.GROUPING_OPTION].state">
        <div
          class="modal-content"
          :class="modals[ModalEnum.GROUPING_OPTION].transitionClass"
        >
          <div class="title-group">
            <h1 class="title-g2">กรุณาใส่จำนวนกลุ่มหรือสมาชิกต่อกลุ่ม</h1>
            <label class="numberTitleGroup"
              >จำนวนสมาชิกทั้งหมด : {{ name.length }}</label
            >
            <div class="content-all">
              <form>
                <div class="box-number">
                  <label class="contentG" for="numberGroup">จำนวนกลุ่ม</label>
                  <div class="sum-input-bin">
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

                <div class="box-number">
                  <label class="contentG" for="no-less"
                    >จำนวนสมาชิกในแต่ละกลุ่ม</label
                  >
                  <div class="sum-input-bin">
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
            <button
              @click="closeGroupOption()"
              class="modal-btnG modal-lucky-ok"
            >
              กลับสู่หน้าหลัก
            </button>
            <button
              @click="doGrouping"
              class="modal-btnG modal-lucky-again"
              :disabled="name.length <= 0"
            >
              สุ่มกลุ่ม
            </button>
          </div>
        </div>
      </div>

      <!--  -->

      <!-- Group mode summit -->
      <div class="modal-bg" v-show="modals[ModalEnum.GROUPTING_RESULT].state">
        <div
          class="modal-content-group-submit"
          :class="modals[ModalEnum.GROUPTING_RESULT].transitionClass"
        >
          <div class="title-modal-submit">
            <h1 class="h1G">ประกาศการจับกลุ่ม</h1>
          </div>
          <div class="grid-container">
            <div
              class="grid-item"
              v-for="(totalGroup, index) in totalGroups"
              :key="index"
            >
              <div class="list-Group-final">
                <div class="head-modalG">
                  <h4 class="indexGroup">
                    กลุ่มที่{{ index + 1 }} : {{ totalGroup.length }} คน
                  </h4>
                </div>
                <p class="listG" v-html="totalGroup.join('<br>')"></p>
              </div>
            </div>
          </div>
          <div class="back-final-maodal">
            <button
              @click="modals[ModalEnum.GROUPTING_RESULT].close()"
              class="modal-group-final"
            >
              กลับสู่หน้าหลัก
            </button>
          </div>
        </div>
      </div>
      <!--  -->

      <!-- ShowList -->
      <div class="modal-bg" v-show="modals[ModalEnum.INPUT_LIST].state">
        <div
          class="modal-content-group-submit"
          :class="modals[ModalEnum.INPUT_LIST].transitionClass"
        >
          <div class="title-modal-submit">
            <h1 class="h1G">รายชื่อทั้งหมด</h1>
          </div>
          <div class="grid-container-list mainList">
            <p
              class="singleRemove"
              v-for="(xx, index) in name"
              :key="index"
              @click="singleRemove(index)"
            >
              {{ xx }}
            </p>
          </div>
          <div class="input-group">
            <button
              @click="modals[ModalEnum.INPUT_LIST].close()"
              class="modal-btn modal-lucky-ok"
            >
              กลับสู่หน้าหลัก
            </button>
          </div>
        </div>
      </div>
      <!--  -->
    </div>
  </div>
</template>

<style>
@import url(./style.css);
</style>
