<template>
  <div
    class="leading-normal tracking-normal text-white gradient"
    style="font-family: 'Source Sans Pro', sans-serif"
  >
    <Header />
    <Section id="make" title="画像を選択する">
      <div class="flex flex-wrap">
        <div class="w-full sm:w-1/2 p-6 mx-auto">
          <h3
            class="md:text-3xl text-lg text-gray-800 font-bold leading-none mb-3"
          >
            ライブラリから画像を選択
          </h3>
          <p class="text-gray-600 mb-3 text-base">
            できる限り輪郭がはっきりしている画像を選んでください。
            <br />選択できる画像は2Mバイトまでです。
          </p>
          <label
            class="bg-gray-400 cursor-pointer inline-flex items-center hover:bg-gray-300 text-white font-bold py-2 px-4 rounded-full"
          >
            <svg
              class="w-8 h-8"
              fill="currentColor"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 20 20"
            >
              <path
                d="M16.88 9.1A4 4 0 0 1 16 17H5a5 5 0 0 1-1-9.9V7a3 3 0 0 1 4.52-2.59A4.98 4.98 0 0 1 17 8c0 .38-.04.74-.12 1.1zM11 11h3l-4-4-4 4h3v3h2v-3z"
              />
            </svg>
            <span class="ml-2 text-base leading-normal">画像を選択</span>
            <input
              id="file"
              ref="fileInput"
              type="file"
              accept="image/*"
              class="hidden"
              @change="setImage"
            />
          </label>
          <p v-if="tooBig">
            画像サイズが大きすぎます<br />2Mバイトまでの画像を選んでください。
          </p>
        </div>
        <div class="w-full mt-3 sm:w-1/2 relative">
          <img
            v-if="!uploadImageUrl && !changedImageUrl"
            src="../assets/img/defaultPic.png"
            alt="default"
            class="border-dashed border-4 border-gray-600"
          />
          <img
            v-if="uploadImageUrl"
            class="border-dashed border-4 border-gray-600"
            width="100%"
            :class="{ 'opacity-50': overlay }"
            :src="uploadImageUrl"
          />
          <img
            v-if="changedImageUrl"
            class="border-dashed border-4 border-gray-600"
            width="100%"
            :src="changedImageUrl"
          />
          <div v-if="overlay" class="absolute overlay">
            <svg
              class="animate-spin"
              xmlns="http://www.w3.org/2000/svg"
              version="1.1"
              height="40"
              width="40"
              viewBox="0 0 75 75"
            >
              <circle
                cx="37.5"
                cy="37.5"
                r="33.5"
                stroke-width="8"
                fill="none"
                stroke="#F5AD54"
                stroke-linecap="square"
                stroke-dasharray="151.55042961px,210.48670779px"
                stroke-dashoffset="0"
              />
            </svg>
          </div>
        </div>
      </div>
    </Section>
    <Section title="画像のスタイルを選ぶ">
      <p
        class="w-full text-center mt-3 text-gray-600 text-2xl mb-3"
        v-if="!uploadImageUrl && !changedImageUrl"
      >
        先に画像を選択してください
      </p>
      <div
        v-for="art in arts"
        :key="art.id"
        class="w-full md:w-1/3 p-3 flex flex-col cursor-pointer"
      >
        <div
          class="overflow-hidden rounded-lg shadow-lg transform transition hover:scale-105 duration-300 ease-in-out"
          @click="getArt(art[1])"
        >
          <img
            :src="art[0]"
            class="imageClass block w-full h-auto object-cover"
            alt="art"
          />
        </div>
      </div>
      <div class="mt-3" v-if="uploadImageUrl">
        <label
          class="cursor-pointer inline-flex items-center bg-white boder-solid border-2
                 border-orange-600 hover:border-orange-500 text-white font-bold py-2 px-4 rounded-full"
        >
          <svg
            class="w-8 h-8"
            fill="#000000"
            viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              d="M4 3a2 2 0 00-2 2v10a2 2 0 002 2h12a2 2 0 002-2V5a2 2 0 00-2-2H4zm12 12H4l4-8 3 6 2-4 3 6z"
              clip-rule="evenodd"
            ></path>
          </svg>
          <div
            class="text-gray-600 ml-2 text-base leading-normal"
            @click="getArt"
          >
            絵画にする
          </div>
        </label>
      </div>
    </Section>
    <Section title="画像をシェア/保存">
      <p v-if="!changedImageUrl" class="w-full text-center mt-3 text-gray-600 text-2xl mb-3">
        先に絵画を作成してください
      </p>
      <ul v-if="changedImageUrl" class="flex space-x-6 mt-3">
        <li>
          <div @click="OpenTwitterModal" class="cursor-pointer">
            <img
              alt="twitter"
              src="../assets/img/twitter.svg"
              :width="size"
              :height="size"
            />
          </div>
        </li>
        <li>
          <div @click="OpenFBModal" class="cursor-pointer">
            <img
              alt="twitter"
              src="../assets/img/facebook.svg"
              :width="size"
              :height="size"
            />
          </div>
        </li>
        <li>
          <a :href="changedImageUrl" download>
            <img
              alt="line"
              src="../assets/img/download.svg"
              :width="size"
              :height="size"
            />
          </a>
        </li>
      </ul>
    </Section>
    <Modal v-if="twitterModalFlag">
      <div>
        <img
          v-if="changedImageUrl"
          class="border-dashed border-4 mt-3 border-gray-600 mx-auto modalImageClass block w-full h-auto object-cover"
          :src="changedImageUrl"
        />
        <div
          @click="CloseModal"
          class="absolute cursor-pointer top-0 right-0"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            class="w-8 h-8"
          >
            <path
              fill-rule="evenodd"
              d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z"
              clip-rule="evenodd"
            />
          </svg>
        </div>
        <button
          @click="tweetButton"
          class="text-center mx-auto bg-gray-400 text-white font-bold rounded-b py-4 w-full shadow-lg"
        >
          ツイートする
        </button>
      </div>
    </Modal>
    <Modal v-if="FBModalFlag">
      <div>
        <img
          v-if="changedImageUrl"
          class="border-dashed border-4 mt-3 border-gray-600 mx-auto modalImageClass block w-full h-auto object-cover"
          :src="changedImageUrl"
        />
        <div
          @click="CloseModal"
          class="absolute cursor-pointer top-0 right-0"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            class="w-8 h-8"
          >
            <path
              fill-rule="evenodd"
              d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z"
              clip-rule="evenodd"
            />
          </svg>
        </div>
        <button
          @click="tweetButton"
          class="text-center mx-auto bg-gray-400 text-white font-bold rounded-b py-4 w-full shadow-lg"
        >
          シェアする
        </button>
      </div>
    </Modal>
    <Footer />
  </div>
