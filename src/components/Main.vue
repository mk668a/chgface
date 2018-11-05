<template>
<div class="main">
  <FaceAwareness></FaceAwareness>
</div>
</template>

<style>
h1,
h2 {
  font-weight: bold;
  margin: 0;
}
</style>

<script>
import FaceAwareness from '@/components/FaceAwareness.vue'

export default {
  name: 'Main',
  components: {
    FaceAwareness
  }
}
</script>
