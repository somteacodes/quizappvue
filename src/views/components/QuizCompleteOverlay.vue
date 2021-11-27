<template>
  <div
    class="w-screen h-screen absolute z-30 bg-black bg-opacity-60 flex justify-center items-center"
  >
    <div class="bg-green-700 p-4 text-center text-white">
      <p class="font-bold text-2xl">All Done!</p>
      <p class="my-4 font-bold text-3xl">{{percent}}% score</p>

      <!-- buttons -->
      <div class="flex justify-around">
        <!-- <div
          class="rounded-full py-1 w-28 border cursor-pointer hover:text-black hover:bg-white"
        >
          Done
        </div> -->
        <div
          class="rounded-full py-1 w-28 border cursor-pointer hover:text-black hover:bg-white"
          @click="$emit('restartQuiz')"
        >

          Play Again
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props:['percent'],
  setup() {
    
  },
}
</script>