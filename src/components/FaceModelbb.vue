<template>
<div class="FaceModelbb">
  <!-- <div ref="stage"></div> -->
  <div id="container"></div>
  <img :src="imgface" />
</div>
</template>

<!-- <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/89/three.min.js"></script> -->
<!-- <script src="./three.min.js"></script> -->
<script>
import * as THREE from 'three';

export default {
  name: 'FaceModelbb',
  data() {
    return {
      renderer: null,
      camera: null,
      scene: null,
      mesh: null,
      imgface: require("../assets/face.jpg")

      // imgface: 'http://planetpixelemporium.com/images/mappreviews/earthmapthumb.jpg'
    }
  },
  methods: {
    async init() {
      const container = document.getElementById('container')
      // シーンを作成
      this.scene = new THREE.Scene();

      // レンダラーを作成
      this.renderer = new THREE.WebGLRenderer({
        alpha: true
      });

      this.renderer.setSize(460, 540);
      this.renderer.setClearAlpha(0);

      // カメラを作成
      this.camera = new THREE.PerspectiveCamera(75, 460 / 540, 1, 10000);
      this.camera.position.set(0, 0, +1000);

      // 球体を作成
      const geometry = new THREE.SphereGeometry(300, 30, 30);
      // マテリアルにテクスチャーを設定
      console.log(this.imgface);
      const material = new THREE.MeshStandardMaterial({
        color: 0xEF6C00
        // map: new THREE.TextureLoader().load(this.imgface)
        // map: new THREE.TextureLoader().load('https://static.wixstatic.com/media/9e6285_4d7ccb6dc7ed4271bddb5d2e4de70310~mv2.jpg')
      });
      // メッシュを作成
      this.mesh = new THREE.Mesh(geometry, material);
      // 3D空間にメッシュを追加
      this.scene.add(this.mesh);
      // 平行光源
      // const directionalLight = new THREE.DirectionalLight(0xFFFFFF);
      // this.directionalLight.position.set(1, 1, 1);
      // シーンに追加
      // this.scene.add(this.directionalLight);
      container.appendChild(this.renderer.domElement)
    },
    async animate() {
      this.mesh.rotation.x += 0.00;
      this.mesh.rotation.y += 0.03;
      // レンダリング
      this.renderer.render(this.scene, this.camera);

      requestAnimationFrame(this.animate);
    }
  },
  created() {
    // ページの読み込みを待つ
    // window.addEventListener('load', init);
  },
  mounted() {
    this.init()
    this.animate()
    // this.$refs.stage.appendChild(this.renderer.domElement);
  }
}
</script>
