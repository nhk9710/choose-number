<script setup>
defineProps({
  resultArray: Array
})


</script>

<template>
  <div class="scoreBoard">
    <div v-for="v in resultArray">
      <span class="text-h3 text-red mr-1">{{ v.red }}</span>
      <span class="text-h3 text-yellow mr-1">{{ v.yellow }}</span>
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