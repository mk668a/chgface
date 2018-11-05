<template>
<div class="Camera">
  <video autoplay playsinline muted id="webcam" width="250" height="250"></video>
</div>
</template>

<style>
.Camera {
  margin: 10px auto;
}

.Camera video {
  border: 1px solid #c1c1c1;
  border-radius: .5em;
  box-shadow: 20px 20px 20px rgba(0,0,0,0.2);
}
</style>

<script>
import {
  Webcam
} from '../assets/webcam'

export default {
  name: "Camera",
  data: function() {
    return {
      webcam: null,
    }
  },
  mounted: function() {
    this.loadWebcam();
  },
  methods: {
    loadWebcam: function() {
      this.webcam = new Webcam(document.getElementById('webcam'));
      this.webcam.setup();
    }
  }
};
</script>
