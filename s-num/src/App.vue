<script setup>
import { ref, onMounted } from 'vue'
import ScoreBoard from "./components/ScoreBoard.vue";

//입력 숫자
const inputNum = ref([0,0,0])
//랜덤 숫자
const randomNum = ref([0,0,0])
//결과
const resultArr = ref([])
//기록
const historyArr = ref([])

//dialog
const dialog = ref(false)
const history = ref(false)

//우승 확인
const winnerChk = ref(false)

//커서용 ref
const first = ref(null)
const second = ref(null)
const third = ref(null)


//자동 포커스 이동
const targetFirst = () => {
  first.value.focus()
}
const targetSecond = () => {
    second.value.focus();
}
const targetThird = () => {
  third.value.focus();
}

//정답 확인 메소드
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

  if(greenCount === 3){
    winnerChk.value = true;
    alert('우승!');
    location.reload();
  }

  if(resultArr.value.length === 6){
    alert('정답은' + randomNum.value + '였습니다~ 화이팅~')
    location.reload()
  }else{
    historyArr.value.push({ one: inputNum.value[0], two: inputNum.value[1], three: inputNum.value[2] })
    resultArr.value.push({ red: redCount, green: greenCount, yellow: yellowCount })
    inputNum.value = [0,0,0]
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
            <v-card-title>규칙</v-card-title>
            <v-card-text>
              1. 기회는 6번
            </v-card-text>
            <v-card-text>
              2. 빨간색 숫자: 랜덤수와 일치하는 숫자 없음
            </v-card-text>
            <v-card-text>
              3. 노란색 숫자: 랜덤수와 일치하는 숫자는 있으나 위치가 틀림
            </v-card-text>
            <v-card-text>
              4. 초록색 숫자: 위치와 숫자 모두 맞음
            </v-card-text>
          </v-card>
        </v-dialog>
        </span>
      </div>
      <div class="d-flex flex-column">
        <div class="d-flex mb-10">
        <v-text-field class="mr-3"
                      ref="first"
                      @input="targetSecond"
                      @focus="inputNum[0] === 0 ? inputNum[0] = '' : ''"
                      @blur="!inputNum[0] ? inputNum[0] = 0 : ''"
                      v-model="inputNum[0]" variant="solo-filled"></v-text-field>
        <v-text-field class="mr-3"
                      ref="second"
                      @input="targetThird"
                      @focus="inputNum[1] === 0 ? inputNum[1] = '' : ''"
                      @blur="!inputNum[1] ? inputNum[1] = 0 : ''"
                      v-model="inputNum[1]" variant="solo-filled"></v-text-field>
        <v-text-field class="mr-3"
                      ref="third"
                      @input="targetFirst"
                      @focus="inputNum[2] === 0 ? inputNum[2] = '' : ''"
                      @blur="!inputNum[2] ? inputNum[2] = 0 : ''"
                      v-model="inputNum[2]" variant="solo-filled"></v-text-field>
        </div>
        <v-btn
            :disabled=
                "inputNum[0].length > 1 ||
                 inputNum[1].length > 1 ||
                 inputNum[2].length > 1"
            variant="text" size="x-large" class="font-weight-bold text-h4" @click="chkAnswer">
          {{ resultArr.length === 6 || winnerChk ? '재도전' : '도전!' }}
        </v-btn>
        <v-btn class="mb-1">기록
          <v-dialog
              v-model="history"
              activator="parent"
              width="auto"
          >
            <v-card>
              <v-card-title>이전에 입력한 숫자</v-card-title>
              <v-card-text v-for="v in historyArr">
                <span>{{ v.one }}</span>
                <span>{{ v.two }}</span>
                <span>{{ v.three }}</span>
              </v-card-text>
            </v-card>
          </v-dialog>
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
