<template>
  <br />
  <center>
    <button
      @click="bukakamera"
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
    >
      Buka Kamera
    </button>
    <button
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
    >
      Tutup Kamera
    </button>
    <br /><br />
    <img :src="imgs" alt="Screenshot" />
    <video
      ref="videoElement"
      autoplay
      style="border-radius: 20px; height: 10%"
    ></video>
    <br />
    <button
      @click="sekrinsut"
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
    >
      Sekrinsut
    </button>
    <br /><br />

    <button
      @click="ulangi"
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
    >
      Ulangi
    </button>
    {{ ulangis }}
  </center>
</template>

<script>
export default {
  data() {
    return { stream: null, imgs: null };
  },
  methods: {
    bukakamera() {
      this.opencamera = false;
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
    tutupkamera() {
      if (this.stream) {
        this.stream.getTracks().forEach((track) => track.stop());
        this.$refs.videoElement.srcObject = null;
      }
    },
    sekrinsut() {
      const video = this.$refs.videoElement;
      const canvas = document.createElement("canvas");
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;
      const context = canvas.getContext("2d");
      context.drawImage(video, 0, 0, canvas.width, canvas.height);
      const dataURL = canvas.toDataURL("image/png");
      this.imgs = dataURL;
      this.tutupkamera();
    },
    ulangi() {
      this.bukakamera(); // This method reopens the camera.
    },
  },
};
</script>
