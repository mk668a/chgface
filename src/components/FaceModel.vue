<template>
<div class="FaceModel">
  <img :src="imgface" />
</div>
</template>

<style>
.FaceModel {
  margin-right: 30px;
  margin-left: auto;
  width: auto;
}
</style>

<script>
import * as THREE from 'three';

export default {
  name: 'FaceModel',
  data() {
    return {
      imgface: require("../assets/neutral.jpg")
    }
  }
}
</script>
