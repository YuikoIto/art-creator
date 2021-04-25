<template>
  <div class="">
    <button class="bg-black" @click="isStart = !isStart">スタート</button>
    <div v-if="isStart">
      <BlockGame />
    </div>
  </div>
</template>
<script>
import BlockGame from "~/components/BlockGame.vue";
export default {
  components: {
    BlockGame,
  },
  data() {
    return {
      isStart: false,
    };
  },
};
</script>
