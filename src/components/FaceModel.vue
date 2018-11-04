<template>
<div class="FaceModel">
  <canvas id="myCanvas"></canvas>
</div>
</template>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/89/three.min.js"></script>
<script>
  import * as THREE from 'three';
  export default {
    name: 'FaceModel',
    data () {
      return {
      }
    }
  }

  // ページの読み込みを待つ
  window.addEventListener('load', init);

  // サイズを指定
  const width = 460;
  const height = 540;

  function init() {
    // レンダラーを作成
    const renderer = new THREE.WebGLRenderer({
      canvas: document.querySelector('#myCanvas'),
      alpha: true
    });
    renderer.setSize(width, height);
    renderer.setClearAlpha(0);

    // シーンを作成
    const scene = new THREE.Scene();

    // カメラを作成
    const camera = new THREE.PerspectiveCamera(75, width / height, 1, 10000);
    camera.position.set(0, 0, +1000);

    // 球体を作成
    const geometry = new THREE.SphereGeometry(300, 30, 30);
    // マテリアルにテクスチャーを設定
    const material = new THREE.MeshStandardMaterial({
      map: new THREE.TextureLoader().load('https://cdn.shopify.com/s/files/1/1061/1924/products/Confused_Face_Emoji_Icon_ios10_large.png?v=1513251032')
    });
    // メッシュを作成
    const mesh = new THREE.Mesh(geometry, material);
    // 3D空間にメッシュを追加
    scene.add(mesh);

    // 平行光源
    const directionalLight = new THREE.DirectionalLight(0xFFFFFF);
    directionalLight.position.set(1, 1, 1);
    // シーンに追加
    scene.add(directionalLight);

    tick();

    // 毎フレーム時に実行されるループイベントです
    function tick() {
      // メッシュを回転させる
      mesh.rotation.x += 0.00;
      mesh.rotation.y += 0.03;
      // レンダリング
      renderer.render(scene, camera);

      requestAnimationFrame(tick);
    }
  }
</script>
