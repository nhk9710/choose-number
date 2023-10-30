<script setup>
defineProps({
  resultArray: Array
})


</script>

<template>
  <div class="scoreBoard">
    <div v-for="v in resultArray">
      <span class="text-h3 text-red">{{ v.red }}</span>
      <span class="text-h3 text-yellow">{{ v.yellow }}</span>
      <span class="text-h3 text-green">{{ v.green }}</span>
    </div>
  </div>
</template>

<style scoped>
.scoreBoard{
  width: 10vw;
  height: 30vh;
}


</style>