<template>
<div class="main">
  <h1>Change Face</h1>
  <FaceModel></FaceModel>
  <FaceAwareness></FaceAwareness>
  <a>3D model face is changed by your facial expression</a>
</div>
</template>

<script>
import FaceModel from './FaceModel.vue'
import FaceAwareness from './FaceAwareness.vue'

export default {
  name: 'Main',
  data() {
    return {}
  },
  components: {
    FaceModel,
    FaceAwareness
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
h1,
h2 {
  font-weight: bold;
}

.main{
  margin: 10vh 10vw;
}
</style>
