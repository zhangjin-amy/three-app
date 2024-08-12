<template>
  <div ref="threeContainer" class="three-container"></div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import * as THREE from 'three';

const threeContainer = ref(null);

onMounted(() => {
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
  const renderer = new THREE.WebGLRenderer();

  renderer.setSize(window.innerWidth, window.innerHeight);
  threeContainer.value.appendChild(renderer.domElement);

  const geometry = new THREE.BoxGeometry();
  const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
  const cube = new THREE.Mesh(geometry, material);
  scene.add(cube);

  // 添加环境光
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.5); // 白色光，强度为0.5
  scene.add(ambientLight);

  // 添加点光源
  const pointLight = new THREE.PointLight(0xffffff, 1); // 白色光，强度为1
  pointLight.position.set(5, 5, 5); // 设置点光源的位置
  scene.add(pointLight);

  camera.position.z = 5;

  const animate = function () {
    requestAnimationFrame(animate);

    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;

    renderer.render(scene, camera);
  };

  animate();
});
</script>

<style>
.three-container {
  width: 100%;
  height: 100vh;
  overflow: hidden;
}
</style>