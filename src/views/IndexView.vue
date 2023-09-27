<template>
  <div>
    <center>
      <button
        v-if="cameraOpen === false"
        @click="bukaKamera"
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
      >
        Buka Kamera
      </button>
      <button
        v-else
        @click="tutupKamera"
        class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"
      >
        Tutup Kamera
      </button>

      <img v-if="ulangis === false" :src="imgSrc" alt="Screenshot" />
      <img v-else :src="imgSrc" alt="Screenshot" />
      <video ref="videoElement" autoplay></video>

      <button
        @click="ambilScreenshot"
        class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
      >
        Ambil Screenshot
      </button>

      <button
        @click="ulangi"
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
      >
        Ulangi
      </button>
    </center>
  </div>
</template>

<script>
export default {
  data() {
    return { stream: null, imgSrc: null, cameraOpen: false, ulangis: false };
  },
  methods: {
    bukaKamera() {
      this.cameraOpen = true;
      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then((stream) => {
          this.stream = stream;
          this.$refs.videoElement.srcObject = stream;
        })
        .catch((error) => {
          console.error("Error accessing camera:", error);
        });
    },
    tutupKamera() {
      if (this.stream) {
        this.stream.getTracks().forEach((track) => track.stop());
        this.$refs.videoElement.srcObject = null;
      }
      this.cameraOpen = false;
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
    },
    ulangi() {
      this.imgSrc = null; // Menghapus gambar yang ada
      this.bukaKamera();
      this.ulangis = true;
    },
  },
};
</script>
