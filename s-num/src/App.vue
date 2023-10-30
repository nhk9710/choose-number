<script setup>
import { ref, onMounted } from 'vue'
import ScoreBoard from "./components/ScoreBoard.vue";

const inputNum = ref([0,0,0])
const randomNum = ref([0,0,0])
const resultArr = ref([])

const dialog = ref(false)

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
  if(resultArr.value.length === 5){
    alert('화이팅~')
    location.reload()
  }else{
    resultArr.value.push({ red: redCount, green: greenCount, yellow: yellowCount })
  }
});


onMounted(() => {
  const allNumbers = Array.from({ length: 10 }, (_, index) => index);

  for (let i = allNumbers.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [allNumbers[i], allNumbers[j]] = [allNumbers[j], allNumbers[i]];
  }
  randomNum.value = allNumbers.slice(0, 3);
})

</script>

<template>
  <div class="backyard">
    <div style="display: flex; justify-content: center; margin-left: 10%">
    <div class="game-board">
      <div class="flex-column mb-10">
        <span class="font-weight-bold titleTxt">Choose the Number
        <v-dialog
          v-model="dialog"
          activator="parent"
          width="auto"
        >
          <v-card>
            <v-card-text>
              규칙
              1. 기회는 5번
              2. 빨간색 : 모두 틀림, 노란색: 숫자는 맞음, 초록색: 위치와 숫자 모두 맞음
            </v-card-text>
          </v-card>
        </v-dialog>
        </span>
      </div>
      <div class="d-flex flex-column">
        <div class="d-flex mb-10">
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
            variant="text" size="x-large" class="font-weight-bold text-h4" @click="chkAnswer">
          {{ resultArr.length === 5 ? '재도전' : '도전!' }}
        </v-btn>
      </div>
    </div>
    <ScoreBoard :result-array="resultArr"></ScoreBoard>
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

  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: space-around;
}

@media (max-width: 1980px) {
  .titleTxt{
    font-size: 3em;
  }
}
@media (max-width: 1480px) {
  .titleTxt{
    font-size: 2.5em;
  }
}
@media (max-width: 1080px) {
  .titleTxt{
    font-size: 2em;
  }
}
@media (max-width: 500px) {
  .titleTxt{
    font-size: 1em;
  }
}


</style>