</template>

<script>
import Vue from "vue";
import getArtImage from "~/assets/lib/getArt";
import postImageData from "~/assets/lib/postImageData";
import Modal from "~/components/Modal.vue";
import Header from "~/components/Header.vue";
import Footer from "~/components/Footer.vue";
import Section from "~/components/Section.vue";
import Wave from "~/assets/img/wave.jpeg";
import Muse from "~/assets/img/la_muse.jpeg";
import Rain from "~/assets/img/rain_princess.jpeg";
import Scream from "~/assets/img/the_scream.jpeg";
import Wreck from "~/assets/img/the_shipwreck_of_the_minotaur.jpeg";
import Udnie from "~/assets/img/udnie.jpeg";
export default Vue.extend({
  components: {
    Modal,
    Header,
    Footer,
    Section,
  },
  data() {
    return {
      changedImageUrl: "",
      uploadImageUrl: "",
      overlay: false,
      file: "",
      uuid: "",
      twitterModalFlag: false,
      FBModalFlag: false,
      imageData: "",
      isFetched: false,
      tooBig: false,
      arts: [
        [Wave, 0],
        [Muse, 2],
        [Rain, 3],
        [Scream, 1],
        [Wreck, 5],
        [Udnie, 4],
      ],
    };
  },
  computed: {
    size() {
      return 36;
    },
    url() {
      return `https://nurie-maker.com/nurie/${this.uuid}`;
    },
    twitterURL() {
      return (
        `https://twitter.com/intent/tweet?url=${this.url}&text=` +
        encodeURIComponent(
          `絵画ツクールでオリジナル絵画を作ったよ\r\n #塗り絵ツクール`
        )
      );
    },
    facebookURL() {
      return `https://www.facebook.com/sharer/sharer.php?u=${this.url}&t=塗り絵ツクールで塗り絵を作ったよ\n#塗り絵ツクール`;
    },
  },
  methods: {
    tweetButton() {
      if (this.isFetched) {
        window.open(this.twitterURL, "_blank");
      }
    },
    FBButton() {
      if (this.isFetched) {
        window.open(this.facebookURL, "_blank");
      }
    },
    async OpenTwitterModal() {
      this.twitterModalFlag = true;
      this.uuid = this.generateUuid();
      await postImageData(this.uuid, this.imageData).then(() => {
        this.isFetched = true;
        window.history.pushState(null, null, `/nurie/${this.uuid}`);
      });
    },
    async OpenFBModal() {
      this.FBModalFlag = true;
      this.uuid = this.generateUuid();
      await postImageData(this.uuid, this.imageData).then(() => {
        this.isFetched = true;
        window.history.pushState(null, null, `/nurie/${this.uuid}`);
      });
    },
    CloseModal() {
      this.twitterModalFlag = false;
      this.FBModalFlag = false;
      this.isFetched = false;
      window.history.pushState(null, null, "/");
    },
    setImage(e) {
      this.file = e.target.files[0];
      if(this.file) {
        if (this.file.size > 2 * 1000 * 1000) {
          this.tooBig = true;
          return;
        }
        this.changedImageUrl = "";
        const reader = new FileReader();
        reader.addEventListener("load", () => {
          this.uploadImageUrl = reader.result;
        });
        reader.readAsDataURL(this.file);
      }
    },
    getArt(number) {
      this.getResult(this.file, number);
    },
    generateUuid() {
      let chars = "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".split("");
      for (let i = 0, len = chars.length; i < len; i++) {
        switch (chars[i]) {
          case "x":
            chars[i] = Math.floor(Math.random() * 16).toString(16);
            break;
          case "y":
            chars[i] = (Math.floor(Math.random() * 4) + 8).toString(16);
            break;
        }
      }
      return chars.join("");
    },
    async getResult(file, style) {
      this.overlay = true;
      try {
        this.imageData = await getArtImage(file, style);
        this.changedImageUrl = "data:image/png;base64," + this.imageData;
        this.uploadImageUrl = "";
        this.overlay = false;
      } catch (error) {
        console.error(error);
        this.overlay = false;
      }
    },
  },
});
</script>
<style>
.gradient {
  background: linear-gradient(90deg, #38382e 0%, #c19a7d 100%);
}
.overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translateY(-50%) translateX(-50%);
  -webkit-transform: translateY(-50%) translateX(-50%);
  margin: auto;
}
.imageClass {
  height: 350px;
}
.modalImageClass {
  max-height: 350px;
}
</style>
