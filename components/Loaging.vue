<template>
  <div class="w-full mx-auto">
    <div
      class="loading-message w-full text-center mt-3 text-gray-600 text-2xl mb-3"
    ></div>
    <div class="time-loader w-full mx-auto"></div>
  </div>
</template>
<style>
.loading-message::before {
  content: "変換中...";
  animation: message 5s infinite linear;
}

@keyframes message {
  50% {
    content: "変換には10秒ほどかかります";
  }
  100% {
    content: "焦らずにお待ち下さい";
  }
}

.time-loader {
  --c1: #38382e;
  --c2: #d97707;
  width: 40px;
  height: 80px;
  border-top: 4px solid var(--c1);
  border-bottom: 4px solid var(--c1);
  background: linear-gradient(
      90deg,
      var(--c1) 2px,
      var(--c2) 0 5px,
      var(--c1) 0
    )
    center/7px 8px no-repeat;
  display: grid;
  overflow: hidden;
  animation: t5-0 3s infinite linear;
}
.time-loader::before,
.time-loader::after {
  content: "";
  grid-area: 1/1;
  width: 75%;
  height: calc(50% - 4px);
  margin: 0 auto;
  border: 2px solid var(--c1);
  border-top: 0;
  border-radius: 0 0 40% 40%;
  -webkit-mask: linear-gradient(#000 0 0) bottom/4px 2px no-repeat,
    linear-gradient(#000 0 0);
  -webkit-mask-composite: destination-out;
  mask-composite: exclude;
  background: linear-gradient(var(--d, 0deg), var(--c2) 50%, #0000 0) bottom /
      100% 205%,
    linear-gradient(var(--c2) 0 0) center/0 100%;
  background-repeat: no-repeat;
  animation: inherit;
  animation-name: t5-1;
}

.time-loader::after {
  transform-origin: 50% calc(100% + 2px);
  transform: scaleY(-1);
  --s: 3px;
  --d: 180deg;
}

@keyframes t5-0 {
  80% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(0.5turn);
  }
}

@keyframes t5-1 {
  10%,
  70% {
    background-size: 100% 205%, var(--s, 0) 100%;
  }
  70%,
  100% {
    background-position: top, center;
  }
}
</style>
