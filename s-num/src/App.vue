<script setup>
import { ref, onMounted } from 'vue'

const inputNum = ref([0,0,0])
const randomNum = ref([0,0,0])

onMounted(() => {
  randomNum.value[0] = Math.round(Math.random());
  randomNum.value[1] = Math.floor(Math.random() * 10);
  randomNum.value[2] = Math.floor(Math.random() * 10);
  alert('랜덤넘버는 ' + randomNum.value[0] + randomNum.value[1] + randomNum.value[2] + '입니다')
})

</script>

<template>
  <div class="backyard">
    <div class="game-board">
      <div class="flex-column">
        <span class="text-h3 font-weight-bold ">Choose the Number</span>
      </div>
      <div class="d-flex flex-column">
        <div class="d-flex">
        <v-text-field class="mr-3"
                      @focus="inputNum[0] === 0 ? inputNum[0] = '' : ''"
                      @blur="!inputNum[0] ? inputNum[0] = 0 : ''"
                      v-model="inputNum[0]" variant="solo-filled"></v-text-field>
        <v-text-field class="mr-3"
                      @focus="inputNum[1] === 0 ? inputNum[1] = '' : ''"
                      @blur="!inputNum[1] ? inputNum[1] = 0 : ''"
                      v-model="inputNum[1]" variant="solo-filled"></v-text-field>
        <v-text-field class="mr-3"
                      @focus="inputNum[2] === 0 ? inputNum[2] = '' : ''"
                      @blur="!inputNum[2] ? inputNum[2] = 0 : ''"
                      v-model="inputNum[2]" variant="solo-filled"></v-text-field>
        </div>
        <v-btn variant="text" class="font-weight-bold text-h3">도전</v-btn>
      </div>
    </div>

  </div>
</template>

<style scoped>
.backyard{
  width: 100vw;
  height: 100vh;
  background-color: #535bf2;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.game-board{
  width: 50%;
  height: 50%;
  margin: 0 auto;
  display: flex;
  border: 1px solid black;
  align-items: center;
  flex-direction: column;
  justify-content: space-around;
}


</style>
