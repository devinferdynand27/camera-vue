<template>
  <center>
    <div style="width: 80%">
      <br /><br />
      <center>
        <button
          v-if="cameraOpen === false"
          @click="bukaKamera"
          style="width: 64%"
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
        >
          Buka Kamera
        </button>
        <button
          v-else
          style="width: 64%"
          @click="tutupKamera"
          class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"
        >
          Tutup Kamera
        </button>
        <br /><br />
        <img v-if="ulangis === false" :src="imgSrc" />
        <img v-else :src="imgSrc" /><br />

        <video
          ref="videoElement"
          autoplay
          style="border-radius: 20px; transform: scaleX(-1)"
        ></video>
        <br />

        <div v-if="cameras == true">
          <span>
            <button
              v-if="ambilscrinsut == false"
              @click="ambilScreenshot"
              style="width: 32%"
              class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
            >
              Ambil Screenshot
            </button>
            &nbsp;
            <button
              @click="ulangi"
              style="width: 32%"
              class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
            >
              Ulangi
            </button>
          </span>
        </div>
      </center>
    </div>
  </center>
</template>

<script>
export default {
  data() {
    return {
      stream: null,
      cameras: false,
      imgSrc: null,
      cameraOpen: false,
      ulangis: false,
      ambilscrinsut: false,
    };
  },
  methods: {
    bukaKamera() {
      this.imgSrc = null;
      this.cameras = true;
      this.cameraOpen = true;
      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then((stream) => {
          this.stream = stream;
          this.$refs.videoElement.srcObject = stream;
        })
        .catch((error) => {
          alert("Error accessing camera:", error);
        });
    },
    tutupKamera() {
      if (this.stream) {
        this.stream.getTracks().forEach((track) => track.stop());
        this.$refs.videoElement.srcObject = null;
      }
      this.cameraOpen = false;
      this.cameras = false;
    },
    ambilScreenshot() {
      const video = this.$refs.videoElement;
      const canvas = document.createElement("canvas");
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;
      const context = canvas.getContext("2d");
      context.drawImage(video, 0, 0, canvas.width, canvas.height);
      const dataURL = canvas.toDataURL("image/png");
      this.imgSrc = dataURL;
      this.tutupKamera();
      this.cameras = true;
      this.ambilscrinsut = true;
    },
    ulangi() {
      this.imgSrc = null; // Menghapus gambar yang ada
      this.bukaKamera();
      this.ulangis = true;
      this.ambilscrinsut = false;
    },
  },
};
</script>
