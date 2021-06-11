<template>
  <div class="w-full mx-auto">
    <div class="w-full text-center mt-3 text-gray-600 md:text-2xl text-lg mb-3">
      {{ message }}
    </div>
    <div class="time-loader w-full mx-auto"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      message: "変換中...",
    };
  },
  mounted() {
    setTimeout(() => {
      this.message = "ただいま愛情込めて描いています";
    }, 2000);
    setTimeout(() => {
      this.message = "おまたせしてすみません";
    }, 4000);
    setTimeout(() => {
      this.message = "焦らずにお待ち下さい";
    }, 6000);
    setTimeout(() => {
      this.message = "遅すぎますか？";
    }, 8000);
    setTimeout(() => {
      this.message = "決してサボってるわけではないんです";
    }, 10000);
    setTimeout(() => {
      this.message = "只今大慌てで描いています";
    }, 12000);
    setTimeout(() => {
      this.message = "もう少し..もう少しだけお待ち下さい";
    }, 14000);
    setTimeout(() => {
      this.message = "この画像重くないですか？";
    }, 16000);
    setTimeout(() => {
      this.message = "でも頑張って描いていますよ";
    }, 18000);
    setTimeout(() => {
      this.message = "あとちょっとだと思うんです";
    }, 20000);
    setTimeout(() => {
      this.message = "あああ手元が狂いました";
    }, 22000);
    setTimeout(() => {
      this.message = "すぐに修正するのでお待ち下さい";
    }, 24000);
    setTimeout(() => {
      this.message = "遅いって？";
    }, 26000);
    setTimeout(() => {
      this.message = "お金がなくてサーバー代をケチった結果です";
    }, 28000);
  },
};
</script>
<style>
.time-loader {
  width: 120px;
  height: 20px;
  -webkit-mask: linear-gradient(90deg, #d97707 70%, #0000 0) left/20% 100%;
  background: linear-gradient(#d97707 0 0) left -25% top 0 /20% 100% no-repeat
    #ddd;
  animation: ct7 1s infinite steps(6);
}
@keyframes ct7 {
  100% {
    background-position: right -25% top 0;
  }
}
</style>
