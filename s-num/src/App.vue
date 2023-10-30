<script setup>
import { ref, onMounted } from 'vue'

const inputNum = ref([0,0,0])
const randomNum = ref([0,0,0])

const chkAnswer = (() => {
  let redCount = 0;
  let yellowCount = 0;
  let greenCount = 0;

  let set = new Set(inputNum.value)

  if(inputNum.value.length !== set.size){
    alert('같은 숫자는 사용할 수 없어요~')
    return false;
  }


  randomNum.value.forEach((v,i) => {
    if(v === Number(inputNum.value[i])){
      greenCount++;
    }else if(randomNum.value.includes(Number(inputNum.value[i]))){
      yellowCount++;
    }else{
      redCount++;
    }
  })
});


onMounted(() => {
  const allNumbers = Array.from({ length: 10 }, (_, index) => index);

  for (let i = allNumbers.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [allNumbers[i], allNumbers[j]] = [allNumbers[j], allNumbers[i]];
  }
  randomNum.value = allNumbers.slice(0, 3);
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
                      :rules="[val => val.length <= 1 ||  '1자리 이상 입력하지 마세요~' ]"
                      v-model="inputNum[0]" variant="solo-filled"></v-text-field>
        <v-text-field class="mr-3"
                      @focus="inputNum[1] === 0 ? inputNum[1] = '' : ''"
                      @blur="!inputNum[1] ? inputNum[1] = 0 : ''"
                      :rules="[val => val.length <= 1 ||  '1자리 이상 입력하지 마세요~' ]"
                      v-model="inputNum[1]" variant="solo-filled"></v-text-field>
        <v-text-field class="mr-3"
                      @focus="inputNum[2] === 0 ? inputNum[2] = '' : ''"
                      @blur="!inputNum[2] ? inputNum[2] = 0 : ''"
                      :rules="[val => val.length <= 1 ||  '1자리 이상 입력하지 마세요~' ]"
                      v-model="inputNum[2]" variant="solo-filled"></v-text-field>
        </div>
        <v-btn
            :disabled=
                "inputNum[0].length > 1 ||
                 inputNum[1].length > 1 ||
                 inputNum[2].length > 1"
            variant="text" size="x-large" class="font-weight-bold text-h4" @click="chkAnswer">도전</v-btn>
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
  align-items: center;
  flex-direction: column;
  justify-content: space-around;
}


</style>
